# 将 Deno APIs 持续部署到 Heroku | CircleCI

> 原文：<https://circleci.com/blog/continuous-deployment-deno/>

我第一次承担维护生产服务器的任务时，我依赖于我的前任留给我的清单。该清单包含所有维护步骤及其相应的命令。在那些早期的日子里，我虔诚地复制每个命令，在按下回车键之前反复检查每个字符。慢慢地，但肯定地，这些命令被记住了，直到有一天我意识到我不需要清单。

但是后来我的任务增加了，我发现我不能足够快地键入命令。我试着用自定义脚本来缩短这个过程，但是没有多大帮助。然后有惊无险开始了，很快手动部署变得更加可怕而不是有趣。这就是手动部署的现实；你总是一个错误的命令就可能导致灾难。寻找更安全、更高效的替代方案让我不断地进行部署。我很快发现，自动化部署过程消除了错误键入命令的风险，以及与人类完成重复任务相关的其他错误。

在本教程和它的同伴中，我将带领你使用 CircleCI 和 Heroku 为一个 [Deno](https://deno.land/) 项目建立一个 [CI/CD 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)。Deno 是一个简单、现代、安全的 JavaScript 和 TypeScript 运行时。Deno 为项目提供了以下优势:

*   Deno 在默认情况下是安全的。除非明确启用，否则没有文件、网络或环境访问权限。
*   支持现成的 TypeScript。
*   仅提供一个可执行文件。

## 先决条件

开始之前，请确保您的系统上安装了以下项目:

*   [Deno](https://deno.land/#installation) 的最新安装。

对于存储库管理和持续集成/持续部署，您需要:

*   一个 [GitHub](https://github.com/) 账户。你可以在这里创建一个[。](https://github.com/join)
*   一个 [CircleCI](https://circleci.com/) 账户。你可以在这里创建一个[。要轻松连接您的 GitHub 项目，您可以注册您的 GitHub 帐户。](https://circleci.com/signup/)
*   一个 Heroku 的账户。你可以在这里创建一个[。](https://signup.heroku.com/login)

## 创建项目目录

创建一个新目录来保存所有项目文件:

```
mkdir deno_circleci_heroku

cd deno_circleci_heroku 
```

## 设置 Deno 服务器

在这一节中，我们将使用 [Oak](https://deno.land/x/oak@v7.3.0) 中间件框架来设置我们的 Deno 服务器。它是 Deno 的 [HTTP 服务器](https://doc.deno.land/https/deno.land/std/http/mod.ts)的一个可以媲美 [Koa](https://koajs.com/) 和 [Express](https://expressjs.com/) 的框架。首先，创建一个名为`server.ts`的文件，并向其中添加以下代码:

```
import { Application, Router } from "https://deno.land/x/oak@v7.5.0/mod.ts";
import { parse } from "https://deno.land/std@0.99.0/flags/mod.ts";

const app = new Application();
const { args } = Deno;

const DEFAULT_PORT = 8000;
const port = parse(args).port ?? DEFAULT_PORT;

const router = new Router();

router.get("/", (context) => {
  context.response.type = "application/json";
  context.response.body = { data: "This API is under construction" };
});

app.addEventListener("error", (event) => {
  console.error(event.error);
});

app.use(router.routes());
app.use(router.allowedMethods());

app.listen({ port });
console.log(`Server is running on port ${port}`);

export default app; 
```

这个例子展示了一个基本的 API，当一个请求被发送到索引路由时，它返回一个“正在构建”的消息。一旦我们的管道设置成功，我们将为这个端点添加实际的功能。

请注意，我们以一种不寻常的方式声明了我们正在监听的端口。这是因为在运行 API 时，端口是由 Heroku 提供的。你可以在这里阅读更多关于这个[的内容。该端口将在`deno run`命令中提供，并将被解析和用于运行应用程序。如果没有指定端口(当应用程序在本地运行时)，将使用端口 8000。](https://devcenter.heroku.com/articles/runtime-principles#web-servers)

## 运行应用程序

我们可以使用以下命令运行应用程序，看看到目前为止我们做了些什么:

```
deno run --allow-net server.ts 
```

导航至`http://localhost:8000/`查看响应。

![API response](img/0763e2b43e19417d5ece11d046da5efb.png)

## 为应用程序编写测试

是时候为我们的 API 端点编写一个测试用例了。创建一个名为`server.test.ts`的新文件，并添加以下内容:

```
import { superoak } from "https://deno.land/x/superoak@4.2.0/mod.ts";
import { delay } from "https://deno.land/x/delay@v0.2.0/mod.ts";

import app from "./server.ts";

Deno.test("it should return a JSON response with status code 200", async () => {
  const request = await superoak(app);
  await request
    .get("/")
    .expect(200)
    .expect("Content-Type", /json/)
    .expect({ data: "This API is under construction" });
});

// Forcefully exit the Deno process once all tests are done.
Deno.test({
  name: "exit the process forcefully after all the tests are done\n",
  async fn() {
    await delay(1);
    Deno.exit(0);
  },
  sanitizeExit: false,
}); 
```

## 在本地运行测试

导航到终端。从应用程序的根目录，使用以下命令运行测试:

```
deno test --allow-net server.test.ts 
```

您应该会看到下面的响应。

```
test it should return a JSON response with status code 200 ... ok (28ms)
test exit the process forcefully after all the tests are done
 ...% 
```

有了您的测试用例，您就可以设置您的管道。

## 配置 Heroku

在应用程序文件夹的根目录下，创建一个名为`Procfile`的新文件。请注意，这个文件没有扩展名。

在`Procfile`中，添加以下命令。

```
web: deno run --allow-net server.ts --port=${PORT} 
```

这个命令声明了我们的应用程序的 web 进程和启动时要执行的命令。注意，我们正在传递参数`port`并将其绑定到 Heroku 提供的环境变量`PORT`。

接下来要做的是在 Heroku 上创建一个新的应用程序。你可以从 Heroku [仪表盘](https://dashboard.heroku.com/new-app)上完成这项工作。点击**新建**，然后点击**新建 App。**填写表格。您可以使用您喜欢的名称和地区。

![Create Heroku App](img/46121bcc29b2d463266a6dcc3e1eff16.png)

接下来，点击**创建 app** 完成创建过程。然后，您将被重定向到新创建的应用程序的**部署**视图。

您的下一个任务是添加一个构建包。为此，点击**设置**选项卡。在**构建包**部分，点击**添加构建包。**

![Buildpack Button](img/19eb95a45e8d182044d400fa1d4e53c9.png)

这将打开一个表单，您可以在其中选择一个官方支持的构建包，或者为您的构建包提供一个 URL。目前，Heroku 还没有官方支持的 Deno 构建包，所以您需要提供 Deno 构建包的 URL。将`https://github.com/chibat/heroku-buildpack-deno.git`粘贴到输入栏，点击**保存更改**。

![Add BuildPack URL](img/3def3d2d2abcbec18eaed9dc681d2d4f.png)

我们最不需要的就是 API 密钥。除了应用程序名称之外，您将使用它将 CircleCi 管道连接到 Heroku。要获取 API 密钥，打开[账户设置](https://dashboard.heroku.com/account)页面，向下滚动到 **API 密钥**部分。

![Reveal API Key](img/957444437baa5e5fb98f87b8ca4617c7.png)

点击**显示**按钮，复制 API 密钥。将它保存在您以后可以轻松访问的地方。

## 配置 CircleCI

接下来，我们需要为 CircleCI 添加管道配置。对于本项目，管道将包括两个步骤:

1.  构建和测试——在这一步中，您将为应用程序构建项目并运行测试。如果任何测试失败，将显示一条错误消息，该过程将终止。
2.  部署到 Heroku——如果构建和测试步骤成功完成，最新的更改将在这一步部署到 Heroku。

在项目的根目录下，创建一个名为`.circleci`的文件夹，并在其中创建一个名为`config.yml`的文件。在新创建的文件中，添加以下配置:

```
# Use the latest 2.1 version of CircleCI pipeline process engine.
version: 2.1

orbs:
  heroku: circleci/heroku@1.2

jobs:
  build-and-test:
    docker:
      - image: denoland/deno:1.10.3
    steps:
      - checkout
      - run: |
          deno test --allow-net server.test.ts

workflows:
  sample:
    jobs:
      - build-and-test
      - heroku/deploy-via-git:
          force: true # this parameter instructs the push to use a force flag when pushing to the heroku remote, see: https://devcenter.heroku.com/articles/git
          requires:
            - build-and-test 
```

这个配置引入了 Heroku orb `circleci/heroku`，它自动让我们访问一组强大的 Heroku 任务和命令。其中一个任务是`heroku/deploy-via-git`，它直接从你的 GitHub repo 将你的应用程序部署到你的 Heroku 账户。

该配置指定了一个名为`build-and-test`的作业。这项工作检查最新的代码，使用 Deno 创建 Docker 映像，并在`server.test.ts`中运行测试

最后，该配置指定了一个工作流，该工作流运行`build-and-test`作业，然后运行`heroku/deploy-via-git`作业。注意，有一个`requires`选项告诉 CircleCI 只有在构建任务完成时才运行`deploy-via-git`任务。

接下来，我们需要在 GitHub 上建立一个存储库，并将项目链接到 CircleCI。查看[将您的项目推送到 GitHub](https://circleci.com/blog/pushing-a-project-to-github/) 以获取指导。

接下来，登录你的 CircleCI 账户。如果你注册了你的 GitHub 账户，你所有的库都可以在你项目的仪表盘上看到。

点击`deno_circleci_heroku`项目旁边的**设置项目**。

CircleCI 检测项目的`config.yml`文件。点击**使用现有配置**，然后**开始建造**。您的第一个工作流将开始运行，但会失败！

![CircleCI Build Failed](img/62d555cf4adce1205b76cc4adecf4e10.png)

部署过程失败，因为我们没有提供 Heroku API 密钥。要解决这个问题，点击**项目设置**按钮，然后点击**环境变量**菜单选项。添加两个新变量。

*   对于`HEROKU_APP_NAME`，添加您在 Heroku 中使用的应用名称。该名称将是`deno-heroku-circleci`或自定义名称(如果您创建了一个名称的话)。

*   对于`HEROKU_API_KEY`,输入您之前从帐户设置页面检索的 Heroku API 密钥。

从头重新运行您的工作流，这一次您的工作流将成功运行。要确认工作流是否成功，您可以在浏览器中打开新部署的应用程序。您的应用程序的 URL 应该是这样的格式`https://<HEROKU_APP_NAME>.herokuapp.com/`。

应显示正在构建的**API 响应。**

## 实施索引路线

现在您的工作流已经启动并运行，您可以实现索引路径了。对于这个 API，它将返回硬编码用户的列表。在应用程序目录的根目录下，创建一个名为`users.ts`的新文件，并添加以下代码:

```
export default [
  {
    _id: "60d7b2ccfb294b7be115b6c4",
    isActive: true,
    picture: "http://placehold.it/32x32",
    age: 22,
    eyeColor: "green",
    name: "Harding Taylor",
    gender: "male",
    email: "hardingtaylor@lexicondo.com",
    phone: "+1 (839) 440-2917",
    address: "686 Harman Street, Reno, New Jersey, 5152",
    about:
      "Nisi irure aliquip aliquip Lorem. Elit ullamco commodo laborum aliqua commodo Lorem occaecat pariatur aute est reprehenderit ad. Qui nostrud enim aliqua consequat sit duis pariatur ex consectetur aute elit ad commodo officia.\r\n",
    registered: "2017-02-10T11:52:40 -01:00",
    friends: [
      {
        id: 0,
        name: "Donovan Nielsen",
      },
      {
        id: 1,
        name: "Barrera Hartman",
      },
      {
        id: 2,
        name: "Carmen Bean",
      },
    ],
    greeting: "Hello, Harding Taylor! You have 5 unread messages.",
    favoriteFruit: "banana",
  },
  {
    _id: "60d7b2cc411283f9ea9fdaff",
    isActive: false,
    picture: "http://placehold.it/32x32",
    age: 25,
    eyeColor: "blue",
    name: "Charlene Gibson",
    gender: "female",
    email: "charlenegibson@lexicondo.com",
    phone: "+1 (864) 446-3848",
    address: "132 Columbus Place, Clarksburg, Delaware, 281",
    about:
      "Exercitation voluptate cillum cillum do et voluptate officia Lorem sint. Ullamco quis ullamco et mollit. Veniam et labore aliquip elit fugiat proident labore.\r\n",
    registered: "2015-01-02T04:41:06 -01:00",
    friends: [
      {
        id: 0,
        name: "Morris Barrera",
      },
      {
        id: 1,
        name: "Aguilar Pearson",
      },
      {
        id: 2,
        name: "Denise Jacobs",
      },
    ],
    greeting: "Hello, Charlene Gibson! You have 5 unread messages.",
    favoriteFruit: "strawberry",
  },
  {
    _id: "60d7b2cc836c6ddb012508c1",
    isActive: false,
    picture: "http://placehold.it/32x32",
    age: 27,
    eyeColor: "blue",
    name: "Lavonne Barton",
    gender: "female",
    email: "lavonnebarton@lexicondo.com",
    phone: "+1 (958) 505-3633",
    address: "403 Ruby Street, Wintersburg, Pennsylvania, 1063",
    about:
      "Nisi sit cillum aute qui sunt ipsum deserunt ut. Fugiat laboris cupidatat mollit exercitation proident ad ut laboris nostrud amet amet dolor. Excepteur qui ea amet ut reprehenderit magna et proident sunt eu duis velit. Aliquip culpa proident aliqua dolore aliqua sint cillum anim amet duis esse nisi eu. Amet anim fugiat irure sit velit ad excepteur exercitation Lorem cillum sit deserunt dolore irure. Officia pariatur aliquip aliquip officia sunt sit dolore duis excepteur proident. Labore elit sunt ea deserunt officia nostrud.\r\n",
    registered: "2018-06-09T04:57:28 -01:00",
    friends: [
      {
        id: 0,
        name: "Barber Jimenez",
      },
      {
        id: 1,
        name: "Eliza Robbins",
      },
      {
        id: 2,
        name: "Charmaine Alexander",
      },
    ],
    greeting: "Hello, Lavonne Barton! You have 8 unread messages.",
    favoriteFruit: "banana",
  },
  {
    _id: "60d7b2cc112a8197462d9a8e",
    isActive: true,
    picture: "http://placehold.it/32x32",
    age: 32,
    eyeColor: "green",
    name: "Lea Evans",
    gender: "female",
    email: "leaevans@lexicondo.com",
    phone: "+1 (891) 524-3545",
    address: "725 Dennett Place, Alamo, New York, 1382",
    about:
      "Laborum dolor labore reprehenderit voluptate laborum ullamco non dolore magna officia ex velit. Ex nostrud duis ullamco cillum commodo occaecat pariatur nostrud nostrud occaecat incididunt minim eu. Minim ut ea quis laboris sunt.\r\n",
    registered: "2017-11-10T06:28:55 -01:00",
    friends: [
      {
        id: 0,
        name: "Fulton Oneal",
      },
      {
        id: 1,
        name: "House Watts",
      },
      {
        id: 2,
        name: "Isabel Melton",
      },
    ],
    greeting: "Hello, Lea Evans! You have 4 unread messages.",
    favoriteFruit: "strawberry",
  },
  {
    _id: "60d7b2cc3689b747a755bd89",
    isActive: true,
    picture: "http://placehold.it/32x32",
    age: 30,
    eyeColor: "green",
    name: "Jocelyn Harper",
    gender: "female",
    email: "jocelynharper@lexicondo.com",
    phone: "+1 (964) 464-2509",
    address: "524 Banner Avenue, Brenton, Texas, 1373",
    about:
      "Aliqua consectetur anim incididunt eu aute minim proident esse. Commodo eu tempor cillum veniam duis incididunt cupidatat. Tempor qui eu incididunt nostrud amet velit quis amet consequat. Deserunt nostrud eu laboris commodo irure fugiat dolore nisi in consequat ea in ullamco duis.\r\n",
    registered: "2015-04-10T11:13:10 -01:00",
    friends: [
      {
        id: 0,
        name: "Beatriz Carver",
      },
      {
        id: 1,
        name: "Gillespie Ferrell",
      },
      {
        id: 2,
        name: "Chris Boyer",
      },
    ],
    greeting: "Hello, Jocelyn Harper! You have 8 unread messages.",
    favoriteFruit: "banana",
  },
  {
    _id: "60d7b2cc03d9f88b8a833480",
    isActive: true,
    picture: "http://placehold.it/32x32",
    age: 39,
    eyeColor: "green",
    name: "Sharpe Wallace",
    gender: "male",
    email: "sharpewallace@lexicondo.com",
    phone: "+1 (979) 492-3250",
    address: "522 Madison Place, Charco, Missouri, 2144",
    about:
      "Voluptate culpa labore excepteur ut commodo veniam elit ea consectetur laboris adipisicing. Adipisicing ea officia qui reprehenderit. Ut ipsum elit irure id nisi. Enim cupidatat ea ea veniam est et enim nisi tempor. Minim laboris ipsum et ipsum mollit exercitation est labore voluptate cillum in dolor. Nostrud dolore labore et reprehenderit.\r\n",
    registered: "2017-11-12T10:09:50 -01:00",
    friends: [
      {
        id: 0,
        name: "Valenzuela Shelton",
      },
      {
        id: 1,
        name: "Margret Stuart",
      },
      {
        id: 2,
        name: "Rosie Nixon",
      },
    ],
    greeting: "Hello, Sharpe Wallace! You have 3 unread messages.",
    favoriteFruit: "apple",
  },
]; 
```

接下来，更新`server.test.ts`以匹配该代码:

```
import { superoak } from "https://deno.land/x/superoak@4.2.0/mod.ts";
import { delay } from "https://deno.land/x/delay@v0.2.0/mod.ts";
import app from "./server.ts";

Deno.test(
  "it should return a JSON response containing users with status code 200",
  async () => {
    const request = await superoak(app);
    await request
      .get("/")
      .expect(200)
      .expect("Content-Type", /json/)
      .expect(/"users":/);
  }
);

// Forcefully exit the Deno process once all tests are done.
Deno.test({
  name: "exit the process forcefully after all the tests are done\n",
  async fn() {
    await delay(1);
    Deno.exit(0);
  },
  sanitizeExit: false,
}); 
```

在`server.ts`中，用以下代码更新索引路线:

```
router.get("/", (context) => {
  context.response.type = "application/json";
  context.response.body = { users };
}); 
```

不要忘记从`users.ts`导入用户:

```
import users from "./users.ts"; 
```

提交您的代码并[推送到您的 Github 库](https://circleci.com/blog/pushing-a-project-to-github/)。

```
git add .

git commit -m "Implement index route"

git push origin main 
```

将更改推送到 Github 存储库之后，回到 CircleCI 仪表板，新的工作流正在那里运行。一切完成后，刷新你的 Heroku app。您新实现的路由将返回用户列表。

![API response User List](img/58c83d295a8479ea63463d5c9a75096c.png)

## 结论

在本教程中，我向您展示了如何使用 GitHub、CircleCI 和 Heroku 为 Deno API 设置 CI/CD 管道。

通过自动化发布新功能的过程，大大降低了人为错误对生产环境造成负面影响的风险。此外，产品还增加了一定程度的质量保证，因为新功能只有在通过指定的测试用例后才会部署。

这些实践有助于创建一个更有效的软件管理过程，该过程自动化重复的、平凡的部署方面，以便您的团队可以专注于解决问题。

本教程的完整代码库可从 GitHub 上的[获得。](https://github.com/yemiwebby/deno_circleci_heroku)

感谢您的阅读！

* * *

[Oluyemi](https://twitter.com/yemiwebby) 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他的问题解决技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。作为技术专家，他的爱好包括尝试新的编程语言和框架。

* * *

Oluyemi 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他的问题解决技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。作为技术专家，他的爱好包括尝试新的编程语言和框架。

[阅读更多 Olususi Oluyemi 的帖子](/blog/author/olususi-oluyemi/)