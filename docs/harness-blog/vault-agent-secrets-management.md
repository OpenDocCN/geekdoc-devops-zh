# 教程:如何将新的存储库代理集成方法与 Harness | Harness 一起使用

> 原文：<https://www.harness.io/blog/vault-agent-secrets-management>

Harness 即将发布一个新的选项来与 Vault 集成，以作为一个秘密管理器。下面是教程！

Harness 即将发布一个新的选项来与 Vault 集成，以作为一个秘密管理器。我们说的是一种叫做保险库代理的安全可靠的方法。

我在设置一个良好的集成时遇到了一些困难，这个集成将工作量减少到了 0。我不确定如何处理 Vault 提供的底层身份验证方法选项。到处都是 TTL，RoleIDs，过期的秘密等等。所以，这是我对有同样遭遇的人的一点微薄贡献。系好安全带。

## 辅导的

### 要求

一个保险库服务器，良好的凭证，和一些耐心。

### Vault 上的任务

我将创建一个名为 Harness:
的“kv”秘密引擎

然后，我将创建一个名为 harness_v2_engine_gabs 的策略:

path " harness/* " {
capabilities =[" create "、" list "、" read "、" update "、" delete "]
}

path " harness " {
capabilities =[" create "、" list "、" read "、" update "、" delete "]
}

path " auth/token/renew-self "
{
capabilities =[" read "、" update"]
}

*#列出现有秘密引擎。*
路径【sys/mounts】
{
功能= ["read"]
}

我们准备好了！

### 第一步

我将通过 Vault UI 控制台启用 AppRole 方法。然后，我将切换到 API(使用强大的令牌)来执行以下操作。

首先，我们运行它来启用 AppRole 方法:

保险库授权启用方法

然后，我们可以创建我们的角色:

curl \
-header " X-Vault-Token:s .<...> " \
-请求 POST \
-data ' { " token _ TTL ":" 60m "，" token_max_ttl": "120m "，" token _ policies ":[" harness _ v2 _ engine _ gabs "]，" bind _ secret _ id ":true } ' \
http:*//15 . 228 . 43 . 18:8200/v1/auth/approle/role/harness _ gabs _ agent*

成功了！

### 第二步

是时候配置我们的保险库代理了。我们首先从检索非常重要的 RoleID 开始:

curl \
-header " X-Vault-Token:s . pasnbqcp 46 cujov 6 coj 7 ut w3 " \
http:*//15。228 .43 .18:8200/v1/auth/approle/role/harness _ gabs _ agent/role-id*

{ " request _ id ":"<...>，" lease_id ":"，" renewable":false，" lease_duration":0，" data":{"role_id":"4ecb33 <..> "}，" wrap_info":null，" warnings":null，" auth":null}

现在，我们的“不被爱”秘密:

curl \
-header " X-Vault-Token:s . pasnbqcp 46 cujov 6 coj 7 ut w3 "
-request POST \
http:*//15。228 .43 .18:8200/v1/auth/approle/role/harness _ gabs _ agent/secret-id*{ " request _ id ":"<...>、" lease _ id ":" renewable ": false 、" lease_duration":0 、" data":{"secret_id":"e72d <...>、" secret_id_accessor":"3aa <...>，" secret_id_ttl":0}，" wrap_info":null，" warnings":null，" auth":null}

将这两个 id 保存在你的大脑中(你的大脑或者记事本++因为这是一个实验室)。

### 第三步

是时候去找金库特工了！非常重要的一点是，Harness 只需要访问代理将令牌写入的接收器文件。我将在托管我的**管理代理**的同一个**服务器**上运行**保险库代理**。

根据您的 SecOps 团队，他们可能有一个共享卷或类似的东西。唯一需要做的事情是确保 Harness 委托能够到达接收器文件路径。

您要做的第一件事是在该代理上获得保险库。我们不会启动服务器。是代理，我猜也在同一个捆绑包里。

使用 Vault 的第一步是[安装它](https://learn.hashicorp.com/tutorials/vault/getting-started-install)。

这是我的 Vault 代理主页，位于/etc/vault:

我将在下一步解释每个文件。

### 第四步

我想做的第一件事是编写一个好的 config.hcl。这可能会根据您的首选身份验证方法和安全限制而有所不同，但我在本实验中使用的是 AppRole 方法。考虑到这一点，这是一个很好的开始配置文件:

pid_file = " ./PID file "

金库{
地址= " http://15。228 .43 .18:8200 "
TLS _ skip _ verify = true
retry {
num _ retries = 5
}
}
}
auto _ auth {
method " approle " {
config = {
role _ id _ file _ path = " ./roleid "
secret _ id _ file _ path = " ./secret id "
remove _ secret _ id _ file _ after _ reading = false
}
}

sink " file " {
config = {
path = " 1 ./sink "
}
}
}
}
}
缓存{
use _ auto _ auth _ token = true
}

监听器" TCP " {
address = " 127。0 .0 .1:8100 "
TLS _ disable = true
}

您可以使用该文档来增强甚至更改它:[存储库代理](https://www.vaultproject.io/docs/agent)。

存储库代理是一个客户端守护程序，可用于自动执行某些存储库功能。

我当前的配置要求我提供 RoleID + AppID。这是我在这里做的:

### 第五步

是时候运行和测试代理凭证了！让我们运行代理。如果你有信心，可以配合 nohup 使用！

存储库代理-config =/etc/vault/config . HCl
<或>
nohup 存储库代理-config =/etc/vault/config . HCl&

您应该会看到类似这样的内容:

### 第六步

是时候用马具测试它了！**重要提示**:目前，秘密管理器的这一功能在一个功能标志后面。您可以要求您的 CSM 或任何联系人来实现这一点。然而，这将在下周正式上市，所以我认为你不需要这样做。

好吧，让我们让秘密经理开始工作吧！

访问您的线束 UI ->机密管理器->配置机密管理器(蓝色图标)->添加机密管理器。

请注意新字段，如委托选择器和接收器路径。这很容易配置。看一看:

### 重要的最后一步-测试跳马！

让我们通过线束 UI 创建一个漂亮的秘密:

有用！

### 结果

这是一个安全，可靠的方法，使保险库你的线束秘密经理。如果你需要在 **secretID** 中使用 TTL，这可能会增加一点麻烦来使代理保持良好的凭证，但是这是符合自动化的，我觉得。

有任何问题或意见吗？不要犹豫伸出手。

加百列