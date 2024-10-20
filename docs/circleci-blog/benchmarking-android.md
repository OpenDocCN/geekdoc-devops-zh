# CI/CD 管道中的 Android 应用基准测试| CircleCI

> 原文：<https://circleci.com/blog/benchmarking-android/>

加载速度快、交互流畅的高性能应用已经成为必需。这些品质是用户所期望的，而不仅仅是一个好东西。确保顺利交互的方法是将性能验证作为发布流程的一部分。

谷歌最近发布了对他们的 [Jetpack 基准库](https://developer.android.com/studio/profile/benchmarking-overview)的更新。一个值得注意的新增功能是 Macrobenchmark 库。这让你可以测试你的应用在启动和滚动等方面的性能。在本教程中，您将从 Jetpack 基准库开始，并学习如何将其作为 CI/CD 流的一部分来实现。

## 先决条件

要从本教程中获得最大收益，您需要几样东西:

*   有 Android 开发和测试经验，包括仪器测试
*   与格拉德的经历
*   一个免费的 CircleCI 账户
*   Firebase 和谷歌云平台账户
*   安卓工作室北极狐

**注:** *我写这个教程用的版本是 2020.3.1 Beta 4。在撰写本文时(2021 年 7 月)，Android Studio 的当前稳定版本不支持 Macrobenchmark。*

## 关于项目

该项目基于我为一篇关于在 CI/CD 管道中测试 Android 应用的博客文章创建的早期测试样本。

我已经扩展了示例项目的[,将`benchmark`任务作为 CI/CD 构建的一部分。这个新作业使用 Jetpack Macrobenchmark 库运行应用程序启动基准。](https://github.com/CircleCI-Public/android-testing-circleci-examples)

## Jetpack 基准测试

Android Jetpack 提供了两种基准测试:微基准测试和宏基准测试。自 2019 年以来一直存在的微基准测试允许对应用程序代码进行性能测量(想想可能需要一段时间处理的缓存或类似过程)。

Macrobenchmark 是 Jetpack 的新成员。它允许你在应用程序启动和滚动等容易注意到的地方测量应用程序的整体性能。示例应用程序使用这些宏基准测试来测量应用程序的启动。

基准测试库和方法都与运行在连接设备和仿真器上的熟悉的 Android 工具框架一起工作。

### 建立图书馆

在官方的 Jetpack 网站上有详细的库设置文档[-macro benchmark 设置](https://developer.android.com/studio/profile/macrobenchmark#setup)。注意:我们不会详细介绍所有步骤，因为它们可能会在未来的预览版中发生变化。相反，下面是该过程的概述:

*   创建一个名为`macrobenchmark`的新测试模块。在 Android Studio 中创建一个 Android 库模块，并将`build.gradle`改为使用`com.android.test`作为插件。新模块需要最低 SDK 级别的 API 29: Android 10 (Q)。
*   对新模块的`build.gradle`文件进行一些修改。将测试实现更改为 implementation，指向您想要测试的 app 模块，并使发布构建类型为`debuggable`。
*   将 profileable 标签添加到您的应用程序的`AndroidManifest`。
*   指定本地版本签名配置。您可以使用现有的`debug`配置。

完整的分步说明请参考官方 Macrobenchmark 库文档中的指南:https://developer . Android . com/studio/profile/macro benchmark # setup。

### 编写和执行宏基准测试

Macrobenchmark 库引入了一些新的 JUnit 规则和指标。

我们使用的`StartupTimingMetric`取自 GitHub 上的[性能样本项目。](https://github.com/android/performance-samples/tree/main/MacrobenchmarkSample/macrobenchmark)

```
const val TARGET_PACKAGE = "com.circleci.samples.todoapp"

fun MacrobenchmarkRule.measureStartup(
    profileCompiled: Boolean,
    startupMode: StartupMode,
    iterations: Int = 3,
    setupIntent: Intent.() -> Unit = {}
) = measureRepeated(
    packageName = TARGET_PACKAGE,
    metrics = listOf(StartupTimingMetric()),
    compilationMode = if (profileCompiled) {
        CompilationMode.SpeedProfile(warmupIterations = 3)
    } else {
        CompilationMode.None
    },
    iterations = iterations,
    startupMode = startupMode
) {
    pressHome()
    val intent = Intent()
    intent.setPackage(TARGET_PACKAGE)
    setupIntent(intent)
    startActivityAndWait(intent)
}

@LargeTest
@RunWith(Parameterized::class)
class StartupBenchmarks(private val startupMode: StartupMode) {

    @get:Rule
    val benchmarkRule = MacrobenchmarkRule()

    @Test
    fun startupMultiple() = benchmarkRule.measureStartup(
        profileCompiled = false,
        startupMode = startupMode,
        iterations = 5
    ) {
        action = "com.circleci.samples.target.STARTUP_ACTIVITY"
    }

    companion object {
        @Parameterized.Parameters(name = "mode={0}")
        @JvmStatic
        fun parameters(): List<Array<Any>> {
            return listOf(StartupMode.COLD, StartupMode.WARM, StartupMode.HOT)
                .map { arrayOf(it) }
        }
    }
} 
```

上面的代码将 3 种类型的启动度量作为参数化选项:热启动、热启动和冷启动。使用`MacroBenchmarkRule`将这些选项传递给测试(意味着应用程序最近运行了多久，以及它是否被保存在内存中)。

### 在 CI/CD 管道中运行基准测试

你可以在 Android Studio 中运行这些样本，它会给你一个很好的应用程序性能指标的打印输出，但不会做任何事情来确保你的应用程序总是高性能的。为此，您需要在 CI/CD 流程中集成基准。

所需的步骤是:

*   构建 app 和 Macrobenchmark 模块的发布版本
*   在 Firebase 测试实验室(FTL)或类似工具上运行测试
*   下载基准测试结果
*   将基准存储为工件
*   处理基准测试结果以获得计时数据
*   基于结果通过或不通过构建

我创建了一个新的`benchmark-ftl`作业来运行我的测试:

```
orbs:
  android: circleci/android@1.0.3
  gcp-cli: circleci/gcp-cli@2.2.0

...

jobs:
  ...
    benchmarks-ftl:
    executor:
      name: android/android
      sdk-version: "30"
      variant: node
    steps:
      - checkout
      - android/restore-gradle-cache
      - android/restore-build-cache
      - run:
          name: Build app and test app
          command: ./gradlew app:assembleRelease macrobenchmark:assemble
      - gcp-cli/initialize:
          gcloud-service-key: GCP_SA_KEY
          google-project-id: GCP_PROJECT_ID
      - run:
          name: run on FTL
          command: |
            gcloud firebase test android run \
              --type instrumentation \
              --app app/build/outputs/apk/release/app-release.apk \
              --test macrobenchmark/build/outputs/apk/release/macrobenchmark-release.apk \
              --device model=flame,version=30,locale=en,orientation=portrait \
              --directories-to-pull /sdcard/Download \
              --results-bucket gs://android-sample-benchmarks \
              --results-dir macrobenchmark \
              --environment-variables clearPackageData=true,additionalTestOutputDir=/sdcard/Download,no-isolated-storage=true
      - run:
          name: Download benchmark data
          command: |
            mkdir ~/benchmarks
            gsutil cp -r 'gs://android-sample-benchmarks/macrobenchmark/**/artifacts/sdcard/Download/*'  ~/benchmarks
            gsutil rm -r gs://android-sample-benchmarks/macrobenchmark
      - store_artifacts:
            path: ~/benchmarks
      - run:
          name: Evaluate benchmark results
          command: node scripts/eval_startup_benchmark_output.js 
```

这个片段代表了在 [Firebase 测试实验室](https://firebase.google.com/docs/test-lab/android/command-line)的真实设备上运行宏基准测试的作业。我们将 Docker executor 与 Android orb 提供的图像一起使用，它安装了 Android SDK 30 并包含 NodeJS。我们将在评估结果的脚本中使用 Node。

在构建了 app 和 macrobenchmark 模块 APKs 之后，我们需要使用 orb 初始化 [Google Cloud CLI。这是我们在 CircleCI 中为 Google Cloud 提供环境变量的地方。然后，我们在 Firebase 测试实验室中运行测试:](https://circleci.com/developer/orbs/orb/circleci/gcp-cli#commands-initialize)

```
- run:
    name: run on FTL
    command: |
      gcloud firebase test android run \
        --type instrumentation \
        --app app/build/outputs/apk/release/app-release.apk \
        --test macrobenchmark/build/outputs/apk/release/macrobenchmark-release.apk \
        --device model=flame,version=30,locale=en,orientation=portrait \
        --directories-to-pull /sdcard/Download \
        --results-bucket gs://android-sample-benchmarks \
        --results-dir macrobenchmark \
        --environment-variables clearPackageData=true,additionalTestOutputDir=/sdcard/Download,no-isolated-storage=true 
```

这将 apk 上传到 Firebase，指定设备(在我们的例子中是 Pixel 4)，为测试提供环境变量，并指定存储结果的云存储桶。我们总是把它放在`macrobenchmark`目录中，以便于获取。我们不需要安装用于运行测试的`gcloud`工具；它与 CircleCI Android Docker 映像捆绑在一起。

一旦作业终止，我们需要使用与 GCP CLI 工具捆绑在一起的`gsutil`工具下载基准数据:

```
- run:
    name: Download benchmark data
    command: |
      mkdir ~/benchmarks
      gsutil cp -r 'gs://android-sample-benchmarks/macrobenchmark/**/artifacts/sdcard/Download/*'  ~/benchmarks
      gsutil rm -r gs://android-sample-benchmarks/macrobenchmark
- store_artifacts:
    path: ~/benchmarks 
```

这将创建一个`benchmarks`目录，并将基准从云存储中复制到其中。复制完文件后，我们还删除了存储桶中的`macrobenchmark`目录，以避免它与之前的跟踪文件混淆。例如，您也可以将文件保留在 Google Cloud Storage 中，而不是根据作业 ID 构造目录名。

### 评估基准结果

如果基准测试已经完成，基准测试命令将成功终止。不过，它不会给你任何迹象表明他们实际表现如何。为此，您需要分析基准测试的结果，并自己决定测试是失败还是通过。

结果被导出到一个 JSON 文件中，该文件包含每个测试的最小、最大和中间计时。我编写了一个简短的 Node.js 脚本来比较这些基准，如果它们中的任何一个超出了我的预期时间，就会失败。Node.js 非常适合处理 JSON 文件，并且在 CircleCI 机器映像上很容易获得。

运行该脚本只是一个命令:

```
- run:
    name: Evaluate benchmark results
    command: node scripts/eval_startup_benchmark_output.js 
```

```
const benchmarkData = require('/home/circleci/benchmarks/com.circleci.samples.todoapp.macrobenchmark-benchmarkData.json')

const COLD_STARTUP_MEDIAN_THRESHOLD_MILIS = YOUR_COLD_THRESHOLD
const WARM_STARTUP_MEDIAN_THRESHOLD_MILIS = YOUR_WARM_THRESHOLD
const HOT_STARTUP_MEDIAN_THRESHOLD_MILIS = YOUR_HOT_THRESHOLD

const coldMetrics = benchmarkData.benchmarks.find(element => element.params.mode === "COLD").metrics.startupMs
const warmMetrics = benchmarkData.benchmarks.find(element => element.params.mode === "WARM").metrics.startupMs
const hotMetrics = benchmarkData.benchmarks.find(element => element.params.mode === "HOT").metrics.startupMs

let err = 0
let coldMsg = `Cold metrics median time - ${coldMetrics.median}ms `
let warmMsg = `Warm metrics median time - ${warmMetrics.median}ms `
let hotMsg = `Hot metrics median time - ${hotMetrics.median}ms `

if(coldMetrics.median > COLD_STARTUP_MEDIAN_THRESHOLD_MILIS){
    err = 1
    console.error(`${coldMsg} ❌ - OVER THRESHOLD ${COLD_STARTUP_MEDIAN_THRESHOLD_MILIS}ms`)
} else {
    console.log(`${coldMsg} ✅`)
}

if(warmMetrics.median > WARM_STARTUP_MEDIAN_THRESHOLD_MILIS){
    err = 1
    console.error(`${warmMsg} ❌ - OVER THRESHOLD ${WARM_STARTUP_MEDIAN_THRESHOLD_MILIS}ms`)
} else {
    console.log(`${warmMsg} ✅`)
}

if(hotMetrics.median > HOT_STARTUP_MEDIAN_THRESHOLD_MILIS){
    err = 1
    console.error(`${hotMsg} ❌ - OVER THRESHOLD ${HOT_STARTUP_MEDIAN_THRESHOLD_MILIS}ms`)
} else {
    console.log(`${hotMsg} ✅`)
}

process.exit(err) 
```

为了建立计时的阈值，我将我的预期计时建立在之前的基准运行结果的基础上。如果我们引入一些减慢启动速度的代码，比如一个冗长的网络调用，基准评估将会在阈值之上运行，并且构建失败。

要使一个构建失败，我可以调用`process.exit(err)`，它从一个脚本中返回一个非零的状态代码，这导致基准评估工作失败。

这是一个非常简单的基准测试示例。从事 Jetpack 基准测试库工作的人已经写了一些文章，将基准测试与最近的构建进行比较，使用逐步拟合的方法来检测任何回归。你可以在[这篇博文](https://medium.com/androiddevelopers/fighting-regressions-with-benchmarks-in-ci-6ea9a14b5c71)中读到。

使用 CircleCI 的开发人员可以通过使用 circle ci API:https://circle ci . com/docs/API/v2/# operation/getJobArtifacts 从以前的构建中获取历史作业数据来实现类似的分步拟合方法。

## 警告

根据定义，端到端基准测试是不可靠的测试，需要在真实设备上运行。在模拟器上运行不会产生接近用户所看到的结果。为了说明这一点，我创建了一个`benchmarks-emulator`作业来展示计时的不同。

撰写本文时(2021 年 7 月)，基准测试工具仍处于测试阶段。要使用它们，您需要预览 Android Studio(北极狐)版本。插件和库正在积极开发，所以更新一切可能不会像描述的那样工作。

## 结论

在本文中，我介绍了如何将 Android 应用程序性能基准测试与其他测试一起包含在 CI/CD 管道中。这可以防止当您添加新功能和其他改进时，性能退化影响到您的用户。

我们使用了新的 Android Jetpack 宏基准库，并展示了将其与 Firebase Test Lab 集成的方法，以在真实设备上运行基准。我们展示了如何分析结果，如果应用程序启动时间超过我们允许的阈值，则通过构建。

如果你对我接下来应该报道的话题有任何反馈或建议，请通过 [Twitter - @zmarkan](https://twitter.com/zmarkan/) 联系我。

## 资源

[在 CI 中运行基准](https://developer.android.com/studio/profile/run-benchmarks-in-ci)