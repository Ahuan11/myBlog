# DOM

#### 文档对象模型（Document Object  Model，简称DOM）

#### 是W3C组织土建的处理可扩展标记语言（HTML或者XML）的标准编程接口

#### W3C已经定义了一系列的DOM接口，通过这些DOM接口可以改变网页的内容，结构和样式

## DOM树

![1655875759072](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/1655875759072.png)

## getElementById根据ID获取元素

```javascript
var timer=document.getElementById('time')
console.log(typeof timer);
//打印我们返回的元素对象，更好的查看里面的属性和方法
console.dir(timer)
```

## getElementsByTageName根据标签名获取元素

```javascript
//返回的是获取过来元素对象的集合，以伪数组的形式存储的
var lis = document.getElementsByTageName('li');
console.log(lis);
```

## getElementByClassName根据类名获取元素

## querySelector 返回指定选择器的第一个元素对象

```javascript
var firstBox = document.querySelector('.box')
var nav = document.querySelector('#nav')
var li = document.querySelector('li')
```

## querySelectorAll 返回指定选择器的所有元素对象

## document.body获取body元素

## document.documentElement获取html元素

## 事件基础

### 常见的鼠标事件

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

## innerText（非标准）和innerHTML（标准）的区别

### innerText不识别html标签，去除空格换行，innerHTML识别，保留空格和换行

