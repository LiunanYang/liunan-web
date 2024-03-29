## 变量提升 & 暂时性死区
- 声明提升：变量的声明提升在代码顶部，可以在声明之前使用变量，值为 undefined，var 声明的变量存在声明提升
- 暂时性死区：let 和 const 不存在声明提升，凡在声明之前使用，就会报错

```
console.log(foo); // 输出undefined
var foo = 2;

console.log(bar); // 报错ReferenceError
let bar = 2;
```

比较隐蔽的死区：
```
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```
原因：参数x默认等于参数y，而此时y还没有声明，属于死区

## 块级作用域
let、const声明的变量（常量）只在声明所在的代码块内有效
```js
{
  let a=1;
  var b=2;
}
a //ReferenceError: a is not defined.
b // 2
```

ES5只有全局作用域和函数作用域，缺点：
- 内层变量可能会覆盖外层变量
- 循环变量泄露为全局变量

## 常量
const 声明一个只读的常量，声明时必须赋值，一旦声明，常量的值就不能改变，改变则报错

```js
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```


## ES6声明变量/常量的6种方法
- var
- function
- let 
- const
- import
- class

## 顶层对象的属性
- 在浏览器环境指的是window对象，在Node指的是global对象


```
  let x = [12,23]
  function fn(y){
    y[0]=100
    y=[100]
    y[1]=200
    console.log(y)    
  }
  fn(x)
  console.log(x) 
```