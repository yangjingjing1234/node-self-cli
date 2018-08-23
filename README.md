# node-self-cli

## 知识点
* npm link
* node的cli调用

## 参考文档
http://javascript.ruanyifeng.com/nodejs/packagejson.html

######  CLI 跟GUI
* 命令解释程序也可以翻译为命令行用户界面（command-line interface），与图形用户界面（graphical user interface）并列作为人机交互界面的两种实现方式


> Node.js分2种模块
> 
> 普通模块，供代码调用
>
> 二进制模块，提供cli调用
>
> 大家都知道，生成器是cli工具，所以我们应该使用cli二进制模块
>
> npm init 初始化一个模块生成package.json

* 几个比较重要的字段：name,version,main,bin。

> name是表示包发布的名字；
>
> version是每次发布的时候要修改版本好才能生效（供npm识别），因为每次都要修改，所以建议自动化处理；
>
> main是项目的入口（别人以npm或commonJS方式来使用），
>
> bin 用来指定各个内部命令对应的可执行文件的位置。(注：没有后缀的一个文件 `（#!/usr/bin/env node）` 这是此文件的标识，并不是注释)
>
>  #! 告诉系统其后路径所指定的程序即是解释此脚本文件的 node 程序。(这句话解释的有误)
>
这里主要增加里一个bin的配置，bin里的gen为cli的具体命令，它的具体执行的文件gen.js，

```
package.json 文件
{
  "name": "a",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "bin": {
    "gen": "bin/gen"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

```
{
    "name": "yjjtest",
    "version": "0.0.1",
    "description": "前端写作开发流程的脚手架",
    "main": "index.js",
    "bin": "bin/init", // 安装之后执行 yjjtest 即为执行命令
    // 或者以下写法
    "bin":{
        "yjjself":"bin/init",// 给入口命令指定别名 ，安装之后执行  yjjself 即可
    }
}
    
```

* 创建入口文件gen
如果是windwo 应该这样填写：#! E:\nodejs\node

```
#!/usr/bin/env node

  var argv = process.argv;
  var filePath = __dirname;
  var currentPath = process.cwd();

  console.log(argv)
  console.log(filePath)
  console.log(currentPath)
```
说明

* argv是命令行的参数

* filePath是当前文件的路径，也就是以后安装后文件的路径，用于存放模板文件非常好

* currentPath是当前shell上下文路径，也就是生成器要生成文件的目标位置

### 本地安装此模块

在package.json文件路径下，执行

```
npm link
```

/Users/sang/.nvm/versions/node/v4.4.5/bin/gen -> /Users/sang/.nvm/versions/node/v4.4.5/lib/node_modules/a/gen.js/Users/sang/.nvm/versions/node/v4.4.5/lib/node_modules/a -> /Users/sang/workspace/github/i5ting/a

此时说明已经安装成功了。

* 执行gen测试
```
gen
```



## npm link 使用场景
我理解的就是一个当前模块要想建立全局关联的时候使用
在多模块开的时候，联调比较麻烦，改动一个模块就需要重新安装，使用npm link 之后 每个修改嵌套模块之间都是相互关联的
多模块开发联调的时候使用非常方便
test-example 使用需要 test模块
![avatar](https://github.com/yangjingjing1234/node-self-cli/blob/master/11111.png)

每个应用都用到 Coffee-script
![avatar](https://github.com/yangjingjing1234/node-self-cli/blob/master/222.png)


* 我们假设项目是 my-project, 需要用到一个独立的 my-utils 模块
直接用相对路径安装
```
$ cd path/to/my-project
$ npm install path/to/my-utils
```
优点：简单明了
缺点： 调试过程中往往需要微调，此时需要切换到 my-utils 目录修改，然后反复重新 install，很麻烦

npm link

```
$ cd path/to/my-project
$ npm link path/to/my-utils
```
如果这两种的目录不在一起，那还有一种方法：

```
$ # 先去到模块目录，把它 link 到全局
$ cd path/to/my-utils
$ npm link
$
$ # 再去项目目录通过包名来 link
$ cd path/to/my-project
$ npm link my-utils
```



