# CircleCI 博客内容许可

> 原文：<https://circleci.com/blog/blog-content-licensing-at-circleci/>

我们不时会收到来自贡献者和读者的问题，询问我们博客上的内容是如何获得许可的。我们的目的一直是让读者在有限的限制下使用我们博客上的内容。今天，我们更新了我们的博客许可条款，以明确说明在此发布的内容如何获得许可，以便我们的读者可以更好地使用它。

除了 CircleCI 商标之外，CircleCI 博客上的所有内容都获得了[知识共享署名 4.0 国际版(CCY 4.0 版)许可](https://creativecommons.org/licenses/by/4.0/)的许可，除非另有说明。除非另有说明，CircleCI 博客上发布的所有软件代码，包括代码片段，均根据 [Apache 许可证 2.0 版](https://www.apache.org/licenses/LICENSE-2.0.txt)进行许可。

我们向读者提供代码示例，以帮助他们解决常见的瓶颈，突出新颖的解决方案，并介绍新功能的集成。我们完全期待它们被我们的读者复制和使用。例如，下面是一个项目的[配置文件](https://circleci.com/docs/configuration-reference/)片段，展示了如何在管道中的作业中使用`cURL`与 Bintray REST API 通信。它确保服务是可操作的，如果不是，它使用逻辑失败并退出作业。

```
...
      - run:
          name: Get Bintray API status
          command: |
                    BT_STATUS=$(curl -s https://status.bintray.com/api/v2/status.json |  jq --raw-output '.status.description')
                    echo “Bintray API Status: $BT_STATUS”
                    if [ “$BT_STATUS” != “All Systems Operational” ]; then
                      echo “Error [Bintray API Status: $BT_STATUS]”
                      exit 1
                    fi
... 
```

请随意使用这段代码，并根据您的需要进行调整！

我们希望这些许可条款能够阐明如何使用我们博客上的内容，但如果有任何问题，请随时联系我们。