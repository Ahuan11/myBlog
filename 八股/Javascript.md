## 变量提升

**定义：**变量提升即将变量声明提升到它所在[作用域](https://so.csdn.net/so/search?q=作用域&spm=1001.2101.3001.7020)的最开始的部分。

- **通过var定义（声明）的变量，在定义语句之前就可以访问到；**
- **值：undefined；**

```javascript
console.log(a); //undefined
var a = 1;
12
```

 	因为有变量提升的缘故，上面代码实际的执行顺序为：

```js
var a;
console.log(a);
a = 1;
```

> `let`和 `const`没有变量提升，控制台会报错