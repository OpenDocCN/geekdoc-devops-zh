# GraphQL 教程-如何使用 Python | Harness 与 Harness API 交互

> 原文：<https://www.harness.io/blog/graphql-python-api>

答案永远是 Python。在本教程中，我们将学习如何使用 Python 与 Harness API 进行交互。

我们不断地向 GraphQL 添加新的实体，这使得与 Harness 的编程交互变得非常有趣。

自然地，我们开始看到以编程方式与 Harness GraphQL 交互的多个用例，以获得诸如“每个服务有多少个实例？好的，那么每个服务的环境呢？”

我对壳剧本上瘾了。简单又强大。但是，根据复杂性以及如何操作 GraphQL 结果集，阅读代码(并支持它)可能会成为一场噩梦。因此，我花了一些时间寻找一种方法来自动理解这种交互的 HTTP 方面，同时我可以轻松地操作 dictionary/JSON 集合。

当然，答案是 Python。当我介绍 [GQL 模块](https://github.com/graphql-python/gql)时，我将与您一起探索这个发现。我不会强制执行 pep-8、异常处理等。这篇博客文章只是给你一个很好的介绍这种方法。

## 要求

我将在教程中探讨这一点。但是，如果您想直接测试我的项目，我们需要在运行时设置一些环境变量。可以跟着这个 [README.md](https://github.com/gabrielcerioni/harness_instanceStats_gql_to_csv/blob/main/README.md) 做参考！

## 辅导的

### 第一步

需要进口什么？对于本例，我导入了以下内容:

**导入**OS
导入 csv
**导入**日志

**从** gql **客户端导入**gql
从 gql.transport.requests **导入** RequestsHTTPTransport

您可以通过运行以下命令来解决所有依赖关系:

python 3-m pip 安装-r https://raw。githubusercontent。com/gabrielcerioni/harness _ instance stats _ gql _ to _ CSV/main/requirements。文本文件（textfile）

### 第二步

让我们定义一个简单的日志记录器和一个“常量”,我将从环境变量中获取:

伐木。基本配置(format = ' %(ASC 时间)s-%(级别名)s-%(消息)s '，级别=日志记录.INFO)

API _ KEY = OS。环境。get(' HARNESS _ graph QL _ API _ KEY)
API _ ENDPOINT = OS。环境。get('线束 _ 图形 QL _ 端点')
OUTPUT _ CSV _ NAME _ CONST = OS。环境。get(' HARNESS _ GQL _ CSV _ NAME ')

### 第三步

我不想把这段代码写得太长，所以我就开门见山了。让我们定义一个好的通用**查询**函数来处理 GraphQL:

def generic _ graphql _ query(query):
req _ headers = {
' x-API-KEY ':API _ KEY
}

_ transport = RequestsHTTPTransport(
URL = API _ ENDPOINT，
headers=req_headers，
use_json=True，
)
 *#使用定义的传输创建一个 graph QL 客户端*
Client = Client(transport = _ transport，fetch_schema_from)

我们也可以做一些非常类似于运行**突变**的事情:

def generic _ graphql _ mutation(mutation _ query，params):
req _ headers = {
' x-API-KEY ':API _ KEY
}

_ transport = RequestsHTTPTransport(
URL = API _ ENDPOINT，
headers=req_headers，
use_json=True，
)
 *#使用定义的传输*
客户端创建一个 graph QL 客户端

## 真实用例

让我们从客户那里探索两个真实的使用案例。

### 用例 1

**客户陈述:**我需要按服务和环境为所有部署的实例生成(按需)CSV 报告。我还需要服务 ID 来确保我计算正确。

**结果项目(我还在增强这个):**[GitHub-gabrielcerioni/harness _ instance stats _ gql _ to _ csv:](https://github.com/gabrielcerioni/harness_instanceStats_gql_to_csv)这是一个简单的 Python，它将 instanceStats GraphQL 查询解析为 CSV。

至此，我还需要两个函数:

*   一个用于检索简单的 instanceStats 结果集；
*   另一个得到它并把所有东西都放在 UTF-8 CSV 上。

差不多就是这样:

**def get _ all _ instances _ by _ Service _ by _ env**():
query = ' ' ' {
实例统计(分组依据:[{实体聚合:服务}，{实体聚合:环境}]){
...在堆叠的数据上{
数据点{
key {
name
id
}
values {
key {
name
}
value
}
}
}
}
}
} ' ' '
generic _ query _ result = generic _ graph QL _ query(query)

**return**
result _ list 中 Service _ item **的

**:
instances =[]
Service _ Name = Service _ item[' key '][' Name ']
Service _ ID = Service _ item[' key '][' ID ']

Instance _ environments = Instance _ environments dict writer(output _ file，fieldnames=clean_dict_list[0]. 按键()，)
fc。写头()
fc。writerows(clean _ dict _ list)

**return**(clean _ dict _ list)**** 

然后，我们可以轻松地协调“主”入口点中的一切:

if _ _ name _ _ = = ' _ _ main _ _ ':
logging . info("启动程序...")

logging.info("检索您当前的 instanceStats GraphQL 查询结果集...")
result _ from _ query = get _ all _ instances _ by _ service _ by _ env()
logging . info(" Done！")

logging.info("展开嵌套 dict 中的所有行-然后将其放在 CSV 上:{0} "。format(OUTPUT _ CSV _ NAME _ CONST))
parsed _ result _ set = parse _ result _ to _ CSV(result _ from _ query)
logging . info(" Done！在此输出列表内容:")
print(parsed _ result _ set)

logging . info("程序已退出！祝你愉快！”)

### 用例 2

**客户陈述:**我需要从我的帐户生成所有用户的输出。但是我有很多，这个会无限分页。请尽快帮助和处理此事！

**结果项目(我还在增强这个):**[GitHub-gabrielcerioni/Harness _ graph QL _ Labs:](https://github.com/gabrielcerioni/harness_graphql_labs)Gabs CSE-Harness-graph QL Labs-Python。

我的 GH 项目有一个虚拟加载器，但是我不建议运行它！我们可以关注一个将查询所有用户的函数，但它也知道如何处理 GraphQL 偏移/分页:

**def get _ harness _ account _ users**():
offset = 0
has _ more = True
total _ user _ list =[]

**while**has _ more:
query = ' ' ' {
users(限制:100 offset:' ' '+str(offset)+' ' '){
pageInfo {
total
limit
hasMore
offset
}
nodes {
name
}
}
} ' ' '

generic _ query _ result = generic _ graph QL _ query(query)
loop _ user _ list = generic _ query _ result[" users "][" nodes "]【T25

然后，我们可以有这个非常简单的入口点:

if _ _ name _ _ = = ' _ _ main _ _ ':
logging . info("启动程序...")

logging.info("从你的 Harness 账户获取所有用户")
result _ from _ query = get _ Harness _ Account _ users()
logging . info(" Done！您的帐户中有{0}个用户！"。format(len(result _ from _ query))
print(" "
logging . info("在您的 STDOUT 上打印用户列表")

print(result _ from _ query)

logging . info("程序已退出！")

## 结果

下面是我们可以期待看到的，特别是对于用例 1:

希望这对你们有帮助！我们只看了两个用例，但是还有很多可以帮助我们的。

一如既往，如果你有任何问题，给我发消息！我很乐意帮忙。

加布里埃尔