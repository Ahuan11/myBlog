# JS基础知识学习

## 变量

## number,string,boolean,null,undefined基本数据类型

## 	object引用数据类型

### 数字型Number

```javascript
//1.八进制 0~7  在前面加  0 表示八进制
var num1 = 010
console.log(num1)------8
//2.十六进制 0~9 a~f  数字前面加  0x  表示十六进制
var num2 = 0x9
console.log(num2)------9
var num3 = 0xa
console.log(num3)-------10

//非数字
console.log('pink老师' - 100);-----NaN

Infinity //表示正无穷  属于number类型
-Infinity //表示负无穷  属于number类型

NaN 是一个特殊的数字，表示not a number 
typeof NaN -----number
```

#### 函数

**isNaN() ：**判断一个变量是否为非数字的类型， 是数字返回false，不是数字返回true

### 字符串型String

#### 字符串转义符

| 转义符 | 解释说明                 |
| ------ | ------------------------ |
| \n     | 换行符，n是newline的意思 |
| \\\    | 斜杠\                    |
| \*     | ‘单引号                  |
| \t     | tab缩进                  |
| \b     | 空格                     |
| \\"    | 表示引号                 |

#### 字符串的基本操作

| 操作                      | 解释说明                     |
| ------------------------- | ---------------------------- |
| str.length                | 获取字符串长度               |
| str1+str2                 | 字符串拼接                   |
| ‘pink’+18--->‘pink18’     | 字符串与数字拼接得到字符串   |
| ‘pink’+true--->‘pinktrue’ | 字符串与布尔值拼接得到字符串 |

### 布尔型Boolean

```javascript
console.log(true+1) //2      //true在加法运算中当1来看
console.log(false+1) //1	  //false在加法运算中当0来看
```

### undefined

```javascript
var variable = undefined
console.log(variable + 'pink')  //undefinedpink
console.log(variable + 1)       //NaN

typeof undefined  返回undefined
```

### null

```
var space = null
console.log(space + 'pink')  //nulldpink
console.log(space + 1)       //1

typeof null 返回object
```

### 数据类型转换

#### 转换为字符串

| 方式             | 说明                                             | 案例                             |
| ---------------- | ------------------------------------------------ | -------------------------------- |
| toString()       | 转成字符串(不会改变原变量)，null,undefined不能用 | var num=1;alter(num.toString()); |
| String()强制转换 | 转成字符串                                       | var num=1;alter(String(num));    |
| 加号拼接字符串   | 和字符串拼接的结果都是字符串（隐式转换）         | var num=1;alter(num+‘’);         |

#### 转换为数字型（重点）

| 方式                   | 说明                         | 案例                |
| ---------------------- | ---------------------------- | ------------------- |
| parseInt(string)函数   | 将string类型转成整数数值型   | parseInt(‘78’)      |
| parseFloat(string)函数 | 将string类型转成浮点数数值型 | parseFloat(‘18.89’) |
| Number()强制转换函数   | 将string类型转成数值型       | Number(‘16’)        |
| js隐式转换（- * /）    | 利用算术运算隐式转换为数值型 | ‘12’-0              |

```javascript
console.log(parseInt('3.14'))  //3
console.log(parseInt('3.98'))  //3
console.log(parseInt('120px')) //120
console.log(parseInt('rem120px')) //NaN
```

#### 转换为布尔型

| 方式          | 说明                 | 案例             |
| ------------- | -------------------- | ---------------- |
| Boolean()函数 | 其他类型转换成布尔值 | Boolean(‘true’); |

代表空、否定的值会被转换成false，如‘’  0  NaN  null  undefined

其余值都会转换成true

## 深拷贝与浅拷贝

### 一、先了解内存分区

内存是用来存储数据的，不同的数据存储在不同的内存中，即分类存储。

<img src="https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/b402858fef104d6381f49683deeada58.png" alt="img" style="zoom:150%;" />

###  二、再来看基本类型和引用类型

数据分为基本数据类型(String, Number, Boolean, Null, Undefined，Symbol)和对象数据类型。

（1）基本类型：就是值类型，字符串，即在变量所对应的内存区域存储的是值。

           特点：直接存储在栈（stack）中的数据，拷贝后形成新的数据，修改拷贝后的数据也不会影响原始数据。

（2）引用类型：就是地址类型（相当于编号---便于寻找），对象/数组。  

           特点：存储的真实数据在堆中，但是在栈中被引用，拷贝后不生成新的数据，是引用，修改拷贝后的数据会影响原始数据。

### 三、浅拷贝和深拷贝

深拷贝和浅拷贝都是主要针对Object和array对象。

<img src="https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/efcf155f37144ea7a4b009b2af30d330.png" alt="img" style="zoom:150%;" />

### （1）浅拷贝实现

#### 1.Object.assign()

Object.assign() 方法可以把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。但是 Object.assign()进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。

```javascript
var obj = { a: {a: "kobe", b: 39} };
var initalObj = Object.assign({}, obj);
initalObj.a.a = "wade";
console.log(obj.a.a); //wade
```

>  注意：当object只有一层的时候，是深拷贝
>

```javascript
let obj = {
    username: 'kobe'
    };
let obj2 = Object.assign({},obj);
obj2.username = 'wade';
console.log(obj);//{username: "ko be"}
```

#### 2.Array.prototype.concat() 

```javascript
let arr = [1, 3, {
    username: 'kobe'
    }];
let arr2=arr.concat();    
arr2[2].username = 'wade';
console.log(arr);
```

修改新对象会改到原对象:

![img](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/9eb5c008dc31298ffc45e3c7574463d5.jpeg)


------------------------------------------------
#### 3.Array.prototype.slice() 

```js
let arr = [1, 3, {
    username: ' kobe'
    }];
let arr3 = arr.slice();
arr3[2].username = 'wade'
console.log(arr);
```

 同样修改新对象会改到原对象:

![img](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/27f61c0ecc0ba943894048d36107ef8a.jpeg)

> **关于Array的slice和concat方法的补充说明**：Array的slice和concat方法不修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组

### （2）深拷贝的实现

#### 1.JSON.parse(JSON.stringify())

**但是这种方法不能处理函数**

```js
let arr = [1, 3, {
    username: ' kobe'
}];
let arr4 = JSON.parse(JSON.stringify(arr));
arr4[2].username = 'duncan'; 
console.log(arr, arr4)
```

![img](https://aruiblogimages.oss-cn-hangzhou.aliyuncs.com/img/171df5250be04ed9c5463d6c443ce947.jpeg)

#### 2.手写递归方法

递归方法实现深度克隆原理：遍历对象、数组直到里边都是基本数据类型，然后再去复制，就是深度拷贝。

```js
/定义检测数据类型的功能函数
    function checkedType(target) {
      return Object.prototype.toString.call(target).slice(8, -1)
    }
    //实现深度克隆---对象/数组
    function clone(target) {
      //判断拷贝的数据类型
      //初始化变量result 成为最终克隆的数据
      let result, targetType = checkedType(target)
      if (targetType === 'Object') {
        result = {}
      } else if (targetType === 'Array') {
        result = []
      } else {
        return target
      }
      //遍历目标数据
      for (let i in target) {
        //获取遍历数据结构的每一项值。
        let value = target[i]
        //判断目标结构里的每一值是否存在对象/数组
        if (checkedType(value) === 'Object' || checkedType(value) === 'Array') 
        { //对象/数组里嵌套了对象/数组
          //继续遍历获取到value值
          result[i] = clone(value)
        } else { //获取到value值是基本的数据类型或者是函数。
          result[i] = value;
        }
      }
      return result
    }
```

####  3.函数库lodash

```js
var _ = require('lodash');
var obj1 = {
    a: 1,
    b: { f: { g: 1 } },
    c: [1, 2, 3]
};
var obj2 = _.cloneDeep(obj1);
console.log(obj1.b.f === obj2.b.f);
//false
```

