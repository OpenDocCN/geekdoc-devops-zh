# 使用 Gradle | CircleCI 将 Android 库持续部署到 Maven Central

> 原文：<https://circleci.com/blog/publishing-java-android-libraries/>

本文将带您使用 Gradle 为构建、测试和发布库到 Maven Central 设置 CI/CD 集成。随着 jCenter 的关闭，Maven Central 再次成为所有 Android 和 Java 库的主要目的地。在 jCenter 关闭后，库发布者需要将他们的库移植到 Maven Central，以保持他们的库可用。

本文主要关注 CI/CD 集成。为手动发布设置库本身不在讨论范围之内，但是您可以找到一些有用的文章和入门指南的链接。

## 先决条件

要从这篇文章中获得最大的收获，您需要几样东西:

*   Android 或 Java 的一些经验
*   与格拉德的经历
*   熟悉 Java 或 Android 库发布流程
*   一个[圆](https://circleci.com/signup)的账户

## 关于项目

[样本库](https://github.com/zmarkan/Android-Espresso-ScrollableScroll)是我在 2016 年发布的测试工具。相关部分都在 Gradle 构建脚本和 CircleCI 配置:`.circleci/config.yaml`中。

有两个模块:

*   库模块`scrollableScroll`是正在发布的库
*   Android 应用模块`sample`是我们用来测试库的应用

注意: *单独测试一个库有些困难。*

## 使用 Gradle 将库发布到 Maven Central

如果您以前没有使用 Gradle to Maven Central 发布过库，这一节是为您准备的。如果你做到了这一点，你可以跳到下一节。

Maven Central 或者也叫 Sonatype OSSRH(OSS Repository Hosting)提供了一个[官方指南](https://central.sonatype.org/publish/publish-guide/)。

该过程需要几个步骤:

1.  **注册一个`group`来发布。**这可以是你的包名；对于我的项目，我使用了`com.zmarkan`。您需要创建一个吉拉帐户，并为 Sonatype Nexus 开一张票，以便为您创建一个群组。您将在该组下发布您的所有项目。请注意，如果您使用`com.[domain name]`命名法，您可能会被要求证明您拥有该域名。
2.  **准备你的图书馆出版和签名。** Gradle 有`maven-publish`和`signing`插件可以帮助解决这个问题。有几篇[文章](https://proandroiddev.com/publishing-your-first-android-library-to-mavencentral-be2c51330b88)涵盖了这一过程。也有官方的分级指南——出版指南[、签约指南](https://docs.gradle.org/current/userguide/publishing_maven.html)和签约指南[。](https://docs.gradle.org/current/userguide/signing_plugin.html#using_in_memory_ascii_armored_openpgp_subkeys)
3.  创建一个 [GPG 键](https://www.gnupg.org/gph/en/manual/c14.html)。这是你用来签署包裹的东西。

您可以在 GitHub 上的[我的示例项目中找到完整的示例。](https://github.com/zmarkan/Android-Espresso-ScrollableScroll)

如果你已经手动设置好了一切，你应该让`./gradlew assemble publish`开始工作，这意味着你已经准备好了。手动发布过程还要求您登录到 web 上的 Maven Central，并且您需要手动关闭和发布在 Gradle publish 任务期间创建的临时存储库。

## 使用 Gradle 持续部署库

当然，上一节描述的过程适用于本地发布。在 CI/CD 环境中，这将略有不同。首先，我们不想执行所有这些手动任务。我使用的是 Gradle Nexus 发布插件，它建立在`maven-publish`配置的基础上，可以自动完成发布的最后一部分。

其次，我们需要确保所有的密钥都被安全地存储，并且在为*签名*的 CI/CD 过程中是可访问的。正确的*版本*也必须到位。

### 使用 Gradle Nexus 发布插件自动发布

这个 Gradle 插件提供了两个新的 Gradle 任务来代替现有的`publish` Gradle 任务:

*   `publishToSonatype`
*   `closeAndReleaseSonatypeStagingRepository`

要包含插件，将其添加到项目级`build.gradle`文件:

```
plugins {
    id "io.github.gradle-nexus.publish-plugin" version "1.0.0"
}

group = 'com.zmarkan' //Or whatever your Nexus publishing group is
version = findProperty('LIBRARY_VERSION')

// Rest of the setup ...

nexusPublishing {
    repositories {
        sonatype {  
            //only for users registered in Sonatype after 24 Feb 2021
            nexusUrl.set(uri("https://s01.oss.sonatype.org/service/local/"))
            snapshotRepositoryUrl.set(uri("https://s01.oss.sonatype.org/content/repositories/snapshots/"))
            username = findProperty('OSSRH_USERNAME')
            password = findProperty('OSSRH_PASSWORD')
        }
    }
} 
```

在本例中，`username`和`password`值是我的 Sonatype OSSRH 凭证，与 web 界面和吉拉问题跟踪器相同。如果您不想使用自己的凭证，也可以在 [Sonatype OSSRH web UI](https://s01.oss.sonatype.org/#profile;User%20Token) 中创建单独的令牌。

您需要将凭证存储在您的 [CircleCI 环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)中，命名为`ORG_GRADLE_PROJECT_OSSRH_USERNAME`和`ORG_GRADLE_PROJECT_OSSRH_PASSWORD`。

**注意:** *务必保留`ORG_GRADLE_PROJECT_`前缀。如果没有前缀，Gradle 中的`findProperty`查找将不起作用。*

### 部署流程

库的 CI/CD 管道做了几件事:

*   在每次提交时测试库。库测试在相应的示例应用程序上运行。这将尽快捕获任何会破坏构建的提交
*   **从`main`分支释放一个*快照*。**世界协调时每天午夜都会发生这种情况。它允许通过指向快照存储库来测试最新版本
*   在每个标签上发布一个新版本。这是大多数最终用户在使用该库时会与之交互的版本

建立一个 CI/CD 管道来发布你的库意味着你的发布过程将会一直工作，不管你的开发机器发生了什么，或者谁在做开发。您还将保护所有凭据或签名密钥的安全。

设置签名密钥是这个过程中最复杂的部分，所以我们应该开始了。

### 签名密钥

在 CI/CD 环境中对项目进行签名需要一个签名密钥，您可以在构建时将它传递给 Gradle。为此，您需要以字符串格式导出密钥，并将密钥本身及其密码存储在 CircleCI 的环境变量中。最后，您需要在构建时将密钥本身注入到项目的`gradle.properties`中。

这意味着在你的库模块的`build.gradle`中，你可以使用`useInMemoryPgpKeys`函数来代替。这个函数使用基于字符串的`signingKey`而不是密匙环文件。这两个键都可以存储为 Gradle 属性或环境变量:

```
def signingKey = findProperty('SIGNING_KEY')
def signingKeyPwd = findProperty('SIGNING_KEY_PWD')

signing {
    useInMemoryPgpKeys(signingKey, signingKeyPwd)
    sign publishing.publications
} 
```

当您运行命令导出签名密钥时，请用用于生成密钥的电子邮件替换 your_email@example.com。您将被要求提供创建签名密钥时使用的密码。

```
gpg --armor --export-secret-keys zan@circleci.com \
    | awk -v ORS='\\n' '1' \
    | pbcopy 
```

该命令的第一部分`gpg --armor --export-secret-keys`将您的密钥导出为 ASCII 字符串。`awk`命令用`\n`替换任何换行符，`pbcopy`将它存储在剪贴板中进行粘贴。

导出的密钥如下所示(注意中间大量的乱码行):

```
-----BEGIN PGP PRIVATE KEY BLOCK-----

[hundreds of lines of crypto gibberish]

-----END PGP PRIVATE KEY BLOCK----- 
```

然后，您可以在项目的环境设置中创建新的 CircleCI 签名变量`GPG_SIGNING_KEY`时粘贴它:

![Setting the GPG key as environment variable](img/4e66cfe7aa9b689b36f8e950824f34f8.png)

您还需要添加该密钥的密码，作为用于签名的环境变量:`ORG_GRADLE_PROJECT_SIGNING_KEY_PWD`。注意，如果你想在 Gradle 中用`findProperty`直接使用密码，前缀`ORG_GRADLE_PROJECT_`是必需的。

至于键本身，我发现 Gradle 不喜欢它被存储为环境变量。我不得不在编译之前用`.circleci/config.yml`中的这个运行调用将它写入`gradle.properties`:

```
- run:
    name: Inject Maven signing key
    command: |
      echo $GPG_SIGNING_KEY \
        | awk 'NR == 1 { print "SIGNING_KEY=" } 1' ORS='\\n' \
        >> gradle.properties 
```

### 用 Git 标签对库进行版本控制

Maven Central 中的所有项目都需要进行正确的版本控制。我喜欢使用 Git 标签来驱动版本控制，因为我们可以从命令行创建它们，并且它们总是与提交绑定在一起。

对于任何带标签的提交，CircleCI 会将标签作为环境变量`$CIRCLE_TAG`出现。我用那个标签来表示一个发布版本的版本。对于所有其他缺少标记的提交，我将它们视为快照构建。

我们将把它存储为另一个环境变量`$ORG_GRADLE_PROJECT_LIBRARY_VERSION`,可以在 Gradle 中获取。使其可用的方法是在 CircleCI 的`run`步骤中将任何变量推送到`$BASH_ENV`。

这是我用的剧本。它或者将当前的 Git 标签设置为环境变量(例如`1.0.21`)，或者如果它丢失了，则使用最近使用的标签并在标签后附加`-SNAPSHOT`。

```
- run:
    name: Define ORG_GRADLE_PROJECT_LIBRARY_VERSION Environment Variable at Runtime
    command: |
      if [ $CIRCLE_TAG ]
        then
          echo 'export ORG_GRADLE_PROJECT_LIBRARY_VERSION=$CIRCLE_TAG' >> $BASH_ENV
        else
          echo "export ORG_GRADLE_PROJECT_LIBRARY_VERSION=`git tag | tail -1`-SNAPSHOT" >> $BASH_ENV
      fi
      source $BASH_ENV 
```

### 将工件发布到 Sonatype OSSRH

发布的最后一步是在运行`assemble`之后立即运行两个 Gradle publishing 命令:

*   `publishToSonatype`
*   `closeAndReleaseSonatypeStagingRepository`

这将使用 Sonatype Staging 插件部署库及其所有工件，并将其发布到产品中。

```
- run:
    name: Publish to Maven Central
    command: ./gradlew assemble publishToSonatype closeAndReleaseSonatypeStagingRepository 
```

我的`.circleci/config.yml`中的完整部署工作:

```
jobs:
  ...
  deploy-to-sonatype:
    executor:
      name: android/android-machine
      resource-class: xlarge
    steps:
      - checkout
      - run:
          name: Define ORG_GRADLE_PROJECT_LIBRARY_VERSION Environment Variable at Runtime
          command: |
            if [ $CIRCLE_TAG ]
              then
                echo 'export ORG_GRADLE_PROJECT_LIBRARY_VERSION=$CIRCLE_TAG' >> $BASH_ENV
              else
                echo "export ORG_GRADLE_PROJECT_LIBRARY_VERSION=`git tag | tail -1`-SNAPSHOT" >> $BASH_ENV
            fi
            source $BASH_ENV
      - run:
          name: Inject Maven signing key
          command: |
            echo $GPG_SIGNING_KEY \
              | awk 'NR == 1 { print "SIGNING_KEY=" } 1' ORS='\\n' \
              >> gradle.properties
      - run:
          name: Publish to Maven
          command: ./gradlew assemble publishToSonatype closeAndReleaseSonatypeStagingRepository 
```

要让 CircleCI 运行此作业，您需要将其包含在工作流中。

### 发布发布版本

我的发布工作流程如下所示:

```
workflows:
  build-test-deploy:
    jobs:
      - android/run-ui-tests:
          name: build-and-test
          system-image: system-images;android-23;google_apis;x86
          test-command: ./gradlew assemble sample:connectedDebugAndroidTest
          filters:
            tags:
              only: /^[0-9]+.*/
      - hold-for-approval:
          type: approval
          requires:
            - build-and-test
          filters:
            tags:
              only: /^[0-9]+.*/
            branches:
              ignore: /.*/
      - deploy-to-sonatype:
          name: Deploy to Maven Central
          requires:
            - hold-for-approval
          filters:
            tags:
              only: /^[0-9]+.*/ 
```

最佳实践是将测试设置为在任何分支的每一次提交时运行，但是只有在提交被标记为版本时才继续部署。这是第`filters`节所允许的。在这种情况下，部署作业只在以数字和句点开头的版本化标记上运行，比如`1.something`。这样，您仍然可以将标签用于其他目的，但是它们不会触发发布版本。

```
filters:
  tags:
    only: /^[0-9]+.*/
  branches:
    ignore: /.*/ 
```

我还添加了一个`hold-for-approval`，这是一个`approval`作业，需要在部署库之前进行手动确认。这是可选的，但我发现手动步骤令人放心。

### 发布快照

快照是特殊类型的发布，发布速度更快，而且很容易被覆盖，不像常规的版本发布，不能被覆盖。这使得快照对于测试新版本的库非常有用。

要发布快照，您需要做的就是将`-SNAPSHOT`附加到版本字符串的末尾。该过程将在本文的版本控制一节中介绍。我已经设置了我的快照部署管道，以便使用 CircleCI 的预定触发器特性在世界协调时的每晚午夜构建库:

```
 nightly-snapshot:
    triggers: #use the triggers key to indicate a scheduled build
      - schedule:
          cron: "0 0 * * *" # use cron syntax to set the schedule
          filters:
            branches:
              only:
                - main
    jobs:
      - android/run-ui-tests:
          name: build-and-test
          system-image: system-images;android-23;google_apis;x86
          test-command: ./gradlew assemblesample:connectedDebugAndroidTest
      - deploy-to-sonatype:
          name: Deploy Snapshot to Sonatype
          requires:
            - build-and-test 
```

## 结论

在本文中，我介绍了如何使用 Gradle 为 Maven Central 的 Android 或 Java 库建立一个持续的部署管道。您已经学习了如何设置库签名的秘密，如何使用 Git 标记对库进行版本控制，以及如何在每次发布新标记时自动发布库。我希望这对您自己的项目有用。

如果你对我接下来应该报道的话题有任何反馈或建议，请通过 [Twitter - @zmarkan](https://twitter.com/zmarkan/) 联系我。