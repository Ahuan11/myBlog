

## Promise是什么 ？

- **抽象表达：**

  - Promise是一门新的技术（ES6规范）

  - Promise是JS中进行异步编程的**新解决方案**

    > 旧方案是单纯的使用回调函数

- **具体表达：**
  - 从语法上说：Promise是一个构造函数
  - 从功能上说：Promise对象用来封装一个异步操作并可以获取其成功/失败的结果值

## 为什么要用Promise？

1. **指定回调函数的方式更加灵活**

   - 旧的：必须在启动异步任务前指定

   - promise ：启动异步任务 =>返回promise对象 =>给promise对象绑定回调函数（甚至可以在异步任务结束后指定/多个）

1. **支持链式调用，可以解决回调地狱问题**
   - 什么是回调地狱？
     - 回调函数嵌套调用，外部回调函数异步执行的结果是嵌套的回调执行的条件
   - 回调地狱的特点？
     - 不便于阅读
     - 不便于异常处理
   - 解决方案？
     - promise链式使用

## 读取文件（Promise）

```javascript
const fs = require('fs');

const path = require('path');

//回调函数的形式
fs.readFile(path.join(__dirname,'/1.txt'),function(err,data){
    if(err)  throw err;
    console.log(data.toString());
})

//Promise的形式
let p = new Promise((resolve,reject)=>{
    fs.readFile(path.join(__dirname,'/1.txt'),function (err,data) { 
        //如果出粗
        if(err)  reject(err);
        //如果成功
        resolve(data);
     })
})
//调用then
p.then(value=>{
    console.log(value.toString());
},reason=>{
    console.log(reason);
})
```

## Promise封装AJAX请求

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <div class="container">
        <h2>Promise 封装  AJAX  操作</h2>
        <button id="btn">点击发送 AJAX</button>
    </div>
    <script>
        //接口地址  https://api.apiopen.top/getJoke
        //获取元素对象
        const btn = document.querySelector('#btn');
        btn.addEventListener('click',function () { 
            //创建Promise
            const p = new Promise((resolve,reject)=>{
                 //创建对象
                const xhr = new XMLHttpRequest();
                //初始化
                xhr.open('GET','https://api.apiopen.top/getJoke');
                //发送
                xhr.send();
                //绑定事件
                xhr.onreadystatechange = function () { 
                    if(xhr.readySate === 4)
                    {
                        //判断响应状态码  2xx
                        if(xhr.status >= 200 && xhr.status <300)
                        {
                            //控制台输出响应体
                            resolve(xhr.response)
                        }else{
                            //控制台输出响应码
                            reject(xhr.status)
                        }
                    }
                }
            })
            //调用then方法
            p.then(value=>{
                console.log(value);
            },reason=>{
                console.warn(reason);
            })
         })
    </script>
</body>
</html>
```

## util.promisify()方法

```javascript
/*
    util.promisify  方法
 */
//引入util模块
const util =  require('util');
//引入fs模块
const fs = require('fs');
//引入path模块
const path = require('path')

//返回一个新的函数
let mineReadFile = util.promisify(fs.readFile);
mineReadFile(path.join(__dirname,'./1.txt')).then(value=>{
    console.log(value.toString());
})
```

## promise的状态改变

- **Promise的状态**

  -------实例对象中的一个属性 【PromiseState】

  - pending  未决定的
  - resolved    /  fullfilled  成功
  - rejected     失败

  > pending =>  resolved
  >
  > pending =>  rejected
  >
  > 只有这2种，且一个promise对象只能改变一次
  >
  > 无论变为成功还是失败，都会有一个结果数据
  >
  > 成功的结果数据一般称为value，失败的姐夫哦数据一般称为reason

- **Promise对象的值**

  -------实例对象中的另一个属性 【PromiseResult】，保存着异步任务 【成功/失败】的结果

  - resolve
  - rejecte

## Promise  API

### Promise构造函数：Promise(executor){}

- executor函数：执行器 （resolve,reject)=>{}

- resolve 函数 ：内部定义成功时我们调用的函数 value => {}

- reject函数 ：内部定义失败时我们调用的函数  reason => {}

  > executor会在Promise内部立即**同步调用**，异步操作在执行器中执行

### Promise.prototype.then 方法：(onResolved,onRejectd)=>{}

- onResolved函数 ：成功的回调函数 (value)=>{}

- onRejectd函数  ：失败的回调函数  (reason)=>{}

- > 指定用于得到成功value的成功回调和用于得到失败reason的失败回调返回一个新的promise对象

### Promise.prototype.catch 方法： (onRejectd)=>{}

- onRejected函数：失败的回调函数(reason)=>{}

### Promise.resolve方法 ：(value)=>{}

- value :  成功的数据或Promise对象
- 说明：返回一个成功/失败的promise对象

```
let p1 = Promise.resolve(521);
//如果传入的参数为 非Promise类型的对象，则返回的结果为成功promise对象
//如果传入参数为Promise对象，则参数的结果决定了resolve的结果
```

### Promise.reject方法 ：(reason)=>{}

- reason ：失败的原因
- 说明：返回一个失败的promise对象

### Promise.all方法：(promises)=>{}

- promises ：包含n个promise的数组
- 说明：返回一个新的promise，只有所有的promise都成功才成功，只要有一个失败了就直接失败

### Promise.race方法：(promises)=>{}

- promises：包含n个promise的数组
- 说明：返回一个新的promise，第一个完成的promise的结果状态就是最终的结果状态

## Promise的几个关键问题

- **如何改变promise的状态？**
  - resolve(value)：如果当前是pending就会变为resolved
  - reject(reason)：如果当前是pending就会变为rejected
  - 抛出异常：如果当前是pending就会变为rejected
  
- **一个promise指定多个成功/失败回调函数，都会调用吗？**
  - 当promise改变为对应状态时都会调用
  
- **改变promise状态和指定回调函数谁先谁后**
  - 都有可能，正常情况下是先指定回调再改变状态，但也可以先改状态再指定回调
  - 如何先改状态再指定回调
    - 在执行器中直接调用resolved()/reject()
    - 延迟更长时间才能调用then()
  - 什么时候才能得到数据？
    - 如果先指定的回调，那么当状态发生改变时，回调函数就会调用，得到数据
    - 如果先改变的状态，那当指定回调时，回调函数就会调用，得到数据
  
- promise.then()返回的新promise的结果状态由什么决定？
  - 简单表达：由then()指定的回到函数执行的结果决定
  - 详细表达：
    - 如果抛出异常，新promise变为rejected，reason为抛出的异常
    - 如果返回的是非promise的任意值，新promise变为resolved，value为返回的值
    - 如果返回的是另一个新promise，此promise的结果就会称为新promise的结果

- promise异常穿透

  - 当使用promise的then链式调用时，可以在最后指定失败的回调，
  - 前面任何操作出了异常，都会传到最后失败的回调中处理

- 中断promise链？

  - 当使用promise的then链式调用时，在中间中断，不在调用后面的回调函数

  - 办法：在回调函数中返回一个pendding状态的promise对象

    ```javascript
    p.then(value=>{
    	console.log(111)
    	//有且只有一个方式
    	return new Promise(()=>{});
    }).then(value=>{
    	console.log(222)
    }).then(value=>{
    	console.log(333)
    })
    ```

    
    
    
    
    
    
    
    
    
    
    
    
    

## Promise的自定义封装

**index.html**

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Promise的自定义封装</title>
    <script src="./promise.js"></script>
</head>
<body>
    <script>
        let p = new Promise((resolve,reject)=>{
            reject('error');
        });
        console.log(p);
        p.then(value=>{
            console.log(value);
        },reason=>{
            console.warn(reason);
        })
    </script>
</body>
</html>
```

**promise.js**

```js
class Promise{
    //构造方法
    constructor(executor){
        //添加属性
        this.PromiseState = 'pending';
        this.PromiseResult = null;
        //声明属性
        this.callbacks = [];
        //保存示例对象的this的值
        const self = this;
        // resolve函数
        function resolve(data){
            //函数中的this指向的时window
            //判断状态
            if(self.PromiseState !== 'pending') return 
            // 1.修改对象的状态  (promiseState)
            self.PromiseState = 'fulfilled'
            //2.设置对象结果值  (promiseResult)
            self.PromiseResult = data;
            // 调用成功的函数
            setTimeout(()=>{
                self.callbacks.forEach(item=>{
                    item.onResolved(data);
                })
            })
            
        }
        //reject函数
        function reject(data){ 
            //判断状态
            if(self.PromiseState !== 'pending') return 
            // 1.修改对象的状态  (promiseState)
            self.PromiseState = 'rejected'
            //2.设置对象结果值  (promiseResult)
            self.PromiseResult = data;
            setTimeout(() => {
                self.callbacks.forEach(item=>{
                    item.onRejected(data);
                })   
            });
        }
        try{
        //同步调用执行器函数
            executor(resolve,reject);
        }catch(e){
            //修改 promis 对象状态为 【失败】
            reject(e);
        }
    }
    // then 方法封装
    then(onResolved,onRejected){
        //调用回调函数
        const self = this;
        //判断回调函数参数
        if(typeof onRejected !== 'function')
        {
            onRejected = reason=>{
                throw reason;
            }
        }
        if(typeof onResolved !== 'function')
        {
            onResolved = value=>value;
            //value=>{ return value }
        }
        return new Promise((resolve,reject)=>{
            //封装函数
            function callback(type){
                try{
                    let result = type(self.PromiseResult);
                    if(result instanceof Promise)
                    {
                        result.then(v=>{
                            resolve(v)
                        },r=>{
                            reject(r)
                        })
                    }else{
                        resolve(result)
                    }
                }catch(e){
                    reject(e);
                }
            }
            if(this.PromiseState === 'fulfilled'){
                setTimeout(()=>{
                    callback(onResolved)
                })
            
            }
            if(this.PromiseState === 'rejected'){
                setTimeout(()=>{
                    callback(onRejected)
                })
            }
            //判断 pending 状态
            if(this.PromiseState === 'pending'){
                //保存回调函数
                this.callbacks.push({
                    onResolved : function () { 
                    callback(onResolved)
                    },
                    onRejected : function () { 
                        callback(onRejected)
                    },
                })
            }
        })
    }
    //catch 方法
    catch(onRejected){
        return this.then(undefined,onRejected); 
    }
    // 添加resolve 方法
    static resolve(value){
        return new Promise((resolve,reject)=>{
            if(value instanceof Promise){
                value.then(v=>{
                    resolve(v)
                },r=>{
                    reject(r)
                }) 
            }else{
                //状态设置为成功
                resolve(value)
            }
        })
    }
    // 添加 reject 方法
    static reject(reason) { 
        return new Promise((resolve,reject)=>{
            reject(reason);
        }) 
      }
    //添加 all 方法
    static all(promises) { 
        //返回结果为promise对象
        return new Promise((resolve,reject)=>{
            let count = 0;
            let arr= [];
            //遍历
            for(let i=0;i<promises.length;i++){
                promises[i].then(v=>{
                    //得知对象的状态时成功的
                    count++;
                    //将当前promise对象成功的结果，存入数组中
                    arr[i] = v;
                    if(count === promises.length){
                        resolve(arr);
                    }
                },r=>{
                    reject(r);
                })
            }
        })
     }
      //添加 race 方法
    static race(promises){
        return new Promise((resolve, reject) => {
            for(let i=0;i<promises.length;i++){
                promises[i].then(v=>{
                    //修改返回对象的状态为 [成功]
                    resolve(v);
                },r=>{
                    //修改返回对象的状态为 [失败]
                    reject(r);
                })

            }
        })
    }
}
```



## async 与 await

### async函数

- 函数的返回值为promise对象
- promise对象的结果由async函数执行的返回值决定

### await函数

- await右侧的表达式一般为promise对象，但也可以是其他的值
- 如果表达式是promise对象，await返回的是promise成功的值
- 如果表达式是其他值，直接将此值作为await的返回值

### 注意：

- await 必须写在async 函数中，但async函数中可以没有await
- 如果 await 的promis失败了，就会抛出异常，**需要通过try...catch捕获处理**

