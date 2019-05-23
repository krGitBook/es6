```
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6]();

```

上面代码中，变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量，所以最后输出的是6。你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 **JavaScript 引擎内部会记住上一轮循环的值**，初始化本轮的变量i时，就在上一轮循环的基础上进行计算


```
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}

for (var i = 0; i < 3; i++) {
  var i = 'abc';
  console.log(i);
}

for (let i = 0; i < 3; i++) {
  var i = 'abc';
  console.log(i);
}

for (var i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}


1.函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域。(三个'abc')
2.全局一个变量，重新赋值'abc'比3大就不循环了（一个'abc'）
3.报错，let 禁止在同一个作用域下重复声明
4.三个'abc'
```

## **let const:**

- **不存在变量提升** 
- "**暂时性死区**":“暂时性死区”也意味着typeof不再是一个百分之百安全的操作
- **不允许重复声明**:let不允许在相同作用域内，重复声明同一个变量。


```
function bar(x = y, y = 2) {
  return [x, y];
}

bar();


function bar(x = 2, y = x) {
  return [x, y];
}
bar()


function func(arg) {
  let arg;
}
func() 


function func(arg) {
  {
    let arg;
  }
}
func()
```

### 块级作用域

**ES5 只有全局作用域和函数作用域，没有块级作用域**

**let实际上为 JavaScript 新增了块级作用域**




## const

```
const foo;
```
```
const foo = {};

foo.prop = 123;
foo.prop 

foo = {};
```

将一个对象及其子元素完全冻结：
```
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

**?????:**

[暂时性死区](https://www.jianshu.com/p/29adcca45c35)

**?????:**
```

if (true) let x = 1;

 

if (true) {
  let x = 1;
}


上面代码中，第一种写法没有大括号，所以不存在块级作用域，而let只能出现在当前作用域的顶层，所以报错。
第二种写法有大括号，所以块级作用域成立。


'use strict';
if (true) {
  function f() {}
}


'use strict';
if (true)
  function f() {}


函数声明也是如此，严格模式下，函数只能声明在当前作用域的顶层。
```

**顶层对象的属性**

ES6 一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。

```
var a = 1;
window.a 

let b = 1;
window.b 
```

