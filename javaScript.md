# JavaScript

## 数据类型
在JavaScript中，数据可以分为两类：基本数据类型（基本类型或基元）和对象类型（复杂类型）。

基本数据类型包括undefined、null、boolean、number、string和symbol（ES6新增），它们都是原始值，直接存储在栈内存中。

对象类型包括包括数组、普通对象（使用字面量或构造函数创建的对象）、函数是复杂数据结构，它们通常存储在堆内存中，并通过栈中的引用来访问。

### 变量赋值
所有的变量赋值实际上都是将另一个变量中保存的值复制一份放入另一个变量。

基本数据类型之间进行赋值时，由于变量中直接存放的是值，所以相当于值传递
```javascript
var a = 1;
var b = a;
b = 10;
 
console.log(a);  //1
```
对象数据类型之间进行赋值时，由于变量中存放的是实际数据在堆中的地址，所以将地址复制一份存入另一个变量中，这样两个变量都指向堆中同一个数据结构，为地址引用。
```javascript
var arr1 = [1,2,3,4];
var arr2 = arr1;
arr2[0] = 10;
 
console.log(arr1[0]);  //10
```

# JavaScript
## JS事件流
JS事件有三个阶段，首先发生的是捕获阶段，然后是目标阶段，最后是冒泡阶段。DOM元素绑定的事件只能在捕获和冒泡其中一个阶段触发，默认DOM元素事件在冒泡阶段触发。

当button#d1被点击后，事件依次流经div#d3 => div#d2 => button#d1 => div#d2 => div#d3 DOM元素
```html
<div id="d3">
    <div id="d2">
        <button id="d1">点击</button>
    </div>
</div>
```

### 冒泡阶段
**默认DOM元素事件在冒泡阶段触发**

代码使用的是内联事件、DOM事件属性赋值还是`addEventListener()`方法绑定的事件都默认在冒泡阶段触发
```html
<div id="d3" onclick="console.dir(this)">
    <div id="d2" onclick="console.dir(this)">
        <button id="d1" onclick="console.dir(this)">点击</button>
    </div>
</div>
<!--
button#d1
div#d2
div#d3
-->
```

### 捕获阶段
点击按钮，事件在捕获阶段依次经过div#d3,div#d2,button#d1时触发事件

修改事件的触发阶段需要使用`addEventListener()`方法进行设定
```html
<body>
    <div id="d3">
        <div id="d2">
            <button id="d1">点击</button>
        </div>
    </div>
    <script>
        document.getElementById("d1").addEventListener("click",function(){
           console.log(typeof this)
            console.dir(this)
        },true)
        document.getElementById("d2").addEventListener("click",function(){
            console.log(typeof this)
            console.dir(this)
        })
        document.getElementById("d3").addEventListener("click",function(){
            console.log(typeof this)
            console.dir(this)
        },true)
    </script>
</body>
<!--
div#d3
div#d2
button#d1
-->
```

### 特殊事件
有些元素不具有事件冒泡的过程，而有的元素没有相应的事件。focus、blur、mouseenter、mouseleave等默认是不会冒泡的




## 数组和数组方法

在js中，数组是一个对象。数组中的内容被转换为键值对属性，属性名为内容的索引，属性值为内容。数组对象自带一个`length`属性，表示数组长度。数组的原型对象`__proto__`上有很多数组方法。

### forEach()

forEach方法遍历数组对象，接收一个回调函数 callback 作为参数，对数组对象中的每个元素调用回调函数。forEach没有返回值。

```javascript
let arr = [1, 2, 3, 4, 5];
//回调函数有三个参数currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback = (currentValue, index, array) => {
  console.log(currentValue);
};
console.log(arr.forEach(callback));
//输出1,2,3,4,5
```

### map()

map方法遍历数组并返回新数组， 接收一个回调函数作为参数，对数组对象中的每个元素调用回调函数，并将回调函数的返回值插入新数组，遍历结束后将新数组返回。

**map()函数生成的数组长度与原数组长度相同，如果某次回调函数执行没有返回结果会在新数组插入undefined**

```javascript
let arr = [1, 2, 3, 4, 5];
//回调函数有三个参数currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback = (currentValue, index, array) => {
  return currentValue;
};
console.log(arr.map(callback));
//输出[undefined, undefined, 3, 4, 5]
```

### filter()

filter方法遍历数组并返回新数组，接收一个回调函数作为参数，对数组对象中的每个元素调用回调函数，并将回调函数返回为true数组项插入新数组，遍历结束后将新数组返回。

```javascript
let arr = [1, 2, 3, 4, 5];
//回调函数有三个参数currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback = (currentValue, index, array) => {
    return currentValue >= 3};
let a = arr.filter(callback);
console.log(a);
//输出[3, 4, 5]
```

### find()

find方法遍历数组并返回第一个符合条件的元素，接收一个回调函数作为参数，对数组对象中的每个元素调用回调函数。

```javascript
let arr = [1, 2, 3, 4, 5];
//回调函数有三个参数currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback = (currentValue, index, array) => {
    return currentValue >= 3};
let a = arr.find(callback);
console.log(a);
//输出[3]
```

### concat()

concat方法用于合并两个或多个数组。不更改现有数组，返回一个新的结果数组。

```javascript
let arr = [1, 2, 3];
//concat中可以传入数值也可以传入数组，数值会直接插入新数组，数组会被打开并插入
arr = arr.concat(4, [5, 6]);
console.log(arr);
//[1, 2, 3, 4, 5, 6]
```

### push()

push() 方法将一个或多个（任何类型的）元素添加到数组的末尾，并返回数组中项目的更新总数。

**push与concat不同，push并不会展开数组**

```javascript
let arr = [1, 2, 3, 4, 5];
let a = arr.push(7);
console.log(a);
console.log(arr)
arr.push([8,9])
console.log(arr)
//6
//[1, 2, 3, 4, 5, 7]
//[1, 2, 3, 4, 5, 7, [8,9]]
```

### pop()

pop() 方法从数组中删除最后一项并返回该项。

```javascript
let arr = [1, 2, 3, 4, 5];
let a = arr.pop(5);
console.log(a);
console.log(arr)
//5
//[1, 2, 3, 4]
```

### reduce()

reduce方法遍历数组并返回一个计算结果，接收一个回调函数和初始值initialValue（可选）作为参数，对数组对象中的每个元素调用回调函数。

**reduce()方法强大且可以实现多种功能**

```javascript
//求和
let arr = [1, 2, 3, 4, 5];
//传入的回调参数有四个参数,accumulator(必选，reduce函数最后的返回值),currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback=(accumulator, currentValue,index, array) => {return accumulator + currentValue;}
//当存在初始值时，首次执行回调函数之前，将accumulator赋值为初始值，并从数组索引为0的元素开始执行回调。如果没传入初始值，首次执行回调函数之前，将accumulator赋值为数组索引为0的元素，并从数组索引为1的元素开始执行回调。
//执行过程：依次对数组元素调用回调，产生返回值，再把返回值作为下次回调的accumulator传入执行，最后返回accumulator。
let sum = arr.reduce(callback, 0);
console.log(sum);
//15

//比大小
let arr = [1, 2, 3, 4, 5];
//传入的回调参数有四个参数,accumulator(必选，reduce函数最后的返回值),currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback=(accumulator, currentValue,index, array) => {return Math.max(accumulator, currentValue);}
//当存在初始值时，首次执行回调函数之前，将accumulator赋值为初始值，并从数组索引为0的元素开始执行回调。如果没传入初始值，首次执行回调函数之前，将accumulator赋值为数组索引为0的元素，并从数组索引为1的元素开始执行回调。
//执行过程：依次对数组元素调用回调，产生返回值，再把返回值作为下次回调的accumulator传入执行，最后返回accumulator。
let sum = arr.reduce(callback, 0);
console.log(sum);
//5
```

## 遍历

数组遍历和对象遍历有不同的方式。for、forEach、for(let...of...)都只使用于数组类的可迭代对象遍历，而for(let...in...)可直接用于迭代所有对象，但遍历的是对象的所有**属性名**。可以借助 Object.keys、Object.values 和 Object.entries 等方法先将对象的属性提取为数组，再利用数组的遍历方法进行遍历。

### for()

for循环用于遍历基于索引的可迭代对象。

**适用于数组遍历，无法直接遍历对象**

```javascript
let arr = [1, 2, 3, 4, 5];
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}
//[1, 2, 3, 4, 5]
```

### forEach()

forEach用于遍历基于索引的可迭代对象。

**适用于数组遍历，无法直接遍历对象**

```javascript
let arr = [1, 2, 3, 4, 5];
//回调函数有三个参数currentValue(必选，当前数组元素), index(当前数组元素索引), array(当前数组)
let callback = (currentValue, index, array) => {
  console.log(currentValue);
};
console.log(arr.forEach(callback));
//输出1,2,3,4,5
```

### for(let...in...)

let ...in 用于遍历对象的可枚举属性(**这里的属性指的是键值对属性中的属性名，而不包括属性值**)，包括对象自身的属性和继承的可枚举属性。遍历过程中每次拿到对象的一个属性名(键)。

```javascript
let obj = { a: 1, b: 2, c: 3 };
for (let key in obj) {
    console.log(key);  
    console.log(obj[key]); 
}
// 输出: 'a', 'b', 'c'
// 输出: 1, 2, 3
```

**let...in...也可以遍历数组，但是每次拿到数组的一个索引值**

```javascript
let arr = [1, 2, 3];
for (let index in arr) {
    console.log(index);  
    console.log(arr[index]);  
}
// 输出: '0', '1', '2'
// 输出: 1, 2, 3
```

### for(let...of...)

let...of...用于遍历可迭代对象,如数组、字符串,每次拿到可迭代对象的一个值，不能直接遍历普通对象。

```javascript
let arr = [1, 2, 3];
for (let value of arr) {
    console.log(value);  
}
// 输出: 1, 2, 3
```

### Object.keys(obj)

Object.keys方法返回一个包含对象自身可枚举属性的数组。

```javascript
let obj = { a: 1, b: 2, c: 3 };
let keys = Object.keys(obj);

for (let i = 0; i < keys.length; i++) {
    console.log(keys[i]); 
    console.log(obj[keys[i]]); 
}
// 输出: 'a', 'b', 'c'
// 输出: 1, 2, 3
```

### Object.values(obj)

Object.values方法返回一个包含对象自身可枚举属性值的数组。

```javascript
let values = Object.values(obj);

for (let i = 0; i < values.length; i++) {
    console.log(values[i]); 
}
// 输出: 1, 2, 3
```

### Object.entries(obj)

Object.entries方法返回一个包含对象自身可枚举属性的键值对数组。

```javascript
let entries = Object.entries(obj);

for (let i = 0; i < entries.length; i++) {
    let [key, value] = entries[i];
    console.log(key); 
    console.log(value); 
}
// 输出: 'a', 'b', 'c'
// 输出: 1, 2, 3
```

* forEach()用于调用数组的每个元素，并将元素传递给回调函数。有两个参数，分别是回调函数和this，回调函数必须。回调函数中有三个参数分别是currentValue(当前元素，必写), index(当前元素的索引，可选), arr(当前元素所属数组，可选)。forEach 方法无法遍历对象，**仅适用于数组的遍历**，方法不会改变原数组，也没有返回值，回调函数中无法使用 break，continue 跳出循环。

```javascript
 array.forEach(function(currentValue, index, arr), thisValue)
```

* map()方法会返回一个新数组，数组中的元素为原始数组元素调用函数处理后return的值。有两个参数，分别是回调函数和this，回调函数必须。回调函数中有三个参数分别是currentValue(当前元素，必写), index(当前元素的索引，可选), arr(当前元素所属数组，可选)。map()无法遍历对象，仅适用于数组的遍历,不改变原始数组，生成新的数组，回调函数中可以return。

  ```javascript
  array.map(function(currentValue,index,arr), thisValue)
  ```

### 对象遍历

* for…in 主要用于循环对象**属性**，即遍历对象仅得到每个属性名，而不得到属性值。**for…in还可以用于遍历数组，但是得到的是数组索引**
  ```javascript
    var obj = {a: 1, b: 2, c: 3};

    for (var i in obj) {
    console.log('键名：', i);
    console.log('键值：', obj[i]);
    }
  ```

# Javascript
## 块级作用域和函数作用域

### var、let和const
var声明的变量具有函数作用域而不具有块级作用域，let和const声明的变量或者常量具有块级作用域

### 块级作用域
块级作用域是用{}包裹起来代码段，块级作用域中用var声明的变量会被变量提升至代码段所在的最近的函数作用域顶部，如果代码段定义在全局，变量就提升至全局。而let和const声明的变量或常量不会进行提升。

### 函数作用域
函数作用域是函数声明时形成的作用域，函数作用域中使用var定义的变量会自动提升至函数作用域顶部，而let和const声明的变量或者常量也不会提升。函数外部是无法访问函数内部的变量的。
```javascript
console.log(b)  //undefined
console.log(a)  //error

{
  let a=10;
  var b=100;
}
```

**var声明的变量不会提升至其所处函数作用域外，而let和const变量的声明必须在使用之前，否则报错。**
```javascript
console.log(a) //error
function func() {
  console.log(a)  //undefined
  console.log(b)  //error
  var a=100;
  let b=10;
}
func()
console.log(a)  //error
```





```javascript
//此时
let funArr = [];
for(var i = 0;i<5;i++){
    funArr.push(function(){
        console.log(i)
    })
}
 
funArr.forEach((item)=>{
    item()
})
```


---
# Javascript
## 闭包

内部作用域的函数访问外部作用域的变量（外部作用域不是全局），且将内部函数暴露给外部作用域，导致外部作用域可以通过此函数不断访问此变量。**注意：外部作用域可以是函数也可以是块级作用域（循环、if、{}等）**

在 JavaScript 中，当一个函数捕获了外部变量时，JavaScript 引擎会在内存中创建一个闭包（closure）。闭包包含两部分：函数本身和它捕获的外部变量（这些变量构成了闭包的环境）。闭包环境中的变量是对原来变量的值引用，且和原来的变量完全独立。

---
下述代码结合闭包和作用域的概念，看似每次`funArr.push`时就形成了一个闭包，但实际没有。**for中用var声明的变量会被提升至全局，称为全局变量。因为外部环境是全局，所以这里并未形成闭包**
```javascript
let funArr = [];
for(var i = 0;i<5;i++){
    funArr.push(function(){
        console.log(i)
    })
}
 
funArr.forEach((item)=>{
    item()
})
// 5 5 5 5 5
```

---
有多种改进的方法，但是最简单的是使用let声明变量，形成闭包。如果使用let声明变量，此时变量不会提升，funArr中压入的函数访问了其外部作用域的变量i，且外部作用域不是全局。同时在外部可以通过funArr[]的方式不断访问i，形成闭包。

**这里虽然压入的多个闭包函数都是对同一个i的访问，但是每次循环中，创建新的函数，对外部变量i进行引用时，会形成新的闭包环境。闭包环境中的变量值采用值引用，产生闭包环境时的值引用的i变量具有不同的值。**

```javascript
let funArr = [];
for(let i = 0;i<5;i++){
  funArr.push(function(){
    console.log(i)
  })
}

funArr.forEach(item =>{
  item()
})
//1 2 3 4 5
```


# Javascript
## 原型和原型链
原型分为显式原型和隐式原型，函数具有显式原型`prototype`,对象具有隐式原型`__proto__`。

原型链指的是对象通过隐式原型`__proto__`指向另一个对象，当访问一个对象的方法或者属性时，会沿着对象的原型链一直向上寻找。

### 函数与构造函数
1. Js中所有的函数都可以作为构造函数 **(箭头函数不可以作为构造函数，箭头函数没有this)**，构造函数只是函数的一种使用方式。当使用new关键字调用函数时，此函数就称为构造函数。
2. 所有的函数都具有一个显式原型prototype  **(箭头函数没有prototype)**，指向函数对应的原型对象。同时函数的显示原型对象中有一个constructor属性指向函数自己。

### 函数执行过程
函数执行过程即顺序执行函数内部代码语句。

### 构造函数执行过程
有一段代码
```javascript
function f() {
    console.log("Hello")
}
let obj=new f()
```
在`new f()`调用后，具体发生了什么
1. 首先创建一个新的空对象，并将这个对象的隐式原型`__proto__`设置为f.prototype **（f.prototype对象的隐式原型`__proto__`为Object.prototype）**。
2. **然后将函数f中的this绑定到这个新对象上，即函数中this指向新创建的对象。**
3. 执行函数f。
4. 若函数显式返回一个对象，则返回此对象。若函数未显式返回对象，则将新创建的对象返回。


### 构造函数和原型
在ES6之前，通过构造函数加原型的方式就实现了类的功能(实例化对象、继承、复用)
1. 通过调用构造函数并传入不同参数可以生成同类实例化对象。
2. 实例化对象的隐式原型`__proto__`自动指向构造函数的显式原型对象`prototype`,实现了继承。

### 类
在ES6中，出现了类的概念，但类是一种基于构造函数和原型的语法糖，并不是一种新语法，而是提供了一种更方便的书写方式，其内部实现仍为原型链。
1. 在开发者工具中输出一个类的`typeof`会显示为`function`，而并不是`class`。
2. 使用new实例化一个类的过程和使用new调用一个构造函数的过程相同。其中类中的方法会自动挂载到类的显示原型对象`prototype`上。
3. 类的显示原型对象的constructor属性指向类。

---
# JavaScript

## 单线程JS
Js是单线程语言是单线程语言，意味着JS处理所有任务都需要顺序的执行。由于顺序执行时可能遇到某些十分耗费事件的任务，而导致后续的任务长时间的等待导致页面阻塞。因此，js引入了事件循环。

### 同步任务、异步任务
引入事件循环后，所有的任务被分为两类：同步任务和异步任务。

* 同步任务:指在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
* 异步任务:指不进入主线程、而进入"任务队列"（task queue）的任务，只有"任务队列"通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行

### 事件循环
JavaScript事件循环机制包含执行栈、任务队列（Task）。其中任务队列又可以分为微任务队列（Microtask）和宏任务队列（Macrotask）。
* 执行栈：位于主线程，顺序执行JS代码，按循序执行其中所有的同步任务。
* 任务队列：主线程之外还存在一个任务队列，当执行栈执行过程中遇到异步任务，它并不会立即执行，而是将其放入对应的任务队列中。
* 宏任务包括setTimeout、setInterval、I/O操作等，微任务为Promise产生的异步任务。微任务的优先级高于宏任务。

### 事件循环过程
1. 执行栈顺序执行代码，先执行同步任务，将遇到的异步任务放入任务队列中，等待执行。
2. 当执行栈中所有同步任务执行完，JS引擎去读取任务队列中的任务。
3. 先执行微任务队列中的所有任务，将微任务队列中的第一个任务压入执行栈中执行，执行完毕后将其出栈。
4. 如此循环执行，直到微任务队列中的所有任务都执行完毕。
5. 检查宏任务队列，将宏任务队列中的第一个任务压入执行栈中执行，执行完毕后将其出栈。
6. 检查微任务队列，有任务则转到第4步，微任务队列无任务转到第5步。如此循环执行，直到微任务队列中的所有任务都执行完毕。

### 注意
* 浏览器核心是多线程的，之所以说JS是单线程，是因为浏览器在运行时只开启了一个JS引擎线程来解析执行JS。
* 浏览器中的多个线程负责不同的任务，例如定时触发器线程，在定时器结束后将其中的回调函数放入任务队列。例如事件触发线程，不断监听事件，当事件触发就将回调函数放入任务队列。
* 更多详解[https://www.jianshu.com/p/de7aba994523](https://www.jianshu.com/p/de7aba994523)

---
# JavaScript
## promise
promise是异步编程的一种解决方案。

### Promise构造函数
`Promise()`函数接收一个**立即同步执行**的执行器函数，执行器函数有两个参数resolve, reject。Promise函数只能通过new关键字调用，并返回一个promise对象。resolve, reject为两个函数，用于改变Promise()返回的promise对象的状态。

### promise对象
promise对象有三种状态，分别为Pending（等待中）、Fulfilled（已实现）、Rejected（已拒绝）。

### 不同执行器函数的效果

当执行器函数中无任何异步代码，且没有通过resolve, reject来执行任何回调函数。这时，JS引擎顺序执行执行器中的代码，并返回一个状态永远为Pending的promise对象。
```javascript
const prm1=new Promise((resolve, reject)=>{
    console.log("prm1执行")
})
console.log(prm1)
//prm1执行
```
---
当执行器函数中无任何异步代码，但通过resolve改变promise对象的状态为Fulfilled，此时附加到该Promise的then()方法中传入的回调函数都会被添加到微任务队列中。

同理如果调用了reject改变promise对象的状态为Rejected，附加到该Promise的catch()方法中传入的回调函数都会被添加到微任务队列中。
```javascript
const prm2=new Promise((resolve, reject)=>{
    console.log("prm2执行")
    resolve("prm2回调函数执行")
})
prm2.then((result)=>{console.log(result)})
//prm2执行
//prm2回调函数执行
```
---
当执行器函数中有异步任务请求。Promise的执行器函数中的代码顺序执行，遇到异步任务则将其放入任务队列，继续执行下方同步代码。
```javascript
//    <script src="https://cdn.bootcdn.net/ajax/libs/axios/1.5.0/axios.js"></script>注意需要引入axios库
const prm3=new Promise((resolve, reject)=>{
    console.log("prm3执行")
    axios({url:'http://hmajax.itheima.net/api/province'}).then((result)=>{
        console.log("Axios请求成功")
        resolve(result)
    }).catch((result)=>{
        reject(new Error(result))})
})

prm3.then((result)=>{
    console.log("prm3回调成功")
}).catch((result)=>{
    console.log("prm3回调结束")
})
//prm3执行
//Axios请求成功
//prm3回调成功
```
**执行过程：**
1. 首先执行log()和axios()请求。axios由Promise封装而成，请求成功后会返回一个promise对象。但这里不会等待axios()请求的结果，而是直接继续执行下方同步代码。 
2. axios()执行后，返回一个promise对象。当axios()内部的异步Ajax请求结束后，修改promise对象的状态。
3. 然后**JS引擎会根据此promise对象的状态，若为Fulfilled,则将所有then中的回调函数放到任务队列中，若为Rejected则将所有catch中的回调函数放到任务队列中**。 
4. axios()返回的promise对象的then和catch中的回调函数调用resolve和reject修改promise对象prm3的状态。从而使prm3对象的then()和catch()中的回调函数添加到任务队列。 
5. 实际上prm3就是多级回调嵌套，只是这里只有两层回调的嵌套，回调嵌套主要发生在需要按顺序执行多个异步操作时。当每个异步操作依赖于前一个异步操作的结果时。
当出现多级回调嵌套时，我们就需要
6. **Promise()是同步的，会立即执行，promise对象.then()和promise对象.catch()中的回调函数是异步的**，promise对象.then()/catch()代码段不会主动执行，JS引擎会在其promise对象变态时将其中的回调加入任务队列。


## async
async是一个用于修饰函数的关键字，用来声明一个异步函数，当然这个函数内部也可以没有任何异步代码。

用async关键字修饰的函数总会返回一个状态已经确定的promise对象：
* 如果函数没有返回值：promisestatus为fulfilled，promsieresult为undefined
* 如果有返回值：promisestatus为fulfilled，promsieresult为返回值
* 如果抛出错误：promisestatus为rejected，promsieresult为error对象
* 显式的return一个自定义的promise对象。**注意：自定义返回的promise对象中要调用resolve或reject，否则如果await了这个函数，其返回的promise对象的状态永远不会改变，所以其后方的代码永远暂停，这也验证了是先等await等待的promise对象的状态改变才将后方的代码添加进微任务队列**

### 没有效果的async函数
如果async 函数执行完，返回的promise 没有注册回调函数且async函数内部没有用await关键字。比如函数内部做了一次for 循环，你会发现函数的调用，就是执行了函数体，和普通函数没有区别，唯一的区别就是函数执行完会返回一个promise对象。
```javascript
async function asy1(){
    console.log("asy1")
}
console.log(asy1())
//asy1
//promise对象
```

## await
await关键字：await关键字只能放到async函数里面，当JS引擎遇到await关键字后，会进行：
1. 继续执行同一行后方的函数或者表达式，分为多种情况，如果后方是async函数，则会直接返回一个promise对象。如果是普通函数或者表达式，则会将其进行隐式转换为promise对象，函数的返回值和表达式计算得到的值会作为promise对象的promsieresult。 
2. 将后方函数或表达式执行结束后，暂停async函数中此await后方所有代码执行（同步代码和异步代码） 
3. 继续执行async函数外部的同步代码。 
4. 当await后方等待的promise对象通过resolve, reject改变状态后，await拿到promise对象中的promsieresult进行返回。 
5. 为await后方的所有代码创建一个微服务任务，并注册在微服务队列上。

```javascript
// 当遇到 await 时，会阻塞函数内部处于它后面的代码（而非整段代码），去执行该函数外部的同步代码；当外部的同步代码执行完毕，再回到该函数执行剩余的代码。并且当 await 执行完毕之后，会优先处理微任务队列的代码。
async function async1() {
    console.log('async1 start')
    await async2()
    console.log('async1 end')
    setTimeout(() => {
        console.log('timer1')
    }, 0)
}
async function async2() {
    setTimeout(() => {
        console.log('timer2')
    }, 0)
    console.log('async2')
}
async1()
setTimeout(() => {
    console.log('timer3')
}, 0)
console.log('start')
// async1 start => async2 => start => async1 end => timer2 => timer3 => timer1
/*  1.首先进入async1，打印出 async1 start。
    2.然后遇到async2，进入async2，遇到定时器 timer2，加入宏任务队列，之后打印 async2。
    3.由于 async2阻塞了后面的代码执行，将后面的代码加入微任务队列。所以执行后面的定时器 timer3，将其加入宏任务队列，之后打印同步代码 start。
    4.同步代码执行完毕，先执行微任务队列，打印出 async1 end，遇到定时器 timer1，将其加入宏任务队列。
    5.最后再执行宏任务队列，宏任务队列有3个任务，先后顺序是 timer2，timer3，timer1，分别按顺序执行，任务队列按照先进先出原则执行。
*/
```

## 事件
JavaScript事件是指在文档或者浏览器中发生的一些特定交互瞬间

### 原生JS中的事件
在原生JS中可以使用内联、DOM事件属性赋值和`addEventListener()`方法来为标签绑定事件

#### 内联式添加事件
一般适用与很简单的事件，标签事件属性后面直接赋值字符串，字符串中的语句会被直接封装进标签对应的DOM对象身上的`onclick`属性中。

当点击事件发生后，会直接以`DOM.onclick()`的形式调用，所以`onclick`中的`this`是标签对应的DOM对象
```html
<!--内联式事件-->
<button id="d1" onclick="console.dir(this)">点击</button>
```

#### DOM事件属性赋值
这种方法与内联式添加事件的原理相同，都是在标签对应的DOM对象身上添加了对应的事件属性

当事件发生时，会调用对应的DOM对象的事件处理函数，同时传入事件对象（event），事件对象不同于DOM对象，它包含的是当前发生事件的信息。
```html
<body>
<button id="d1" >点击</button>

<script>
  document.getElementById("d1").onclick=function (event){
    console.log(event)
    console.log(this)
  }
</script>
</body>
```

#### addEventListener()
内联式添加事件和DOM事件属性赋值时，都只能为DOM元素的某一事件添加一个事件处理函数，addEventListener()可以为同一事件属性添加多个事件处理函数。同时还可以通过传入参数控制事件的触发阶段（捕获或冒泡）
```html
<body>
    <button id="d1" >点击</button>
    <script>
        document.getElementById("d1").addEventListener("click",function(event){
            console.dir(this)
        })
        document.getElementById("d1").addEventListener("click",function(event){
          console.log(event)
        })
    </script>
</body>
```

### Vue中的事件
Vue中通过指令`v-on`来为模板中的标签添加事件，同时添加的事件会通过Vue实例调用，即`vm.`的形式调用。所以事件处理函数中的this不再指向目标DOM元素，而是Vue实例`vm`

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--最简单的vue项目只需要引入vue.js即可，全局会多一个Vue构造函数    -->
    <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
</head>
<body>
<div id="root">
    <button v-on:click="handelclick">点击</button>
</div>
<script>
    devtools: 1
    var vm=new Vue({
        el:"#root",
        data:{
            name:"zxr",
        },
        methods:{
            handelclick(){
                console.log(this)
            }
        }
    })
</script>
<!--点击后输出vm-->
</body>
```

### React中的事件
React同Vue类似，通过React组件的实例化对象调用事件处理函数，其中的`this`指向组件实例化对象。




