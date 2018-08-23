# node-self-cli

## 知识点
* npm link
* node的cli调用

## 参考文档
http://javascript.ruanyifeng.com/nodejs/packagejson.html

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
test-example 使用需要 test模块
![avatar](https://github.com/yangjingjing1234/node-self-cli/blob/master/11111.png)

每个应用都用到 Coffee-script
![avatar](https://github.com/yangjingjing1234/node-self-cli/blob/master/222.png)
