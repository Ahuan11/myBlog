## Node.js简介

- Node.js是一个能偶在服务器端运行Javascript的开放源代码、跨平台JavaScript运行环境
- Node采用Google开发的V8引擎运行js代码，使用事件驱动、非阻塞和异步I/O模型等技术来提高性能，可优化应用程序的传输量和规模
- Node大部分基本模块都用Javascript编写。在Node出现之前，JS通常作为客户端程序设计语言使用，以JS写出的程序常在用户的浏览器上运行
- 目前，Node已被IBM、Microsoft、Yahoo!、Walmart、Groupon、SPA、Linkedin、Rakuten、PayPal、Voxer和GoDaddy等企业采用
- 传统服务器是多线程的，每进来一个请求，就创建一个线程去处理请求。而**Node**的服务器是**单线程**的，但是在后台用有一个**I/O线程池**

- Node的历史：
- ![image-20221016143254099](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/image-20221016143254099.png)

## 模块化

- 在Node中，一个js文件就是一个模块

- 在Node中，**每一个js文件中的js代码都是独立运行在一个函数中**

  ​	而不是全局作用域，所以一个模块的中和变量和函数在其他模块中无法访问

  ```javascript
  console.log(arguments.callee + "");    //可以看到该js文件的函数
  ```

  

- 我们可以通过exports来向外部暴露变量和方法

  ​				只需要将需要暴露给外部的变量或方法设置为exports的属性即可

## 引入其他模块

```javascript
/*
	在node中，通过require()函数来引入外部的模块
		require()可以传递一个文件的路径作为参数，node将会自动根据该路径来引入外部模块
		这里的路径，如果使用相对路径，必须以./或者 ../开头
	使用require()引入模块以后，该函数会返回一个对象，这个对象代表的是引入的模块
*/
var md = require("./02.module.js");
```

模块分为两大类：

- 核心模块

  - ​	由node引擎提供的模块

  - ​	核心模块的标识就是，模块的名字

- 文件模块

  -   由用户自己创建的模块

  - 文件模块的标识就是文件的路径（绝对路径、相对路径）

    ​		相对路径使用 ./或者 ../开头

## 模块化详解

- 在node中有一个全局对象global，它的作用和网页中window类似

  - 在全局中创建的变量都会作为global的属性保存
  - 在全局中创建的函数都会作为global的方法保存

- 当Node在执行模块中的代码时，他会首先在此js文件的代码最外层，套如下代码

  ```javascript
  function (exports, require, module, __filename, __dirname) {
  
  
  }
  ```

- 实际上模块中的代码都是包裹在一个函数中执行的，并且在函数执行时，同时传递进了5个实参

  ```javascript
  exports	  		
  //该对象用来将变量或函数暴露到外部
  require			
  //函数，用来引入外部的模块
  module			
  //  --module代表的时当前模块本身
  //  --exports就是module的属性
  //  --既可以使用exports导出，也可以使用module.exports导出
  __filename
  //c:\Users\86158\Desktop\hello\hello.js
  //当前模块的完整路径（绝对路径）
  __dirname
  //c:\Users\86158\Desktop\hello
  //当前模块所在文件夹的完整路径（绝对路径）
  ```

  

## 	exports和module.exports

```javascript
module.exports = {
	name:'猪八戒',
	age:300,
	sayName:function(){
		console.log("我是猪八戒");
	}
}

//可以！！

module.exports.a=10   //可以！！
```

```javascript
exports = {
	name:'猪八戒',
	age:300,
	sayName:function(){
		console.log("我是猪八戒");
	}
}
//会报错！！！

exports.a=10   //可以！！
```

> exports   和  module.exports
>
> ​			----通过exports只能使用.的方式向外暴露内部变量
>
> ​			----而module.exports既可以通过.的形式，也可以直接赋值

## 包  package

- CommonJS的包规范允许我们将一组相关的模块组合到一起，形成一组完整的工具

- CommonJS的包规范由包结构和包描述文件两个部分组成。

- 包结构

  - ​	用于组织包中的各种文件

  - 包实际上就是一个压缩文件，解压以后怀远为目录。符合规范的目录，应该包含如下文件：

    - **package.json    描述文件    必须的**    不能有任何注释    JSON格式的文件
    - bin     		可执行二进制文件
    - lib				js代码
    - doc				文档
    - test			单元测试

    

- 包描述文件
  - 描述包的相关信息，以供外部读取分析

## NPM  （Node Package  Manager）

- CommonJS包规范是理论，NPM是其中一种实践
- 对于Node而言，NPM帮助其完成了第三方模块的发布、安装和依赖等。借助NPM、
  Node与第三方模块之间形成了很好的一个生态系统

- **NPM命令**

  ```javascript
  //查看npm版本
  npm -v
  //查看所有模块的版本
  npm version
  //搜索包
  npm search  xxx
  //安装包
  npm install / i  xxx
  //安装包并添加到依赖中
  npm install  xxx --save 	//重要！！很常用
  //删除包
  npm remove / r xxx
  //下载当前这个项目依赖的包
  npm install
  //全局安装包
  npm install xxx -g   //全局安装的包一般都是一些工具，在计算机中用的，不在项目中使用
  
  ```

  

## CNPM   （China  Node Package  Manager）

CNPM 是中国 npm 镜像的客户端。

```
npm install cnpm -g --registry=https://registry.npm.taobao.org
```

## node搜索包的流程

- 通过npm下载的包都放在node_modules文件夹中，我们通过npm下载的包，直接通过包名引入即可

  ```javascript
  var math = require("math");
  console.log(math.add(12,34));
  ```

- node在使用模块名字引入模块时，他会首先在当前目录的node_modules中寻找是否含有该模块

  如果有，则直接使用，如果没有则去上一级目录的node_modules中寻找

  如果有则直接使用，如果没有则再去上一级目录寻找，直到找到为止

  直到找到磁盘的根目录，如果依然没有，则报错

## Buffer缓冲区

- Buffer的结构和数组很像，操作的方法也和数组类似
- 实际上Buffer中的内存不是通过JavaScript分配的，而是在底层通过c++申请的
- 数组中不能存储二进制文件，而Buffer就是抓门用来存储二进制数据的
- 使用buffer不需要引入模块，直接使用即可

- 在buffer中存储的数据都是二进制数据，但是再显示时都是以16进制的形式显示

​						buffer中每一个元素的范围是从00 -- ff     0-225

​						00000000 - 111111111

​						计算机中  一个0  或一个1  我们称为一位（bit）

​						8bit = 1 byte（字节）

​						1024byte = 1kb

​						1024kb = 1mb

​						1024mb = 1gb

​						1024gb = 1tb

​						buffer中的一个元素，占用内存的一个字节，一个汉字占用3个字节

- Buffer的大小一旦确定，不能修改，**Buffer实际上是对底层内存的直接操作**

```javascript
var str = "Hello 尚硅谷"
//将一个字符串保存到buffer中
var buf = Buffer.from(str);
console.log(buf.length);//占用内存的大小 ----15
console.log(str.length);//字符串的长度   ---9
```

- 创建一个指定大小的buffer

  **不推荐方法（buffer构造函数）：**

  ```javascript
  var buf2 = new Buffer(10)   //10个字节的buffer
  //注意：buffer构造函数都是不推荐使用的！！！！！
  console.log(buf2)   //10
  ```

  **推荐方法（Buffer.alloc）**：

  ```javascript
  //创建一个10个字节的buffer
  var buf2 = Buffer.alloc(10);
  console.log(buf2)   //	<Buffer 00 00 00 00 00 00 00 00 00  00>
  
  //通过索引，来操作buf中的元素
  buf2[0] = 88;//（10进制）
  buf2[1] = 255;//(10进制)
  buf2[2] = 0xaa; //(16进制）
  console.log(buf2)   //	<Buffer 58(16进制) ff aa 00 00 00 00 00  00>
  ```

  **不推荐方法（Buffer.allocUnsafe(size)）：**

  ```javascript
  //Buffer.allocUnsafe(size)  创建一个 指定大小的buffer，但是buffer中可能含有敏感数据
  var buf3 = Buffer.alllocUnsafe(10);
  console.log(buf3)	//<Buffer 18 1e 4a 00 00 00 00 00 90 3c>
  //分配空间没有清空数
  ```

- **buffer的常用方法**

  ```
  Buffer.from(str) //将一个字符串转换为buffer
  Buffer.alloc(size) //创建一个指定大小的buffer
  Buffer.allocUbsafe(size) //创建一个指定大小的buffer，可能包含敏感数据
  buf.toString()   //将缓冲区中的数据转换为字符串
  ```

  

## fs文件系统

- 在node中，与文件系统的交互是非常重要的，服务器的本质就将本地的文件发送给远程的客户端

- node通过fs模块来和文件系统进行交互

- 该模块提供了一些标准文件访问API来打开、读取、写入文件、以及与其交互

- 要使用fs模块，首先需要对其进行加载(  **fs是核心模块，不需要下载** )

  ```javascript
  const fs = require("fs")
  ```

- **同步和异步调用**

  - fs模块中所有的操作都有两种形式可供选择  **同步**和 **异步**
  - 同步文件系统会阻塞程序的执行，也就是 除非操作完毕，否则不会向下执行代码
  - 异步文件系统不会阻塞程序的执行，而是在操作完成时，通过回调函数将结果返回



### 同步文件写入

1. 打开文件

   ```javascript
   fs.openSync(path,flags[,mode])
   ```

   ​				——  path  要打开文件的路径

   ​				——  flags	打开文件要做的操作类型

   ​								r   只读的

   ​								w   可写的

   ​				—— mode 设置文件的操作权限，一般不传

   ​				— 返回值：该方法会返回一个文件的描述符作为结果，我们可以通过该描述符来对文件进行各种操作

   

2. 向文件中写入内容

   ```
   fs.writeSync(fd,string[,position[,encoding]])
   ```

   ​				—— fd  	文件的描述符

   ​				——string  		要写入的内容

   ​				——position      写入的起始位置

   ​				——encoding		写入的编码 默认是utf-8

   ------

3. 保存并关闭文件

   ```
   fs.closeSync(fd)
   ```

   

```javascript
var fs = require("fs");
//打开文件
var fd = fs.openSync("hello.txt", "w");
//向文件中写入内容
fs.writeSync(fd,"今天的天气真不错~~~");
//关闭文件
fs.closeSync(fd);
```

### 异步文件写入



- ```javascript
  fs.open(path,flags[,mode],callback)
  ```

  - 用来打开一个文件
  - 异步调用的方法，结果都是通过回调函数返回的
  - 回调函数两个参数
    - err   错误对象，如果没有错误则为null
    - fd    文件的描述符

- ```javascript
  fs.write(fd,string[,position[,encoding]],callback)
  ```

  - ​	用来异步写入一个文件

- ```javascript
  fs.close(fd,callback)
  ```

  - 用来关闭文件

```
//引入fs模块
let fs = require("fs");
//打开文件
fs.open("hello.txt","w",function(err,fd){
    //判断是否出错
    if(!err){
        //如果没有出错，则对文件进行写入操作
        fs.write(fd,"哈哈哈哈",function (err) {
            if(!err)
            {
                console.log("写入成功");
            }
            //关闭文件
            fs.close(fd,function (err) {
                if(!err)
                    console.log("文件已关闭");
              })
          })
    }
});
```

### 简单文件写入

- 异步

  ```
  fs.writeFile(file,data[,options],callback)
  ```

- 同步

  ```
  fs.writeFileSync(file,data[,options])
  ```

  ```javascript
  file  //要操作的文件路径
  data   //要写入的数据
  options  //选项，可以写入进行一些设置
  callback  //当写入完成以后执行的函数
  flag    // r 只读
  		// w可写
  		//  a 追加
  ```

  

![image-20221017220938496](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/image-20221017220938496.png)

### 流式文件写入

> 同步、异步、简单文件写入都不适合大文件的写入，性能比较差，容易导致内存溢出

- 创建一个可写流

  ```
  fs.createWriteStream(path[,options])
  ```

  ```
  path  //文件路径
  options  //配置的参数
  ```

- 写入内容

  ```javascript
  //通过ws向文件中输出内容
  ws.write("通过可写流写入文件的内容");
  ws.write("哈哈哈哈哈哈哈");
  ws.write("102932478334");
  ws.write("！！！！！");
  ```

- 关闭流

  ```javascript
  ws.end();
  //ws.close      //此处不能用close，否则将只执行第一行写操作
  ```

- one 与 once

  ```
  /*
      on(事件字符串，回调函数)
          - 可以为对象绑定一个事件
      once(时间字符串，回调函数)
          - 可以为对象绑定一个一次性的事件，该事件将会在触发依次以后自动失效
  */
  ws.once("open",function () { 
      console.log("流打开了");
   })
  
  ws.once("close",function () { 
      console.log("流关闭了");
   })
  ```

### 同步文件读取

### 异步文件读取

### 简单文件读取

- **同步**

  ```
  fs.readFileSync(path[,options])
  ```

- **异步**

  ```
  fs.readFile(path[,options],callback)
  ```

  ```
  path    //要读取的文件的路径
  options  //读取的选项
  callback	//回调函数，通过回调函数将读取到的内容返回(err,data)
  		//  err错误对象
  		//	data 读取到的数据，会返回一个buffer
  ```

### 流式文件读取

> 流式文件读取也适用于一些比较大的文件 ，可以分多次将文件读取到内存中

```javascript
var fs = require("fs");

//创建一个可读流
var rs = fs.createReadStream("1.jpg");

//创建一个可写流
var ws = fs.createWriteStream("2.jpg")

//监听流的开启和关闭
rs.once("open",function () {
    console.log("可读流打开了");
})
rs.once("close",function () { 
    console.log("可读流关闭了");
    //数据读取完毕，关闭可写流
    //ws.end();   //使用pipe，会自动关闭可写流
 })

ws.once("open",function () {
    console.log("可写流打开了");
})
ws.once("close",function () { 
    console.log("可读流关闭了");
 })

 //如果要读取一个可读流中的数据，必须要为可读流绑定一个data事件，data事件绑定完毕，它会自动开始读取数据
 /*
 rs.on("data",function (data) { 
    //将读取到的数据写入可写流中
    ws.write(data);
  })
*/

//pipe()可以将可读流中的内容，直接输出到可写流中
rs.pipe(ws);
```

### 其他操作

- 验证路径是否存在

  ```
  fs.existsSync(path)		//同步
  fs.exists(path,callback)  //已废弃
  ```

- 获取文件信息

  ```
  fs.stat(path,callback)  //异步
  fs.statSync(path)       //同步
  ```

- 删除文件

  ```
  fs.unlink(path,callback)	//异步
  fs.unlinkSync(path)		//同步
  ```

- 列出文件

  ```
  fs.readdir(path[,options],callback)
  fs.readdirSync(path[,options])
  ```

- 截断文件

  ```
  fs.truncate(path,len,callback)
  fs.truncateSync(path,len)
  ```

- 建立目录

  ```
  fs.mkdir(path[,mode],callback)
  fs.mkdirSync(path)
  ```

  
