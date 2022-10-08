# 1.初识TypeScript

## TypeScript的介绍

TypeScript是一种由微软开发的开源，跨平台的编程语言，他是Javascript的超集，最终会被编译为JavaScript代码

2012年10月，微软发布了首个公开版本的TypeScript，2013年6月19日，在经历了一个预览版之后微软正式发布了正式版TypeScript。

TypeScript的作者是**安德斯·海尔斯伯格**，C#的首席架构师，他是开源和跨平台的编程语言。

TypeScript扩展了JavaScript的语法，所以任何现有的JavaScript程序可以运行在TypeScript环境中。

TypeScript是为大型应用的开发而设计，并且可以编译为JavaScript。

TypeScript是JavaScript的一个超集，主要提供了类型系统和对ES6+的支持**，它由Microsoft开发，代码开源于GitHub上。

## TypeScript的特点

TypeScript主要有3大特点：

- **始于JavaScript，归于JavaScript**

  TypeScript可以编译出纯净、简介的JavaScript代码，并且可以运行在任何浏览器上，Node.js环境中和任何支持ECMAScript3（或更高版本）的JavaScript引擎中

- **强大的类型系统**

  **类型系统**允许JavaScript开发者在开发JavaScript应用程序时使用高效的开发工具和常用操作比如静态检查和代码重构

- **先进的JavaScript**

  TypeScript提供最新的和不断发展的JavaScript特性，包括那些来自2015年的ECMAScript和未来提案中的特性，比如异步功能和Decorators，以帮助建立健壮的组件

## 总结

TypeScript在社区的流行度越来越高，他非常适用于一些大型项目，也非常适用于一些基础库，极大地帮助我们提升了开发效率和体验

# 2.安装TypeScript

命令行运行如下命令，全局安装TypeScript：

```
npm install -g typescript
```

安装完成后，在控制台运行如下命令，检查安装是否成功：

```
tsc -v
```

# 3.第一个TypeScript程序

## 编写TS程序

src/helloworld.ts

```typescript
function greeter(person){
    return 'Hello,' + person
}
let user = 'Yee'

console.log(greeter(user))
```

## 手动编译代码

我们使用了[`.ts`]()拓展名，但是这段代码仅仅是JavaScript而已

在命令行上，运行TypeScript编译器

```
tsc helloworld.ts
```

输出结果为一个`helloworld.js`文件，它包含了和输出文件中相同的JavaScript代码

在命令行上，通过Node.js运行这段代码

```
node helloword.js
```

控制台输出

```
Hello,Yee
```

## vscode自动编译

```
1).生成配置文件tsconfig.json
	tsc --init
2).修改tsconfig.json配置
	"outDir":"./js",
	"strict":false,
3).启动监视任务
	终端->运行任务->监视tsconfig.json
```

## 类型注解

接下来让我们看看Typescript工具带来的高级功能。给`person`函数的参数添加`:string`类型注解。

如下：

```typescript
function greeter(person:string){
	return 'Hello,' + person
}
let user = 'Yee'

console.log(greeter(user))
```

Typescript里的类型注解是一种轻量级的为函数或变量添加约束的方式。在这个例子里，我们希望`greeter`函数接受一个字符串参数。然后并尝试把`greeter`的调用改成传入一个数组：

```typescript
function greeter(person:string){
	return 'Hello,' + person
}
let user = [0,1,2]
console.log(greeter(user))
```

重新编译 ，你会看到产生了一个错误：

```
error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

类似的，尝试删除`greeter`调用的所有参数。Typescript会告诉你使用了非期望个数的参数调用了这个函数。在这两种情况中，Typescript提供了静态的代码分析，它可以分析结构代码和提供的类型注解。

要注意的是尽管有错误，`greeter.js`文件还是被创建了，就算你的代码有错误，你仍然可以使用Typescript，但在这种情况下，Typescript会警告你代码可能不会按预期执行

## 接口

让我们继续扩展整个示例应用。这里我们使用接口来描述一个拥有`firstName`和`lastName`字段对象。在`Typescript`里，只在两个类型内部的结构兼容，那么这两个类型就是兼容的。这就允许我们在实现接口时候只要保证包含接口要求的结构就行，而不必明确的使用`implements`语句

```typescript
interface Person{
	firstName:string
    lastName:string
}
function greeter(person:Person){
    return 'Hello,' + person.firstName + '-' + person.lastName
}
let user = {
    firstName:'Yee',
    lastName:'Huang'
}
console.log(greeter(user))
```

## 类

最后，让我们使用类来改写这个例子。TypeScript支持Javascript的特性，比如支持基于类的面向对象编程。

让我们创建一个`User`类，他带有一个构造函数和一些公共字段。因为类的字段包含了接口所需要的字段，所以他们能很好的兼容。

还要注意的是，我在类的声明上会注明所有的成员变量，这样比较一目了然。

```typescript
class User{
        firstName:string //姓氏
        lastName:string //名字
        fullName:string //姓名
        //定义一个构造器函数
        constructor (firstName:string,lastName:string){
            //更新属性数据
            this.firstName = firstName
            this.lastName = lastName
            this.fullName = firstName + ' ' + lastName
        }
    }
interface Person{
    firstName:string
    lastName:string
}
function greeter(person:Person){
    return 'Hello,'+ person.firstName + ' ' + person.lastName
}
ler user = new User('Yee','Huang')
console.log(greeter(user))
```

重新运行`tsc greeter.ts`，你会看到Typescript里的类只是一个语法糖，本质上还是JavaScript函数的实现

## 总结

到这里，你已经对Typescript有了一个大致的印象，那么下一章让我们来一起学习Typescript的一些常用语法吧。

# 4.使用webpack打包TS

## 下载依赖

```
yarn add -D typescript
yarn add -D webpack webpack-cli
yarn add -D webpack-dev-server
yarn add -D html-webpack-plugin clean-webpack-plugin
yarn add -D ts-loader
yarn add -D cross-env
```

## 入口JS: src/main.ts

```typescript
// import './01_helloworld'

document.write('Hello Webpack TS!')
```

## index页面: public/index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>webpack & TS</title>
</head>
<body>
  
</body>
</html>
```

## build/webpack.config.js

```javascript
const {CleanWebpackPlugin} = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const path = require('path')

const isProd = process.env.NODE_ENV === 'production' // 是否生产环境

function resolve (dir) {
  return path.resolve(__dirname, '..', dir)
}

module.exports = {
  mode: isProd ? 'production' : 'development',
  entry: {
    app: './src/main.ts'
  },

  output: {
    path: resolve('dist'),
    filename: '[name].[contenthash:8].js'
  },

  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: 'ts-loader',
        include: [resolve('src')]
      }
    ]
  },

  plugins: [
    new CleanWebpackPlugin({
    }),

    new HtmlWebpackPlugin({
      template: './public/index.html'
    })
  ],

  resolve: {
    extensions: ['.ts', '.tsx', '.js']
  },

  devtool: isProd ? 'cheap-module-source-map' : 'cheap-module-eval-source-map',

  devServer: {
    host: 'localhost', // 主机名
    stats: 'errors-only', // 打包日志输出输出错误信息
    port: 8081,
    open: true
  },
}
```

## 配置打包命令

```text
"dev": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.js",
"build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
```

## 运行与打包

```text
yarn dev
yarn build
```
