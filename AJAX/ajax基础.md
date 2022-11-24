# 第1章：原生AJAX

## AJAX简介

- AJAX全称为  Asynchronous JavaScript And XML，就是异步的JS 和 XML

- 通过AJAX可以在浏览器中向服务器发送异步请求，最大的优势：**无刷新获取数据**

- AJAX不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式

## XML简介

- XML可扩展标记语言
- XML被设计用来传输存储数据
- XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签

全部都是自定义标签，用来保存一些数据

```xml
<!-- 比如我有一个学生数据：
        name = '孙悟空';age = 18; gender = '男';
    用xml表示如下：
 -->
<student>
    <name>孙悟空</name>
    <age>18</age>
    <gender>男</gender>
</student>
```

​	**现在已经被JSON取代了**

用JSON表示 :

```json
{
    "name":"孙悟空",
    "age":18,
    "gender":"男"
}
```

## AJAX的特点

### AJAX的优点

- 可以无需刷新页面与服务器端进行通信
- 允许你根据用户事件来更新部分页面内容

### AJAX的缺点

- 没有浏览历史,不能回退
- 存在跨域问题(同源)
- SEO不友好

## AJAX的使用

### 核心对象

- `XMLHttpRequest`,AJAX的所有操作都是通过该对象进行的

### 使用步骤

- 创建`XMLHttpRequest`对象

  ​	`var xhr = new XMLHttpRequest()`;

- 设置请求信息

  ​	`xhr.open(method,url)`;

  ​	注意:可以设置请求头,但一般不设置

​			` xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')`

- 发送请求

  `xhr.send(body)`   **get请求不传body参数,只有post请求使用**

- 接收响应

  `xhr.responseXML`接收xml格式的响应数据

  `xhr.responseText` 接收文本格式的响应数据

​			

**代码**

```html
//post.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AJAX POST 请求</title>
    <style>
        #result{
            width: 200px;
            height: 100px;
            border: solid 1px #903;
        }
    </style>
</head>
<body>
    <div id="result"></div>
    <script>
        //获取元素对象
        const result  = document.getElementById('result');
        //绑定事件
        result.addEventListener("mouseover", function () { 
            //创建对象
            const xhr  = new XMLHttpRequest();
            //初始化 设置类型与url
            xhr.open('POST', 'http://127.0.0.1:8001/server');
            //设置请求头
            xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
            //发送
            xhr.send('a=100&b=200&c=300');
            //事件绑定
            xhr.onreadystatechange = function(){
                //判断
                if(xhr.readyState === 4){
                    if(xhr.status>=200 && xhr.status<300){
                        //处理服务器返回的结果
                        result.innerHTML = xhr.response;
                    }
                }
            }
         })
    </script>
</body>
</html>
```

```js
//server.js
//1.引入express

const express = require('express')

//2.创建应用对象
const app = express();

//3.创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax -get')
})
app.post('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax -post')
})

app.all('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax')
})
app.all('/json-server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //响应头
    response.setHeader('Access-Control-Allow-Headers','*');
    //响应一个数据
    const data = {
        name:'hhhh'
    }
    //对对象进行字符串的转换
    const jsonStr = JSON.stringify(data);
    //设置响应体
    response.send(jsonStr)
})
//4.监听端口启动服务
app.listen(8001,()=>{
    console.log("服务器已启动，8001端口监听中……");
})

```

### AJAX请求状态

xhr.readyState 可以用来查看请求当前的状态

https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/readyState

0: 表示 XMLHttpRequest 实例已经生成，但是 open()方法还没有被调用。

1: 表示 send()方法还没有被调用，仍然可以使用 setRequestHeader()，设定 HTTP

2: 表示 send()方法已经执行，并且头信息和状态码已经收到。

3: 表示正在接收服务器传来的 body 部分的数据。

4: 表示服务器数据已经完全接收，或者本次接收已经失败了

# 第2章 跨域

### 同源策略

- 同源策略最早由Netscape 公司提出,是浏览器的一种安全策略
- 同源: 协议   域名   端口号  必须完全相同
- 违背同源策略就是跨域.

### 如何解决跨域

#### JSONP

- **JSONP是什么**

  - JSONP是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来的，只支持get请求

- **JSONP是怎么工作的？**

  - 在网页有一些标签天生具有跨域的能力，比如`img` `link``iframe` `script`.
  - JSONP就是利用`scrip`标签的跨域能力来发送请求的

- **JSONP的使用**

  - 动态的创建一个 script 标签

    `var script = document.createElement("script");`

  - 设置 script 的 src，设置回调函数

    ```js
    script.src = "http://localhost:3000/testAJAX?callback=abc";
    function abc(data) {
    	alert(data.name);
    }
    ```

  - 将 script 添加到 body 中

    `document.body.appendChild(script);`

  - 服务器中路由的处理

    ```js
    router.get("/testAJAX" , function (req , res) {
        console.log("收到请求");
        var callback = req.query.callback;
        var obj = {
            name:"孙悟空",
            age:18
        }
        res.send(callback+"("+JSON.stringify(obj)+")");
    });
    ```



#### CORS

- CORS是什么？
  - CORS，跨域资源共享。CORS是官方的跨域解决方案，它的特点是不需要再客户端做任何特殊的操作，完全在服务器中进行处理，支持get和post请求。跨域请求共享标准新增了一组HTTP首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源
- CORS怎么工作的
  - CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行
- CORS的使用
  - 主要是服务器端的设置
  - ```js
    router.get("/testAJAX" , function (req , res) {
    	//通过 res 来设置响应头，来允许跨域请求
        //res.set("Access-Control-Allow-Origin","*");
        res.set("Access-Control-Allow-Headers","*");
        res.send("testAJAX 返回的响应");
    });
    ```
  
    

## Axios发送AJAX请求

**axios.html**

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=, initial-scale=1.0">
    <title>axios发送</title>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.1.3/axios.js"></script>

</head>
<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>

    <script>
        const btns = document.querySelectorAll('button');

        //配置baseURL
        axios.defaults.baseURL = 'http://127.0.0.1:8001'
        btns[0].onclick = function () { 
            //GET请求
            axios.get('/axios-server',{
                //url参数
                params:{
                    id:100,
                    name:'yr'
                },
                //请求头信息
                headers:{
                    name:'jh'
                }
            }).then(value=>{
                console.log(value);
            })
         }

        btns[1].onclick = function () {
            axios.post('/axios-server',{
                    username:'admin',
                    password:123456 //请求体
                },{
                //url参数
                params:{
                    id:200,
                    vip:8
                },
                //请求头参数
                headers:{
                    text:'pppost'
                },
            })
        }

        btns[2].onclick = function () { 
            axios({
                method:'POST',
                url:'/axios-server',
                //url参数
                params:{
                    vip:10,
                    level:90,
                },
                //头信息
                headers:{
                    a:200,
                    b:300,
                },
                //请求体参数
                data:{
                    username:'admin',
                    password:1111,
                }
            }).then(value=>{
                console.log(value);
                //响应状态码
                console.log(value.status);
                //响应状态字符串
                console.log(value.statusText);
                //响应头信息
                console.log(value.headers);
                //响应体
                console.log(value.data);
            })
         }
    </script>
</html>
```



## Fetch发送AJAX请求

**fetch.html**

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>fetch发送AJAX请求</title>
</head>
<body>
    <button>AJAX请求</button>
    <script>
        const btn = document.querySelector('button');
        btn.onclick = function () {
            fetch('http://127.0.0.1:8001/fetch-server?vip=10',{
                method:'POST',
                //请求头
                headers:{
                    name:'yr',
                    age:18
                },
                //请求体
                body:'username=admin&password=111'
            }).then(res=>{
                return res.json();
            }).then(res=>{
                console.log(res);
            })
          }
    </script>
</body>
</html>
```



# Express框架

## Express介绍

Express是目前流行的基于Node.js运行环境的Web应用程序开发框架，它简洁且灵活，为Web应用程序提供了强大的功能。Express提供了一个轻量级模块，类似于jQuery（封装的工具库），它把Node.js的HTTP模块的功能封装在一个简单易用的接口中，用于扩展HTTP模块的功能，能够轻松地处理服务器的路由、响应、Cookie和HTTP请求的状态.

## 安装

```
npm init -y
```

```
npm install express --save
```

## 优势

- 简洁的路由定义方式。
- 简化HTTP请求参数的处理。
- 提供中间件机制控制HTTP请求。
- 拥有大量第三方中间件。
- 支持多种模版引擎。

## 利用Express搭建Web服务器

### **基本步骤**:

- 引入`express`对象
- 调用`express()`方法创建服务器对象

- 定义路由
- 调用`listen()`方法监听接口

**代码示例**

```js
//1.引入express
const express = require('express')

//2.创建应用对象
const app = express();

//3.创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax -get')
})
app.post('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax -post')
})

app.all('/server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //设置响应体
    response.send('hello ajax')
})
app.all('/json-server',(request,response)=>{
    //设置响应头 设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*')
    //响应头
    response.setHeader('Access-Control-Allow-Headers','*');
    //响应一个数据
    const data = {
        name:'hhhh'
    }
    //对对象进行字符串的转换
    const jsonStr = JSON.stringify(data);
    //设置响应体
    response.send(jsonStr)
})
//4.监听端口启动服务
app.listen(8001,()=>{
    console.log("服务器已启动，8001端口监听中……");
})

```

