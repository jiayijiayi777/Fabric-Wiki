# fabric.mod.json
fabric.mod.json文件是Fabric Loader用于加载mod的mod元数据文件。为了加载,mod必须将此文件的确切名称放在mod JAR的根目录中。
## 必须填写
> * schemaVersion需要内部机制.必须永远`1`.
> * id定义mod的标识符 - 拉丁字母,数字,下划线的字符串,长度从1到63.
> * version定义mod的版本 - 一个字符串值,可选地与Semantic Versioning 2.0.0规范相匹配.
## 可选字段
### Mod加载
> * environment：定义mod运行的位置：仅在客户端（客户端mod）,仅在服务器端（插件）或两端（常规mod）.包含环境标识符：
>> * *到处运行.默认.
>> * 客户端在客户端运行.
>> * 服务器在服务器端运行.
> * entrypoints定义将加载的mod的主要类.
>> * 你的mod有3个默认入口点:
>>> * main将首先运行.对于实现的类ModInitializer.
>>> * 客户端将在第二个运行,仅在客户端运行.对于实现的类ClientModInitializer.
>>> * 服务器将在第二个运行,仅在服务器端运行.对于实现的类DedicatedServerModInitializer.
>> * 每个入口点可以包含任意数量的要加载的类.可以通过两种方式定义类（或方法或静态字段）:
>>> * 如果您使用的是Java,那么只需列出类（或其他）全名.例如:
```
"main": [
    "net.fabricmc.example.ExampleMod",
    "net.fabricmc.example.ExampleMod::handle"
]
```
* 如果您使用的是与Java兼容且具有Fabric适配器的任何其他语言,则应使用以下语法:
```
"main": [
   {
      "adapter": "kotlin",
      "value": "package.ClassName"
   }
]
```
* jars要加载的mod的JAR中的嵌套JAR列表.在使用该字段之前,请查看有关嵌套JAR使用的指南.每个条目都是一个包含file密钥的对象.这应该是mod的JAR中嵌入JAR的路径.例如:
```
"jars": [
   {
      "file": "nested/vendor/dependency.jar"
   }
]
```
* languageAdapters用于其语言及其适配器类全名的适配器的字典.例如:
```
"languageAdapters": {
   "kotlin": "net.fabricmc.language.kotlin.KotlinAdapter"
}
```
>> * mixins mixin配置文件列表.每个条目都是mod的JAR中包含mixin配置文件的路径或包含以下字段的对象:
>>> * config mod的JAR中mixin配置文件的路径.
>>> * 环境与上层环境字段相同.往上看.例如:
```
"mixins": [
   "modid.mixins.json",
   {
      "config": "modid.client-mixins.json",
      "environment": "client"
   }
]
```
### 依赖性解决方案
下面对象的每个条目的键是依赖项的Mod ID.

每个键的值是一个字符串或字符串数​​组,用于声明支持的版本范围.在数组的情况下,假设“OR”关系 - 也就是说,只有一个范围必须匹配要满足的集体范围.

对于所有版本,*是一个特殊字符串,声明任何版本都与范围匹配.此外,无论版本类型如何,都必须能够进行精确的字符串匹配.

下面对象的每个条目的键是依赖项的Mod ID.

每个键的值是一个字符串或字符串数​​组,用于声明支持的版本范围.在数组的情况下,假设“OR”关系 - 也就是说,只有一个范围必须匹配要满足的集体范围.

对于所有版本,*是一个特殊字符串,声明任何版本都与范围匹配.此外,无论版本类型如何,都必须能够进行精确的字符串匹配.

* 依赖于运行所需的依赖项.没有它们,游戏就会崩溃.
* 建议对于不需要运行的依赖项.如果没有它们,游戏将记录警告.
* 建议对于不需要运行的依赖项.将此用作一种元数据.
* 休息对于你一起,其MODS的可能会导致游戏崩溃.有了它们,游戏就会崩溃.
* 冲突对于与你的一起造成某种错误的mod,等等.对于他们,游戏将记录警告.
### 元数据
* name定义用户友好的mod名称.如果不存在,则假设它与id匹配.
* description定义mod的描述.如果不存在,则假设为空字符串.
* contact定义项目的联系信息.它是以下领域的对象:
> * 电子邮件联系与mod有关的电子邮件.必须是一个有效的E-mail地址.
> * 与mod有关的irc IRC频道.必须是有效的URL格式 - 例如:[`irc://irc.esper.net:6667/charset`](irc://irc.esper.net:6667/charset)对于#charsetEsperNet - 端口是可选的,如果不存在则假定为6667.
> * 主页项目或用户主页.必须是有效的HTTP / HTTPS地址.
> * 问题项目问题跟踪器.必须是有效的HTTP / HTTPS地址.
> * 源项目源代码库.必须是有效的URL - 但它可以是给定VCS（例如Git或Mercurial）的专用URL.
> * 该列表并非详尽无遗 - mod可能会提供额外的非标准密钥（例如discord,slack,twitter等） - 如果可能,它们应该是有效的URL.
* 作者 mod的作者列表.每个条目都是单个名称或包含以下字段的对象:
> * name人员的真实姓名或用户名.强制性.
> * 联系的联系信息.与上层联系人相同.往上看.可选的.
* 贡献者 mod的贡献者列表.每个条目与作者字段中的条目相同.往上看.
* license定义许可信息.可以是单个许可证字符串,也可以是它们的列表.
> * 这应该提供传送整个mod包的完整的首选许可证集.换句话说,对所有列出的许可证的合规性应该足以使整个mod包的使用,再分配等.
> * 对于部分代码是双重许可的情况,请选择首选许可证.该列表并非详尽无遗,主要作为一种提示,并不会阻止您根据具体情况授予其他权利/许可.
> * 为了帮助自动化工具,建议将SPDX许可证标识符用于开源许可证.
* icon定义mod的图标.图标是方形PNG文件.（Minecraft资源包使用128×128,但这并不是一项严格的要求 - 但建议使用2的幂.）可以提供以下两种形式之一:
> * 单个PNG文件的路径.
> * 图像字典宽度到文件路径的字典.
## 自定义字段
您可以在字段中添加要添加的任何`custom`字段.Loader会忽略它们.但是,如果将字段（名称）添加到标准规范中,强烈建议对字段命名空间以避免冲突.