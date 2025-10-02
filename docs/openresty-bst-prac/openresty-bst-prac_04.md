# PostgresNginxModule

PostgreSQL 是加州大学博客利分校计算机系开发的对象关系型数据库管理系统(ORDBMS)，目前是免费开源的，且是全功能的自由软件数据库。 PostgreSQL 支持大部分 SQL 标准，其特性覆盖了 SQL-2/SQL-92 和 SQL-3/SQL-99，并且提供了许多其他现代特点，如复杂查询、外键、触发器、视图、事务完整性、多版本并行控制系统（MVCC）等。 PostgreSQL 可以使用许多方法扩展，比如，通过增加新的数据类型、函数、操作符、聚集函数、索引方法、过程语言等。

PostgreSQL 在灵活的 BSD 风格许可证下发行，任何人都可以根据自己的需要免费使用、修改和分发 PostgreSQL，不管是用于私人、商业、还是学术研究。

在 360 企业安全产品中，PostgreSQL 作为关系型数据库基础组件使用，大量的企业安全信息均分解为若干个对象和关系表存储于 PostgreSQL，Openresty 使用 ngx_postgres 模块，与 PostgreSQL 通讯。

ngx_postgres 是一个提供 nginx 与 PostgreSQL 直接通讯的 upstream 模块。应答数据采用了 rds 格式,所以模块与 ngx_rds_json 和 ngx_drizzle 模块是兼容的。

# 调用方式简介

# PostgresNginxModule 模块的调用方式

### ngx_postgres 模块使用方法

```
location /postgres {
    internal;

    default_type text/html;
    set_by_lua $query_sql 'return ngx.unescape_uri(ngx.var.arg_sql)';

    postgres_pass   pg_server;
    rds_json          on;
    rds_json_buffer_size 16k;
    postgres_query  $query_sql;
    postgres_connect_timeout 1s;
    postgres_result_timeout 2s;
} 
```

这里有很多指令要素：

*   internal 这个指令指定所在的 location 只允许使用于处理内部请求，否则返回 404 。
*   set_by_lua 这一段内嵌的 Lua 代码用于计算出 $query_sql 变量的值，即后续通过指令 postgres_query 发送给 PostgreSQL 处理的 SQL 语句。这里使用了 GET 请求的 query 参数作为 SQL 语句输入。
*   postgres_pass 这个指令可以指定一组提供后台服务的 PostgreSQL 数据库的 upstream 块。
*   rds_json 这个指令是 ngx_rds_json 提供的，用于指定 ngx_rds_json 的 output 过滤器的开关状态，其模块作用就是一个用于把 rds 格式数据转换成 json 格式的 output filter。这个指令在这里出现意思是让 ngx_rds_json 模块帮助 ngx_postgres 模块把模块输出数据转换成 json 格式的数据。
*   rds_json_buffer_size 这个指令指定 ngx_rds_json 用于每个连接的数据转换的内存大小. 默认是 4/8k,适当加大此参数，有利于减少 CPU 消耗。
*   postgres_query 指定 SQL 查询语句，查询语句将会直接发送给 PostgreSQL 数据库。
*   postgres_connect_timeout 设置连接超时时间。
*   postgres_result_timeout 设置结果返回超时时间。

这样的配置就完成了初步的可以提供其他 location 调用的 location 了。但这里还差一个配置没说明白，就是这一行：

```
postgres_pass   pg_server; 
```

其实这一行引入了 名叫 pg_server 的 upstream 块，其定义应该像如下：

```
upstream pg_server {
    postgres_server  192.168.1.2:5432 dbname=pg_database
            user=postgres password=postgres;
    postgres_keepalive max=800 mode=single overflow=reject;
} 
```

这里有一些指令要素：

*   postgres_server 这个指令是必须带的，但可以配置多个，用于配置服务器连接参数，可以分解成若干参数：

    *   直接跟在后面的应该是服务器的 IP:Port
    *   dbname 是服务器要连接的 PostgreSQL 的数据库名称。
    *   user 是用于连接 PostgreSQL 服务器的账号名称。
    *   password 是账号名称对应的密码。
*   postgres_keepalive 这个指令用于配置长连接连接池参数，长连接连接池有利于提高通讯效率，可以分解为若干参数：

    *   max 是工作进程可以维护的连接池最大长连接数量。
    *   mode 是后端匹配模式，在 postgres_server 配置了多个的时候发挥作用，有 single 和 multi 两种值，一般使用 single 即可。
    *   overflow 是当长连接数量到达 max 之后的处理方案，有 ignore 和 reject 两种值。
        *   ignore 允许创建新的连接与数据库通信，但完成通信后马上关闭此连接。
        *   reject 拒绝访问并返回 503 Service Unavailable

这样就构成了我们 PostgreSQL 后端通讯的通用 location，在使用 Lua 业务编码的过程中可以直接使用如下代码连接数据库（折腾了这么老半天）：

```
local json = require "cjson"

function test()
    local res = ngx.location.capture('/postgres',
        { args = {sql = "SELECT * FROM test" } }
    )

    local status = res.status
    local body = json.decode(res.body)

    if status == 200 then
        status = true
    else
        status = false
    end
    return status, body
end 
```

### 与 resty-mysql 调用方式的不同

先来看一下`lua-resty-mysql`模块的调用示例代码。

```
# you do not need the following line if you are using
# the ngx_openresty bundle:
lua_package_path "/path/to/lua-resty-mysql/lib/?.lua;;";

server {
    location /test {
       content_by_lua_block {
            local mysql = require "resty.mysql"
            local db, err = mysql:new()
            if not db then
                ngx.say("failed to instantiate mysql: ", err)
                return
            end

            db:set_timeout(1000) -- 1 sec

            local ok, err, errno, sqlstate = db:connect{
                host = "127.0.0.1",
                port = 3306,
                database = "ngx_test",
                user = "ngx_test",
                password = "ngx_test",
                max_packet_size = 1024 * 1024 }

            if not ok then
                ngx.say("failed to connect: ", err, ": ", errno, " ", sqlstate)
                return
            end

            ngx.say("connected to mysql.")

            -- run a select query, expected about 10 rows in
            -- the result set:
            res, err, errno, sqlstate =
                db:query("select * from cats order by id asc", 10)
            if not res then
                ngx.say("bad result: ", err, ": ", errno, ": ", sqlstate, ".")
                return
            end

            local cjson = require "cjson"
            ngx.say("result: ", cjson.encode(res))

            -- put it into the connection pool of size 100,
            -- with 10 seconds max idle timeout
            local ok, err = db:set_keepalive(10000, 100)
            if not ok then
                ngx.say("failed to set keepalive: ", err)
                return
            end
        }
    }
} 
```

看过这段代码，大家肯定会说：这才是我熟悉的，我想要的。为什么刚刚`ngx_postgres`模块的调用这么诡异，配置那么复杂，其实这是发展历史造成的。`ngx_postgres`起步比较早，当时`OpenResty`也还没开始流行，所以更多的 Nginx 数据库都是以 ngx_c_module 方式存在。有了`OpenResty`，才让我们具有了使用完整的语言来描述我们业务能力。

后面我们会单独说一说使用`ngx_c_module`的各种不方便，也就是我们所踩过的坑。希望能给大家一个警示，能转到`lua-resty-***`这个方向的，就千万不要和`ngx_c_module`玩，`ngx_c_module`的扩展性、可维护性、升级等各方面都没有`lua-resty-***`好。

这绝对是经验的总结。不服来辩！

# 不支持事务

我们继续上一章节的内容，大家应该记得我们`Lua`代码中是如何完成`ngx_postgres`模块调用的。我们把他简单改造一下，让他更接近真实代码。

```
 local json = require "cjson"

    function db_exec(sql_str)
        local res = ngx.location.capture('/postgres',
            { args = {sql = sql_str } }
        )

        local status = res.status
        local body = json.decode(res.body)

        if status == 200 then
            status = true
        else
            status = false
        end
        return status, body
    end

    -- 转账操作，对 ID=100 的用户加 10，同时对 ID=200 的用户减 10。
?    local status 
?    status = db_exec("BEGIN")
?    if status then
?        db_exec("ROLLBACK")
?    end
?
?    status = db_exec("UPDATE ACCOUNT SET MONEY=MONEY+10  WHERE ID = 100")
?    if status then
?        db_exec("ROLLBACK")
?    end
?
?    status = db_exec("UPDATE ACCOUNT SET MONEY=MONEY-10  WHERE ID = 200")
?    if status then
?        db_exec("ROLLBACK")
?    end
?
?    db_exec("COMMIT") 
```

后面这部分有问题的代码，在没有并发的场景下使用，是不会有任何问题的。但是这段代码在高并发应用场景下，错误百出。你会发现最后执行结果完全摸不清楚。明明是个转账逻辑，一个收入，一直支出，最后却发现总收入比支出要大。如果这个错误发生在金融领域，那不知道要赔多少钱。

如果你能靠自己很快明白错误的原因，那么恭喜你你对数据库连接、`Nginx`机理都是比较清楚的。如果你想不明白，那就听我给你掰一掰这面的小知识。

数据库的事物成功执行，事物相关的所有操作是必须执行在一条连接上的。`SQL`的执行情况类似这样：

```
连接：`BEGIN` -> `SQL(UPDATE、DELETE... ...)` -> `COMMIT`。 
```

但如果你创建了两条连接，每条连接提交的 SQL 语句是下面这样：

```
连接 1：`BEGIN` -> `SQL(UPDATE、DELETE... ...)` 
连接 2：`COMMIT` 
```

这时就会出现连接 1 的内容没有被提交，行锁产生。连接 2 提交了一个空的`COMMIT`。

说到这里你可能开始鄙视我了，谁疯了非要创建两条连接来这么用 SQL 啊。有麻烦，又不好看，貌似从来没听说过还有人在一次请求中创建多个数据库连接，简直就是非人类嘛。

或许你不会主动、显示的创建多个连接，但是刚刚的示例代码，高并发下这个事物的每个 SQL 语句都可能落在不同的连接上。为什么呢？这是因为通过`ngx.location.capture`跳转到`/postgres`小节后，`Nginx`每次都会从连接池中挑选一条空闲连接，二当时那条连接是空闲的，完全没法预估。所以上面的第二个例子，就这么静悄悄的发生了。如果你不了解`Nginx`的机理，那么他肯定会一直困扰你。为什么一会儿好，一会儿不好。

同样的道理，我们推理到`DrizzleNginxModule`、`RedisNginxModule`、`Redis2NginxModule`，他们都是无法做到在两次连续请求落到同一个连接上的。

由于这个`Bug`藏得比较深，并且不太好讲解，所以我觉得生产中最好用`lua-resty-*`这类的库，更符合标准调用习惯，直接可以绕过这些坑。不要为了一点点的性能，牺牲了更大的蛋糕。看得见的，看不见的，都要了解用用，最后再做决定，肯定不吃亏。

# 超时

当我们所有数据库的`SQL`语句是通过子查询方式完成，对于超时的控制往往很容易被大家忽略。因为大家在代码里看不到任何调用`set_timeout`的地方。实际上`PostgreSQL`已经为我们预留好了两个设置。

请参考下面这段配置：

```
location /postgres {
    internal;

    default_type text/html;
    set_by_lua $query_sql 'return ngx.unescape_uri(ngx.var.arg_sql)';

    postgres_pass   pg_server;
    rds_json          on;
    rds_json_buffer_size 16k;
    postgres_query  $query_sql;
    postgres_connect_timeout 1s;
    postgres_result_timeout 2s;
} 
```

生产中使用这段配置，遇到了一个不大不小的坑。在我们的开发机、测试环境上都没有任何问题的安装包，到了用户那边出现所有数据库操作异常，而且是数据库连接失败，但手工连接本地数据库，发现没有任何问题。同样的执行程序再次 copy 回来后，公司内环境不能复现问题。考虑到我们当次升级刚好修改了`postgres_connect_timeout`和`postgres_result_timeout`的默认值，所以我们尝试去掉了这两行个性设置，重启服务后一切都好了。

起初我们也很怀疑出了什么诡异问题，要知道我们的`nginx`和`PostgreSQL`可是安装在本机，都是使用`127.0.0.1`这样的 IP 来完成通信的，难道客户的机器在这个时间内还不能完成连接建立？

经过后期排插问题，发现是客户的机器上安装了一些趋势科技的杀毒客户端，而趋势科技为了防止无效连接，对所有连接的建立均阻塞了一秒钟。就是这一秒钟，让我们的服务彻底歇菜。

本以为是一次比较好的优化，没想到因为这个原因没能保留下来，反而给大家带来麻烦。只能说企业版环境复杂，边界比较多。但也好在我们一直使用最常见的技术、最常见的配置解决各种问题，让我们的经验可以复用到其他公司里。

# 健康监测

# SQL 注入

有使用 SQL 语句操作数据库的经验朋友，应该都知道使用 SQL 过程中有一个安全问题叫 SQL 注入。所谓 SQL 注入，就是通过把 SQL 命令插入到 Web 表单提交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的 SQL 命令。 为了防止 SQL 注入，在生产环境中使用 Openresty 的时候就要注意添加防范代码。

延续之前的 ngx_postgres 调用代码的使用，

```
 local sql_normal = [[select id, name from user where name=']] .. ngx.var.arg_name .. [[' and password=']] .. ngx.var.arg_password .. [[' limit 1;]]

    local res = ngx.location.capture('/postgres',
        { args = {sql = sql } }
    )

    local body = json.decode(res.body)

    if (table.getn(res) > 0) {
        return res[1];
    }

    return nil; 
```

假设我们在用户登录使用上 SQL 语句查询账号是否账号密码正确，用户可以通过 GET 方式请求并发送登录信息比如：

```
#    http://localhost/login?name=person&password=12345 
```

那么我们上面的代码通过 ngx.var.arg_name 和 ngx.var.arg_password 获取查询参数，并且与 SQL 语句格式进行字符串拼接，最终 sql_normal 会是这个样子的：

```
 local sql_normal = [[select id, name from user where name='person' and password='12345' limit 1;]] 
```

正常情况下，如果 person 账号存在并且 password 是 12345，那么 sql 执行结果就应该是能返回 id 号的。这个接口如果暴露在攻击者面前，那么攻击者很可能会让参数这样传入：

```
 name="' or ''='"
    password="' or ''='" 
```

那么这个 sql_normal 就会变成一个永远都能执行成功的语句了。

```
 local sql_normal = [[select id, name from user where name='' or ''='' and password='' or ''='' limit 1;]] 
```

这就是一个简单的 sql inject （注入）的案例，那么问题来了，面对这么凶猛的攻击者，我们有什么办法防止这种 SQL 注入呢？

很简单，我们只要 把 传入参数的变量 做一次字符转义，把不该作为破坏 SQL 查询语句结构的双引号或者单引号等做转义，把 ' 转义成 \'，那么变量 name 和 password 的内容还是乖乖的作为查询条件传入，他们再也不能为非作歹了。

那么怎么做到字符转义呢？要知道每个数据库支持的 SQL 语句格式都不太一样啊，尤其是双引号和单引号的应用上。有几个选择：

```
 ndk.set_var.set_quote_sql_str()
    ndk.set_var.set_quote_pgsql_str()
    ngx.quote_sql_str() 
```

这三个函数，前面两个是 ndk.set_var 跳转调用，其实是 HttpSetMiscModule 这个模块提供的函数，是一个 C 模块实现的函数，ndk.set_var.set_quote_sql_str() 是用于 MySQL 格式的 SQL 语句字符转义，而 set_quote_pgsql_str 是用于 PostgreSQL 格式的 SQL 语句字符转义。最后 ngx.quote_sql_str 是一个 ngx_lua 模块中实现的函数，也是用于 MySQL 格式的 SQL 语句字符转义。

让我们看看代码怎么写：

```
 local name = ngx.quote_sql_str(ngx.var.arg_name)
    local password = ngx.quote_sql_str(ngx.var.arg_password)
    local sql_normal = [[select id, name from user where name=]] .. name .. [[ and password=]] .. password .. [[ limit 1;]]

    local res = ngx.location.capture('/postgres',
        { args = {sql = sql } }
    )

    local body = json.decode(res.body)

    if (table.getn(res) > 0) {
        return res[1];
    }

    return nil; 
```

注意上述代码有两个变化：

```
* 用 ngx.quote_sql_str 把 ngx.var.arg_name 和 ngx.var.arg_password 包了一层，把返回值作为 sql 语句拼凑起来。
* 原本在 sql 语句中添加的单引号去掉了，因为 ngx.quote_sql_str 的返回值正确的带上引号了。 
```

这样已经可以抵御 SQL 注入的攻击手段了，但开发过程中需要不断的产生新功能新代码，这时候也一定注意不要忽视 SQL 注入的防护，安全防御代码就想织网一样，只要有一处漏洞，鱼儿可就游走了。