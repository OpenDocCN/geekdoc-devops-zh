# React 中的外部化字符串——分步指南| Harness 的第 2 部分

> 原文：<https://www.harness.io/blog/externalizing-strings-in-react-part-2>

React 系列中外部化字符串的第 2 部分！下面是我们如何利用 TypeScript。

继续 React - [第 1 部分](https://harness.io/blog/externalizing-strings-in-react-1/)中的外部化字符串，我们将看看如何利用 TypeScript 来提供静态分析、验证和自动完成。

首先，我们必须编写一个节点脚本。这将读取一个“YAML”文件并生成类型。例如，如果我们有以下 YAML:

key1:值 1
key2:值 2
key 3:
key 3 _ 1:值 3 _ 1
key 3_2:值 3 _ 2

那么它应该产生以下类型:

键入 string key = ' key 1 ' | ' key 2 ' | ' key 3。键 3 _ 1“|”键 3。键 3 _ 2 '

注意，对于嵌套值，我们使用其父代的所有键(直到到达根)来生成键，用` .'分隔。这是 JavaScript 生态系统中遵循的一个通用约定，并受到诸如 [lodash](https://lodash.com/docs/4.17.15#get) 等库的支持。

接下来，我们可以在“StringsContext”文件中利用这种类型，并利用 TypeScript。

以下脚本应该读取 YAML 文件并生成类型:

从“fs”导入 fs；
从“路径”导入路径；
从“yaml”导入 YAML；

/**
*递归遍历对象并生成所有值的路径
* { foo: "bar "，foo2: { key1: "value1 "，key2: "value2" }，foo3: [1，2，3] }
*将给出结果:
*
* ["foo "，" foo2.key1 "，" foo2.key2 "，" foo3.0 "，" foo3.1 "，" foo3.2"]
*/
函数 createKeys(obj，objflatMap(([key，value])=>{
const obj path = initial path？` ＄{ initial path }。$ { key } `:key；

if(type of value = = = " object "&&value！== null) {
返回 createKeys(value，obj path)；
}
返回 objPath
})；
}

/**
*读取输入 YAML 文件并将类型写入输出文件
*/
异步函数 generateStringTypes(input，output){
const data = await fs . promises . readfile(input，" utf8 ")；
const JSON data = YAML . parse(data)；
const keys = create keys(JSON data)；

const typesData = ` export type string keys = \ n | " $ { keys . join(" " \ n | " ')} "；`;

await fs . promises . writefile(output，typesData，" utf8 ")；
}

const input = path . resolve(process . CWD()，“src/strings . YAML”)；
const output = path . resolve(process . CWD()，“src/strings . types . ts”)；

【generateStringTypes】(输入，输出)；

您可以将此脚本放在“scripts/generate-types.mjs”中，然后运行“node scripts/generate-types.mjs”。此外，您应该会看到“src/strings.types.ts”是用以下内容编写的:

导出类型 string keys =
| " home page title "
| " about page title "
| " home page content . para 1 "
| " home page content . para 2 "
| " home page content . para 3 "
| " about page content . para 1 "
| " about page content . para 2 "
| " about page content . para 3 "；

当前形式的脚本不能处理所有的用例/边缘情况。如果需要，您可以增强它，并根据您的使用情况进行定制。

现在，我们可以更新“StringsContext.tsx ”,以利用生成的类型“StringKeys”。

从“react”导入 React，{ create context }；
从“lodash.has”导入 has
从“lodash.get”导入 get
从“小胡子”导入小胡子；

+从“”导入类型{ StringKeys }。/strings . types "；

+导出类型 string map = Record<string keys，string>；

-const string context = create context({ } as any)；
+const strings context = create context<strings map>({ } as any)；

导出接口 StringsContextProviderProps {
-data:记录<字符串，任意>；
+data:strings map；
}

导出函数 StringsContextProvider(
道具:React。props with children<StringsContextProviderProps>
){
return(
<string context)。provider value = { props . data }>
{ props . children }
</string context。提供商>
)；
}

-导出函数 usestringcontext():Record<string，any > {
+导出函数 usestringcontext():strings map {
return react . use context(strings context)；
}

导出接口 UseLocaleStringsReturn {
-getString(key:string，variables？:any):string；
+ getString(key: StringKeys，variables？:any):string；
}

导出函数 useLocaleStrings(){
const strings = useStringsContext()；

return {
-getString(key:string，variables:any = { }):string {
+getString(key:string keys，variables:any = { }):string {
if(has(string，key)){
const str = get(strings，key)；

return mustache . render(str，变量)；
}

抛出新错误(` Strings data 没有定义为:" $ { key } " `)；
}，
}；
}

导出接口 LocaleStringProps extends React。html attributes<any>{
-strKey:string；
+strKey:string keys；
作为？:JSX 之钥。内在要素；
变量？:任何；
}

导出函数 LocaleString(props:LocaleStringProps):React。ReactElement { const { strKey，as，variables，...rest } =道具；
const { getString } = useLocaleStrings()；
const Component = as | | " span "；

返回<组件{...rest} > {getString(strKey，variables)}</Component>；
}

经过这一更改后，您应该能够使用 TypeScript 对存在字符串进行自动完成和验证。

此外，您可以将字符串生成集成到您的构建系统中。每当“strings.yaml”文件发生变化时，这将自动生成类型。我已经用一个 [vitejs 插件](https://vitejs.dev/guide/api-plugin.html)在这里完成了[。](https://github.com/vkbansal/string-externalisation/blob/1038cee182be3015f92c1b48d3922638422882c7/vite.config.js#L8)

结论

## 我希望您会发现这很有用，并将它作为您自己实现的起点。对于那些错过了第 1 部分的人，你可以在这里找到它:[在 React-Part 1](https://harness.io/blog/continuous-delivery/externalizing-strings-in-react-1/)中具体化字符串。

编码快乐！

Happy coding!