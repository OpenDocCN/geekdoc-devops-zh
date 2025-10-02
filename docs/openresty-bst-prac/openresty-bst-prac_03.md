# LuaCjsonLibrary

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它基于 ECMAScript 的一个子集。 JSON 采用完全独立于语言的文本格式，但是也使用了类似于 C 语言家族的习惯（包括 C、C++、C#、Java、JavaScript、Perl、Python 等）。这些特性使 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成(网络传输速率)。

在 360 企业版的接口中有大量的 JSON 使用，有些是 REST+JSON api，还有大部分不同应用、组件之间沟通的中间数据也是有 JSON 来完成的。由于他可读性、体积、编解码效率相比 XML 有很大优势，非常值得推荐。

# json 解析的异常捕获

首先来看最最普通的一个 json 解析的例子（被解析的 json 字符串是错误的，缺少一个双引号）：

```
-- http://www.kyne.com.au/~mark/software/lua-cjson.php
-- version: 2.1 devel

local json = require("cjson")
local str  = [[ {"key:"value"} ]]

local t    = json.decode(str)
ngx.say(" --> ", type(t))

-- ... do the other things
ngx.say("all fine") 
```

代码执行错误日志如下：

```
2015/06/27 00:01:42 [error] 2714#0: *25 lua entry thread aborted: runtime error: ...ork/git/github.com/lua-resty-memcached-server/t/test.lua:8: Expected colon but found invalid token at character 9
stack traceback:
coroutine 0:
    [C]: in function 'decode'
    ...ork/git/github.com/lua-resty-memcached-server/t/test.lua:8: in function <...ork/git/github.com/lua-resty-memcached-server/t/test.lua:1>, client: 127.0.0.1, server: localhost, request: "GET /test HTTP/1.1", host: "127.0.0.1:8001" 
```

这可不是我们期望的，decode 失败，居然 500 错误直接退了。改良了一下我们的代码：

```
local json = require("cjson")

function json_decode( str )
    local json_value = nil
    pcall(function (str) json_value = json.decode(str) end, str)
    return json_value
end 
```

如果需要在 Lua 中处理错误，必须使用函数 pcall（protected call）来包装需要执行的代码。 pcall 接收一个函数和要传递给后者的参数，并执行，执行结果：有错误、无错误；返回值 true 或者或 false, errorinfo。pcall 以一种"保护模式"来调用第一个参数，因此 pcall 可以捕获函数执行中的任何错误。有兴趣的同学，请更多了解下 Lua 中的异常处理。

另外，可以使用 CJSON 2.1.0，该版本新增一个 cjson.safe 模块接口，该接口兼容 cjson 模块，并且在解析错误时不抛出异常，而是返回 nil。

```
local json = require("cjson.safe")
local str  = [[ {"key:"value"} ]]

local t    = json.decode(str)
if t then
    ngx.say(" --> ", type(t))
end 
```

# 稀疏数组

请看示例代码（注意 data 的数组下标）：

```
-- http://www.kyne.com.au/~mark/software/lua-cjson.php
-- version: 2.1 devel

local json = require("cjson")

local data = {1, 2}
data[1000] = 99

-- ... do the other things
ngx.say(json.encode(data)) 
```

运行日志报错结果：

```
2015/06/27 00:23:13 [error] 2714#0: *40 lua entry thread aborted: runtime error: ...ork/git/github.com/lua-resty-memcached-server/t/test.lua:13: Cannot serialise table: excessively sparse array
stack traceback:
coroutine 0:
    [C]: in function 'encode'
    ...ork/git/github.com/lua-resty-memcached-server/t/test.lua:13: in function <...ork/git/github.com/lua-resty-memcached-server/t/test.lua:1>, client: 127.0.0.1, server: localhost, request: "GET /test HTTP/1.1", host: "127.0.0.1:8001" 
```

如果把 data 的数组下标修改成 5，那么这个 json.encode 就会是成功的。 结果是：[1,2,null,null,99]

为什么下标是 1000 就失败呢？实际上这么做是 cjson 想保护你的内存资源。她担心这个下标过大直接撑爆内存（贴心小棉袄啊）。如果我们一定要让这种情况下可以 decode，就要尝试 encode_sparse_array api 了。有兴趣的同学可以自己试一试。我相信你看过有关 cjson 的代码后，就知道 cjson 的一个简单危险防范应该是怎样完成的。

# 空 table 编码为 array 还是 object

# 编码为 array 还是 object

首先大家请看这段源码：

```
-- http://www.kyne.com.au/~mark/software/lua-cjson.php
-- version: 2.1 devel

local json = require("cjson")
ngx.say("value --> ", json.encode({dogs={}})) 
```

输出结果

> value --> {"dogs":{}}

注意看下 encode 后 key 的值类型，"{}" 代表 key 的值是个 object，"[]" 则代表 key 的值是个数组。对于强类型语言(c/c++, java 等)，这时候就有点不爽。因为类型不是他期望的要做容错。对于 Lua 本身，是把数组和字典融合到一起了，所以他是无法区分空数组和空字典的。

参考 openresty-cjson 中额外贴出测试案例，我们就很容易找到思路了。

```
-- 内容节选 lua-cjson-2.1.0.2/tests/agentzh.t
=== TEST 1: empty tables as objects
--- lua
local cjson = require "cjson"
print(cjson.encode({}))
print(cjson.encode({dogs = {}}))
--- out
{}
{"dogs":{}}

=== TEST 2: empty tables as arrays
--- lua
local cjson = require "cjson"
cjson.encode_empty_table_as_object(false)
print(cjson.encode({}))
print(cjson.encode({dogs = {}}))
--- out
[]
{"dogs":[]} 
```

综合本章节提到的各种问题，我们可以封装一个 json encode 的示例函数：

```
function json_encode( data, empty_table_as_object )
  --Lua 的数据类型里面，array 和 dict 是同一个东西。对应到 json encode 的时候，就会有不同的判断
  --对于 linux，我们用的是 cjson 库：A Lua table with only positive integer keys of type number will be encoded as a JSON array. All other tables will be encoded as a JSON object.
  --cjson 对于空的 table，就会被处理为 object，也就是{}
  --dkjson 默认对空 table 会处理为 array，也就是[]
  --处理方法：对于 cjson，使用 encode_empty_table_as_object 这个方法。文档里面没有，看源码
  --对于 dkjson，需要设置 meta 信息。local a= {}；a.s = {};a.b='中文';setmetatable(a.s,  { __jsontype = 'object' });ngx.say(comm.json_encode(a))

    local json_value = nil
    if json.encode_empty_table_as_object then
        json.encode_empty_table_as_object(empty_table_as_object or false) -- 空的 table 默认为 array
    end
    if require("ffi").os ~= "Windows" then
        json.encode_sparse_array(true)
    end
    pcall(function (data) json_value = json.encode(data) end, data)
    return json_value
end 
```

# 跨平台的库选择

大家看过上面三个 json 的例子就发现，都是围绕 cjson 库的。原因也比较简单，就是 cjson 是默认绑定到 openresty 上的。所以在 linux 环境下我们也默认的使用了他。在 360 天擎项目中，linux 用户只是很少量的一部分。大部分用户更多的是 windows 操作系统，但 cjson 目前还没有 windows 版本。所以对于 windows 用户，我们只能选择 dkjson（编解码效率没有 cjson 快，优势是纯 Lua，完美跨任何平台）。

并且我们的代码肯定不会因为 win、linux 的并存而写两套程序。那么我们就必须要把 json 处理部分封装一下，隐藏系统差异造成的差异化处理。

```
local _M = { _VERSION = '1.0' }
-- require("ffi").os 获取系统类型
local json = require(require("ffi").os == "Windows" and "dkjson" or "cjson")

function _M.json_decode( str )
    return json.decode(str)
end
function _M.json_encode( data )
    return json.encode(data)
end

return _M 
```

在我们的应用中，对于操作系统版本差异、操作系统位数差异、同时支持不同数据库使用等，几乎都是使用这个方法完成的，十分值得推荐。

额外说个点，github 上有个项目[cloudflare/lua-resty-json](https://github.com/cloudflare/lua-resty-json)，从官方资料上介绍 decode 的速度更快，我们也做了小范围应用。所以上面的 json_decode 对象来源，就可以改成这个库。

外面总是有新鲜玩意，多抬头多发现，充实自己，站在巨人肩膀上，总是能够更容易够到高峰。