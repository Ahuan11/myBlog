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