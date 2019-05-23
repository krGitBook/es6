# 解构赋值

## 数组的解构赋值

**?????Iterator**
```
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};   //可以转换成可以遍历的遍历器对象吗


等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），
要么本身就不具备 Iterator 接口（最后一个表达式）。

```


**默认值**

解构赋值允许指定默认值

```
let [x, y = 'b'] = ['a']; 
let [x, y = 'b'] = ['a', undefined]; 



let [x = 1] = [undefined];
x

let [x = 1] = [null];
x



ES6 内部使用严格相等运算符（===），判断一个位置是否有值。
所以，只有当一个数组成员严格等于undefined，默认值才会生效。
```


```
function f() {
  console.log('aaa');
}

let [x = f()] = [1]; 等价于：






let x;
if ([1][0] === undefined) {
  x = f();
} else {
  x = [1][0];
}




如果默认值是一个表达式，那么这个表达式是惰性求值的，
即只有在用到的时候，才会求值
```

## 对象的解构赋值


对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；
而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。


对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。
真正被赋值的是后者，而不是前者

```
let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p: [x, { y }] } = obj;让p也作为变量：




let obj = {
  p: [
    'Hello',
    { y: 'World' }
  ]
};

let { p, p: [x, { y }] } = obj;
```



对象的解构赋值可以取到继承的属性:

?????
```
const obj1 = {};
const obj2 = { foo: 'bar' };
Object.setPrototypeOf(obj1, obj2);

const { foo } = obj1;
foo


Object.setPrototypeOf()方法:设置一个指定的对象的原型 (即,内部[[Prototype]]属性）
到另一个对象或null。
```

```
var {x: y = 3} = {x: 5};
y
```

## 注意点

（1）如果要将一个已经声明的变量用于解构赋值，必须非常小心

```
let x;
{x} = {x: 1};




JavaScript 引擎会将{x}理解成一个代码块，从而发生语法错误。
只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题。
```

（2）解构赋值允许等号左边的模式之中，不放置任何变量名。
因此，可以写出非常古怪的赋值表达式

```
({} = [true, false]);
({} = 'abc');
({} = []);


上面的表达式虽然毫无意义，但是语法是合法的，可以执行。
```

（3）由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构。
```
let arr = [1, 2, 3];
let {0 : first, [arr.length - 1] : last} = arr;
first
last 
```


## 字符串的解构赋值

```
const [a, b, c, d, e] = 'hello';
a 
b
c 
d
e 



let {length : len} = 'hello';
len
```


## 数值和布尔值的解构赋值

```
let { prop: x } = undefined; 
let { prop: y } = null; 



解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象。
由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错
```

## 函数参数的解构赋值

```
[[1, 2], [3, 4]].map(([a, b]) => a + b);



?????

function move({x = 0, y = 0} = {}) {
  return [x, y];
}
move({x: 3, y: 8}); 
move({x: 3}); 
move({});
move(); 



function move({x = 0, y = 0}) {
  return [x, y];
}
move({x: 3, y: 8}); 
move({x: 3}); 
move({});
move(); 



function move({x, y} = { x: 0, y: 0 }) {
  return [x, y];
}
move({x: 3, y: 8});
move({x: 3}); 
move({}); 
move(); 
```


undefined就会触发函数参数的默认值。


## 用途

（1）交换变量的值

```
let x = 1;
let y = 2;

[x, y] = [y, x];
```

（2）从函数返回多个值

```
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();


// 返回一个对象
function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```

（3）函数参数的定义

```
// 参数是一组有次序的值
function f([x, y, z]) { ... }
f([1, 2, 3]);

// 参数是一组无次序的值
function f({x, y, z}) { ... }
f({z: 3, y: 2, x: 1});
```

（4）提取 JSON 数据

```
let jsonData = {
  id: 42,
  status: "OK",
  data: [867, 5309]
};

let { id, status, data: number } = jsonData;

console.log(id, status, number);
```

（5）函数参数的默认值

```
jQuery.ajax = function (url, {
  async = true,
  beforeSend = function () {},
  cache = true,
  complete = function () {},
  crossDomain = false,
  global = true,
  // ... more config
} = {}) {
  // ... do stuff
};
```

（6）遍历 Map 结构

**?????**
```
任何部署了 Iterator 接口的对象，都可以用for...of循环遍历。Map 结构
原生支持 Iterator 接口，配合变量的解构赋值，获取键名和键值就非常方便

const map = new Map();
map.set('first', 'hello');
map.set('second', 'world');
for (let [key, value] of map) {
  console.log(key + " is " + value);
}

for (let [key] of map) {
  // ...
}

for (let [,value] of map) {
  // ...
}
```


（7）输入模块的指定方法

```
const { SourceMapConsumer, SourceNode } = require("source-map");
```