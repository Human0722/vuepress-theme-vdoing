---
title: ES6
date: 2022-05-06 17:11:54
permalink: /pages/858668/
sidebar: auto
categories:
  - Javascript
tags:
  - 
author: 
  name: Xueliang
  link: https://github.com/Human0722
---

> ECMAScript6 (2022春节)

#### let 变量声明
1、 变量不能重复声明。   
2、 块级作用域。  
3、 不存在变量提升。 代码在运行前会扫描所有变量，在此期间会给 var 声明的变量赋值： undefined, 这个过程被称之为变量提升。而用 let 声明的变量不会被变量提升，在定义之前访问这个变量会报错。

```javascript
console.log(content)    // undefined
var content = 'hello'   
console.log(title)      // error
let title = 'hi'
```  
4、 不影响作用域链。下面代码中，在 fn 函数内部，没有变量 ```school```, 则会向外层寻找这个变量。
```javascript
    let school = 'hello';
    function fn() {
        console.log(school)
    }
```

#### const 常量声明
> 数组和对象为了保持不变，可以使用常量定义。


1、  必须要赋初始值。  
2、  一般使用大写。
3、  常量的值不能修改。  
4、  块级作用域。
```javascript
{
    const NAME = 'xueliang';
}
console.log(NAME);
```
5、  对于数组和对象元素的修改，不算对常量的修改。即保持常量存储的指针保持不变。

#### 解构赋值
> 批量从数组、对象中取值

```javascript
// 数组批量赋值
const names = ['randy orton', 'John sena', 'hbk'];
let [stu1, stu2, stu3] = names;
console.log(stu1, stu2, stu3);      // randy orton John sena hbk

// 对象批量赋值
const human07 = {
    name: 'human07',
    age: 32,
    slogon: 'guess we can'
    run: function() {
        console.log("I'm running");
    }
};
let {name, age, slogon, run} = human07;
run();  // I'm running
```  

#### 模板字符串
1、 引用新的字符串声明 ``
2、 内容中可以直接出现换行符  
3、 支持占位符方式渲染字符串
```javascript
let item1 = 'pig';
let item2 = 'dog';
let content = `
<ul>
    <li>${item1}</li>
    <li>${item2}</li>
</ul>`;
console.log(content)
```

#### 对象声明简化
ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。
```javascript
let name = "human0722";
let bellow = function() {
    console.log("a~");
}

let me = {
    name,   //相当于  name: name,
    bellow, // 相当于: bellow : function(){}
    sleep() {   // 相当于: sleep : function() {}
        console.log("sleep~");
    }
}

```

#### 箭头函数
ES6中允许使用箭头(=>)定义函数  
1、 箭头函数中的 this 是静态的，始终指向函数声明时所在作用域下的 this 的值。  
2、 不可使用 arguments 变量，可简写。
```html
// 静态 this
<script type="text/javascript">
    let getName = function() {
        console.log(this.name)
    }
    let getName2 = ()=>{
        console.log(this.name)
    }

    window.name = "window.name"
    const school = {
        name: "school.name"
    };

    getName();              // window.name
    getName2();             // window.name

    getName.call(school);   // school.name
    getName2.call(school);  // window.name

</script>
```

#### 箭头函数小应用
```html
<html>
    <head>
        <style type="text/css">
            
        </style>
    </head>
    <body>
        <div id="box"></div>
    </body>
    <script type="text/javascript">
        let box = document.getElementById("box");
        box.addEventListener("click", function() {
            console.log(this);                  // div 
            setTimeout(function(){
                console.log(this)               // window
                this.style.backgroundColor = "pink"; // window.style not defined
            },2000);
        });
        box.addEventListener("click", function() {
            let that = this;
            setTimeout(function(){
                that.style.backgroundColor = "pink";    // OK
            },2000);
        });
        box.addEventListener("click", function() {
            console.log(this);     // div
            setTimeout(()=>{
                console.log(this);  // div
                this.style.backgroundColor = "pink";    // OK
            },2000);
        });
    </script>
</html>
```

#### 函数参数
1、 函数参数可以有默认值  
2、 参数可以和对象解构赋值配合使用

```javascript   
function add(a, b, c=10) {
    return a + b + c;
}
console.log(add(2, 4))

function connect({host, port, username, passwd}) {
    console.log(host);
    consolt.log(port);
}
connect({
    host: 'localhost',
    port: 3306, 
    username: 'randy',
    passwd: 'randy123'
});
```

#### rest 参数
> 可以接受动态数量的参数， ES6 与 ES5 不同。

```javascript
    // es5: arguments
    function func1() {
        console.log(arguments)
    }
    func1(42)   // 对象 {0: 42}
    // es6: rest参数
    function func2(...args) {
        console.log(args)
    }
    func(42)    // 数组 [42]
```

#### 拓展运算符
> ... 拓展运算符能将【数组】 转化为逗号分隔的【参数序列】

```javascript
const colors = ['Red', 'Blue', 'Green'];
function mix() {
    console.log(arguments);
}

mix(colors);    // {'0': 'Red', '1': 'Blue', '2': 'Green'}
```
#### 拓展运算符的应用
1、 数组的合并  
2、 数组的克隆  
3、 伪数组转换为真正的数组

```javascript
// array merge
const arr1 = ['a', 'b'];
const arr2 = ['c', 'd'];
const arrRestult = [...arr1, ...arr2];
// array clone
const arrSource = ['e', 'f']
const arrTarget = [...arrSource];
// array convert
const divs = document.querySelectorAll('div');
const trueDivs = [...divs];
console.log(trueDivs);
```

#### Symbol 类型
> 为了解决模块化功能扩充带来的属性名冲突问题 ,是 JS 第七种数据类型  Undefined string symbol object null number boolean URSNB

1、 Symbol 的值是唯一的，用来解决命名冲突的问题  
2、 Symbol 的值不能与其他数据进行运算  
3、 Symbol 定义的对象属性不能使用 for...in 循环遍历,但是可以使用 Reflect.ownkey 来获取对象的所有键名

```javascript
    // Create
    let s1 = Symbol();
    console.log(s1, typeof s1);     //Symbol symbol
    let s2 = Symbol("flag");
    let s3 = Symbol("flag");
    console.log(s2 === s3);     // false
    let s4 = Symbol.for("identification");
    let s5 = Symbol.for("identification");
    console.log(s4 === s5);     // true

    // add function to object: demo1
    let game = {

    };
    let methods = {
        up: Symbol(),
        down: Symbol()
    };
    game[methods.up] = function() {
        console.log("up");
    };
    game[methods.down] = function() {
        console.log("down");
    }
    // add function to object: demo2
    let play = {
        [Symbol("say")]: function() {
            console.log("game say");
        },
        [Symbol("sing")]: function() {
            console.log("game sing");
        }
    }
    // Symbol 内置值
    // more: https://es6.ruanyifeng.com/#docs/symbol#%E5%86%85%E7%BD%AE%E7%9A%84-Symbol-%E5%80%BC
    class Person {
        static [Symbol.hasInstance] (param) {
            console.log(param)
            console.log("used for hasInstance");
            return true;
        }
    }
    let o = {
        name: "hello"
    };
     console.log(o instanceof Person);
```  

#### 迭代器 Iterator
> 迭代器配合 for of 使用

```javascript
// for of 循环
const chars = ['A', 'B', 'C', 'D'];
for (let v of chars) {
    console.log(v);     // A B C D
}
// 迭代器循环
let iterator = chars[Symbol.iterator]();
console.log(iterator.next());     //{value: 'A', done: false}
console.log(iterator.next());     //{value: 'B', done: false}
console.log(iterator.next());     //{value: 'C', done: false}
console.log(iterator.next());     //{value: 'D', done: false}
console.log(iterator.next());     //{value: undefined, done: true}
```  


自定义迭代器:
```javascript
const school = {
        name: 'human0722',
        stus: [
            'Human',
            'Randy',
            "John",
            "Bush"
        ],
        [Symbol.iterator]() {
            let index = 0;
            let that = this;
            return {
                next: function() {
                    if (index < that.stus.length) {
                        return {value: that.stus[index++], done: false};
                    }else {
                        return {value: undefined, done: true};
                    }
                }
            }
        }
    };
    for (let v of school) {
        console.log(v);
    }
```  

#### Generator 函数
> 一种特殊的支持异步编程的函数

1、 定义时 function 后加 *,函数体内用 yield 关键字标记暂停状态;调用时不执行，返回一个 Iterator.使用 next() 方法运行,遇到 yield 暂停并返回该 yield 处的信息。

```javascript
// create
function* helloGenerator() {
    yield 'hello';
    yield 'generator';
    return 'hello';
}
//获得 Iterator 而不是执行。
let hg = helloGenerator();
//调用 deomo1
console.log(hg.next());     // util yield 'hello'
console.log(hg.next());     // util yield 'generator';
console.log(hg.next());     // util return 'hello'
//调用 demo2
for (let item of hg) {
    console.log(item);
}
```

2、 next() 方法调用时候可以传入实参, 实参将作为本次 next() 返回值所对应的 yield 的上一个 yield 的返回值。 第一次调用 next() 的参数将被弃用。
```javascript
function* hello(arg) {
    console.log(arg);
    let v1 = yield 'hello';
    console.log(v1);
    let v2 = yield 'world';
    console.log(v2)
    return 'end';
}

let cur = hello('aaa'); 
cur.next('bbb');    // aaa
cur.next('ccc');    // ccc
cur.next('ddd');    // ddd
```

#### Generator 函数应用
1、 解决回调地狱
```javascript
// before
    function f1() {
    setTimeout(()=>{
        console.log("aaa");
        setTimeout(()=>{
            console.log("bbb");
            setTimeout(()=>{
                console.log("ccc");
            }, 1000);
        },1000);
    }, 1000);
}
f1();

// now
function f1() {
    setTimeout(()=>{
        console.log("aaa");
        iterator.next();
    }, 1000);
}

function f2() {
    setTimeout(()=>{
        console.log("bbb");
        iterator.next();
    }, 1000);
}

function f3() {
    setTimeout(()=>{
        console.log("ccc");
        iterator.next();
    }, 1000);
}

function * controller() {
    yield f1();
    yield f2();
    yield f3();
}

let iterator = controller();
iterator.next();
```

#### Promise 对象
> 异步编程的另一种方案

1、 实例化 Promise 时传入两个函数名: resolve 和 reject。当在函数体中使用 resolve() 时，会调用 Promise 对象 then 方法的第一个函数参数。 reject() 会调用第二个函数参数。 Promise 可只传入一个参数，默认为 resolve, then() 形参只有一个时，默认被 resolve() 调用。

```javascript
let p = new Promise(function(resolve, reject) {
    let flag = 0; 
    if (flag === 0) {
        let data = "data from database";
        resolve(data);                  // call function A in p.then(A,B)
    }else {
        let data = "error data";
        reject(data);                   // call function B in p.then(A,B)
    }
});

p.then(function(value){
    console.log(value);
},function(reason){
    console.error(reason);
});
```  

2、 Promise.then() 的返回值是一个 Promise 对象， 所以 then() 可以链式调用。

```javascript
let p = new Promise((resolve, reject)=>{
    resolve("call p.then");
});
p.then(value=>{
    resolve("call next p.then");
}).then(value=>{
    console.log(value);     // call next p.then
});
```  

3、 Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。在 then() 中返回不同的值, Promise 会获得不同的状态。

```javascript
let p = new Promise(...);
p.then(function(value){
    console.log(value);     // no return == return undefined, {PromiseState:"fulfilled", PromiseResult: undefined}
});

p.then((value)=>{
    return "hello";         // {PromiseState: "fulfilled", PromiseResult: "hello"}
});

```  
> 当返回值是 Promise 对象时，状态由这个 Promise 对象决定。

4、 Promise 对象异常捕捉。
```javascript
let p = new Promise(...);   
p.catch((reason)=>{         // 捕捉 reject() 函数调用
    console.log(reason);
});
```

#### Set 集合API
> Set 为不重复的集合, 实现了 Iterator 接口。

```javascript
// init
let s1 = new Set();
let s2 = new Set(['a', 'b', 'c', 'd', 'c']);
console.log(s2.size);
s2.add('h')         // 新增
s2.delete('h')      // 删除
s2.detect('h')      // 检测
s2.clear()          // 清空

// 去重
let arr = [1,2,3,4,5,6,5,4,3,2,1];
let result = [...new Set(arr)];
//交集
let arr2 = [4,5,6,6,5];
let result2 = [...new Set(arr)].filter(item => {
    let s2 = new Set(arr2);
    if (s2.has(item)) {
        return true;
    } else {
        return false;
    }
});
// 并集
let union = [...new Set([...arr, ...arr2])];
//差集
let diff = [...new Set(arr)].filter(item=>!new Set(arr2).has(item));
```  

#### Map API
> 键值对，实现了 Iterator接口.


```javascript
let m = new Map();
m.set("name", "human0722");

// object 类型的 key
let key = {
    name: 'randy'
};
m.set(key, "key");
console.log(m.get(key));
m.delete(key);
m.clear();
```

#### Class语法
1、 对象声明: ES5 vs ES6

```javascript   
// ES5
function location(x, y) {
    this.x = x;
    this.y = y;
}
location.prototype.toString = function() {
    return "(" + this.x + "," + this.y + ")";
}
let f = location(1,2);
console.log(f);     // (1,2)
// ES6
Class Location{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return "(" + this.x + "," + this.y + ")";
    }
}
```  
2、静态属性和方法
```javascript
Class Location{
    static name = "randy";
    static function run() {
        console.log("I'm running...");
    }
}
let location = new Location();
location.run()  // error
console.log(Location.name);     // randy
Location.run();                 // I'm running
```  
3、 子类继承: ES5 vs ES6

```javascript
// ES5
function Phone(brand, price) {
    this.brand = brand;
    this.price = price;
}
Phone.prototype.call = function() {
    console.log("I can call");
}

function SmartPhone(brand, price, color, size) {
    Phone.call(this, brand, price);
    this.color = color;
    this.size = size;
}
SmartPhone.prototype = new Phone;
SmartPhone.prototype.constructor = SmartPhone;

SmartPhone.prototype.photo = function() {
    console.log("I can take photots");
}

let smartPhone = new SmartPhone("huawei", 9999, "red", 5,8);
console.log(smartPhone);

// ES6
class Phone{
    constructor(brand, price) {
        this.brand = brand;
        this.price = price;
    }
    call() {
        console.log("I can call");
    }
}
class SmartPhone extends Phone{
    constructor(brand, price, color, size) {
        super(brand, price);
        this.color = color;
        this.size = size;
    }
    photo() {
        console.log("I can take photot");
    }
}
```


#### Module 模块化

1、 暴露语法汇总
```javascript
// demo1.js
export let name = 'human0722';
export let run = function() {
    console.log("run...");
}
// +
<script type='module'>
    import * as m from 'demo1.js'
    console.log(m.name)
</script>

// demo2.js
let name = 'human0722';
let run = function() {
    console.log("run...");
}
export {name, run}
<script type='module'>
    import * as m from 'demo2.js'
    console.log(m.name)
    import {name as myname, run} from 'demo2.js'
    console.log(myname)
</script>

// demo3.js
export default {
    name: 'human0722',
    run: function() {
        console.log("run...");
    }
}
<script type="module">
    import m3 from 'demo3.js'
    m3.default.run();
</script>
```

2、 最佳实践
```javascript
// app.js
import * as m1 from 'demo1.js'
import * as m2 from 'demo2.js'
import * as m3 from 'demo3.js'
// html
<script type="module" src="./src/js/app.js"></script>
```

3、 Bable 打包实验
> 直接引入 js 文件可能不被浏览器支持。 一般使用 Babel 转换后引入。

```javascript
├── lab  
│   ├── home.html  
│   └── js  
│       ├── app.js  
│       ├── m1.js  
│       ├── m2.js  
│       └── m3.js  

// app.js
import * as m1 from './m1.js'
import * as m2 from './m2.js'
import * as m3 from './m3.js'
```
先要在 lab 文件夹中执行 `npm init` 初始化项目, 然后执行 `npm i babel-cli babel-preset-dev browserify -D` 安装三个软件包。`-D` 表示这是开发依赖。  
通过命令 `npx babel js/ -d dist/js --presets=babel-preset-dev` 来将 ES6语法转换为低版本语法。此时生成的 app.js 依旧无法被浏览器识别，因为内部 `require` 函数浏览器无法识别。无法加载多个依赖模块。使用 npx 是因为软件包没有在全局安装。  
再通过打包工具命令 `npx browserify dist/js/app.js -o dist/js/bundle.js` 将多个依赖的 js 文件打包到同一个 js 文件中，可以被浏览器识别。

```html
<!--home.html-->
<script type='module' src="js/bundle.js">
```

