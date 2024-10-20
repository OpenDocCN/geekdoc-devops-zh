# Google bin auth-Kubernetes | circle ci

> 原文：<https://circleci.com/blog/get-started-with-google-binary-authorization/>

## CircleCI 的二进制授权 orb 的演练

在 Next’19 上，Google [宣布](https://cloud.google.com/blog/products/containers-kubernetes/all-the-kubernetes-security-news-from-google-cloud-next19)二进制授权的正式发布，这是一种针对部署在 Google Kubernetes 引擎上的容器图像的安全控制，CircleCI 是其发布合作伙伴。我们的[二进制授权 orb](https://circleci.com/developer/orbs/orb/circleci/gcp-binary-authorization) 简化了使用 CircleCI 构建、测试和部署的映像的验证过程，确保只有那些在 CI/CD 过程中由可信机构签名的映像才能在 GKE 上运行。

二进制授权，像 Grafeas 的 [Kritis](https://github.com/grafeas/kritis) ，它的开源、云不可知的对等物，使用一组不同的 RESTful 资源来管理和认证软件发布过程:

*   [Policies](https://cloud.google.com/binary-authorization/docs/key-concepts#policies) :描述在被特定证明者授权后，哪些容器映像可以被部署到特定的 Kubernetes 集群的规则集；
*   [证明者](https://cloud.google.com/binary-authorization/docs/key-concepts#attestors):通过创建和签署证明来验证容器映像的部署准备状态的指定方，机器或人；
*   [证明](https://cloud.google.com/binary-authorization/docs/key-concepts#attestations):证明者证明单个映像满足部署所需的所有条件的声明。

这些概念与 CircleCI 的功能相吻合，如[受限上下文](https://circleci.com/blog/protect-secrets-with-restricted-contexts/)，它将秘密和环境变量的集合传递给特定的用户组，以及[手动批准](https://circleci.com/blog/manual-job-approval-and-scheduled-workflow-runs/)工作。总之，这些资源允许开发-运营团队根据他们的需求和规范，使用自动和手动检查的组合来平衡部署时间和可靠性/安全性，从而精心设计全面的软件供应链安全流程。

CircleCI 的 orb 让二进制授权很容易上手。一个作业`create-attestation`可以引导您完成创建策略、证明者和证明的整个过程。使用 orb 构建一个全新的 GKE 集群，或者直接引用一个已经存在的集群。即时制定政策，或者自己制定政策。orb 甚至将生成并存储 PGP 密钥对，由证明者用来签署证明(通过 Google 的云密钥管理服务对非对称密钥的支持是 orb 路线图的下一步)。唯一的先决条件是谷歌云平台中的单个项目，或者对于一个[多项目设置](https://cloud.google.com/binary-authorization/docs/multi-project-setup-cli)，三个独立的 GCP 项目(部署者、证明者、证明)。

与 CircleCI 的[Google Container Registry orb](https://circleci.com/developer/orbs/orb/circleci/gcp-gcr)配对，二进制授权 orb 可以在 YAML 的几行代码中提供完整的部署解决方案，如这个`simple-deploy-attested-image` [示例](https://circleci.com/developer/orbs/orb/circleci/gcp-binary-authorization#usage-simple-deploy-attested-image)所示:

```
version: 2.1

orbs:
  gcp-gcr: circleci/gcp-gcr@x.y.z
  bin-authz: circleci/gcp-binary-authorization@x.y.z

workflows:
  push_sign_deploy:
    jobs:
      - gcp-gcr/build_and_push_image:
          context: your-context # context containing any required env vars
          image: your-image # your image name
          registry-url: gcr.io # default value, here for clarity
          tag: your-tag # default value

      - bin-authz/create-attestation:
          context: your-context
          attestor: $CIRCLE_USERNAME # default value
          keypair-email: email.address@used.to.generate.keypair.com
          gke-cluster-name: your-GKE-cluster-name
          use-note-file: true
          note-filepath: your-container-analysis-note.json
          use-policy-file: true
          policy-filepath: your-binauthz-policy-file.yaml
          image-path: gcr.io/$GOOGLE_PROJECT_ID/your-image
          image-tag: your-tag
          requires: [gcp-gcr/build_and_push_image]
          deployment-steps:
            - run: |
                kubectl run your-server \
                  --image gcr.io/$GOOGLE_PROJECT_ID/your-image@$YOUR_IMAGE_DIGEST \
                  --port 8080 
```

二进制授权 orb 有大量的参数，但是不要被淹没——它们被设计为合理的默认值，在大多数用例中最大限度地减少了样板文件。如需进一步指导，请参见 orb 的其他[使用示例](https://circleci.com/developer/orbs/orb/circleci/gcp-binary-authorization#usage-examples)(其 [GitHub 库](https://github.com/CircleCI-Public/gcp-binary-authorization-orb)也有额外的说明)。

最后，由于所有 CircleCI orbs 都是开源的，如果你想在这个 orb 中看到其他东西，我们总是欢迎[问题](https://github.com/CircleCI-Public/gcp-binary-authorization-orb/issues)和[拉请求](https://github.com/CircleCI-Public/gcp-binary-authorization-orb/pulls)—[我们活跃的 orb 开发者和用户社区](https://discuss.circleci.com/c/ecosystem/orbs)可以帮助解决任何关于这个或任何其他 orb 的问题。