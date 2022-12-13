---
title: JavaScript语言基础
date: 2022-12-12 10:36:15
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 999
---

### 区分基本类型和对象类型

#### 基本类型不可变性

在ECMAScript标准中，它们被定义为primitive value，即原始值，代表值本身是不可被改变的。

在JavaScript中，每一个变量在内存中都需要一个空间来存储

<!--more-->

内存空间被分为两种，栈内存和堆内存

栈内存的特点：

- - 存储的值大小固定

- - 空间较小
  - 可以直接操作其保存的变量，运行效率高
  - 由系统自动分配存储空间

由于栈中的内存空间的大小是固定的，那么注定了存储栈中的变量就是不可变的

### 引用类型

- - 存储的值大小不定，可动态调整

- - 空间较大，运行效率低

- - 无法直接操作其内部存储，使用引用地址读取

- - 通过代码进行分配空间

相对于上面具有不可变性的原始类型，我习惯把对象成为引用类型，引用类型的值实际存储在堆内存中，它在栈中值存储了一个固定长度的地址，这个地址指向堆内存中的值

#### 复制

当我们把一个变量的值复制到另一个变量上时，原始类型和引用类型的表现是不一样的

**原始类型**

内存中有一个变量name，值为ConardLi。我们从变量name复制出一个变量name2，此时在内存中创建了一块新的空间用于存储ConardLi，虽然两者值都是相同的，但是两者指向的内存空间完全不同，这两个量参与任何操作都互不影响。

```javascript
var name = 'fsang';
var name2 = name;
name2 = 'code秘密花园';
console.log(name); // fsang;
```

![image-20221213164248277](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213164248277.png)

**引用类型**

当我们复制引用类型的变量时，实际上复制的是栈中存储的地址，所以复制出来的obj2实际上和obj指向的堆中同一个对象。因此，我们改变其中任何一个变量的值，另一个变量都会收到影响，这也是为什么会有深拷贝和浅拷贝的原因

```javascript
var obj = {name:'ConardLi'};
var obj2 = obj;
obj2.name = 'code秘密花园';
console.log(obj.name); // code秘密花园
```

![image-20221213164404630](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213164404630.png)

#### 比较

对于原始类型，比较时会直接比较他们的值，如果值相等，即返回true

对于引用类型，比较时会比较他们的引用地址，虽然两个变量在堆中存储的对象具有的属性都是相等的，但是它们被存储在了不同的存储空间，因此比较值为false

```javascript
var name = 'ConardLi';
var name2 = 'ConardLi';
console.log(name === name2); // true
var obj = {name:'ConardLi'};
var obj2 = {name:'ConardLi'};
console.log(obj === obj2); // false
```

![image-20221213164453686](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213164453686.png)

#### 值传递和引用传递

```javascript
let name = 'ConardLi';
function changeValue(name){
  name = 'code秘密花园';
}
changeValue(name);
console.log(name);
```

执行上面的代码，如果最终打印出来的name是'ConarLi'，没有改变，说明函数参数传递的是变量的值，即值传递。如果最终打印的是'code秘密花园'，内书内部的操作可以改变传入的变量，那么说明函数参数传递的是引用，即引用传递。

很明显，上面的执行结果是'ConarLi'，即函数参数仅仅是被传入变量复制的一个局部变量，改变这个局部变量不会对外部变量产生影响。

```javascript
let obj = {name:'ConardLi'};
function changeValue(obj){
  obj.name = 'code秘密花园';
}
changeValue(obj);
console.log(obj.name); // code秘密花园
```

上面的代码可能会产生疑惑，是不是参数是引用类型就是引用传递呢

首先明确一点，ECMAScript中所有的函数的参数都是按值传递的

当函数参数是引用类型时，同样将参数复制了一个副本到局部变量，只不过复制的这个副本是指向堆内存中的地址，我们在函数内部对对象的属性进行操作，实际上和外部变量指向堆内存中的值相同，但是这并不代表着引用传递

```javascript
let obj = {};
function changeValue(obj){
  obj.name = 'ConardLi';
  obj = {name:'code秘密花园'};
}
changeValue(obj);
console.log(obj.name); // ConardLi
```

函数参数传递的并不是变量的引用，而是变量拷贝的副本，当变量是原始类型时，这个副本就是值本身，当变量是引用类型时，这个副本是指向堆内存的地址。

最后注意：函数参数都是按照值传递的

### 引用数据类型(对象类型)

#### Array

**创建数组的方法**

const arr = [1,2,3]// 数组字面量

const arr = [,,,]// 三元素空位数组（hole array）

const arr = new Array(4)// [,,,,]

const arr = new Array(4,2)// [4,2]

const arr = Array.of(1,2,3)// [1,2,3]

const arr = Array.of(4)// [4]

**操作数组的方法**

concat() 连接两个或多个数组，并返回已连接数组的副本

copyWithin() 将数组中的数组元素复制到指定位置或从指定位置复制

entries() 返回键 / 值对数组迭代对象

every() 检查数组中的每一个元素是否通过测试

fill() 用静态值填充数组中的元素

filter() 使用数组中通过测试的每个元素创建新数组

find() 返回数组中第一个通过测试的元素的值

findIndex() 返回数组中通过测试的第一个元素的索引

forEach() 为每个数组调用函数

Array.from() 方法用于通过拥有 length 属性的对象或可迭代的对象来返回一个数组

includes() 检查数组是否包含指定的元素

indexOf() 在数组中搜索元素并返回其位置

join() 将数组的所有元素连接成一个字符串

keys() 返回Array Iteration对象，包含原始数组的键

lastindexOf() 在数组中搜索元素，从末尾开始，并返回其位置

map() 使用为每个数组元素调用函数的结果创建新数组

pop() 删除数组的最后一个元素，并返回该元素

push() 将新元素添加到数组的末尾，并返回新的长度

reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。数组的值减为单个值

reduceRight() 方法接收一个函数作为累加器，数组中的每个值（从右到左）开始缩减，最终计算为一个值。数组的值减为单个值

reverse() 反转数组中元素的顺序

shift() 删除数组的第一个元素，并返回该元素

slice() 选择数组的一部分，并返回该元素

some() 检查数组中的任意元素是否通过测试

sort() 对数组的元素进行排序

splice() 用于添加或删除数组中的元素

toString() 将数组转为字符串，并返回结果

unshift() 将新元素添加到开头，并返回新的长度

valueOf() 返回数组的元素值

flat() 将嵌套数组转成一维数组

#### Object

JavaScript对象的原生方法分成两类：Object本身的方法和Object的实例方法

Object本身的方法就是直接定义在Object的方法。如Object.print = function (o) => {onsole.log(o)}

Object的实例方法就是Object原型对象Object.prototype上的方法，可以直接被Object实例直接使用。如Object.prototype.print = function () =>{console.log(this)}   var obj = new Object(); obj.ptrint()

Object.keys() 参数是一个对象，返回该对象自身的所有属性名

Object.balues() 返回一个数组，成员是参数对象自身的所有可遍历属性的键值。与Object.keys相对接

Object.entries() 返回一个数组，成员是参数对象自身的所有可遍历属性的键值对数组

Object.getOwnPropertyDescriptor() 获取某个属性的描述对象

Object.defineProperty()通过描述对象，定义某个属性

Object.defineProperties() 通过描述对象，定义多个属性

Object.preventExtensions() 防止对象扩展

Object.isExtensible() 判断对象是否可扩展

Object.seal() 禁止对象配置

Object.isSealed() 判断一个对象是否可配置

Object.freeze() 冻结一个对象

Object.isFrozen() 判断一个对象是否被冻结

Object.create() 该方法可以指定对象和属性，返回一个新的对象

Object.gerPrototypeOf() 获取对象的Prototype对象

Object.prototype.valueOf() 返回当前对象对应的值

Object.prototype.toString() 返回当前对象对应的字符串形式

Object.prototype.toLoacaleString() 返回当前对象对应的本地字符串形式

Object.prototype.hasOwnProperty() 判断某个属性是否为当前对象的属性，还是继承当前原型对象的属性

Object.prototype.isPrototypeOf() 判断当前对象是否为另一个对象的原型

Object.prototype.propertyIsEnumerable() 判断某个属性是否可枚举

#### Function

Function普通函数

Arrow Function 箭头函数适用于需要匿名哈数的地方

函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象

不可以当作构造函数，也就是不可以使用new命令，否则会抛出一个错误

不可以使用argument对象，该对象在函数体内不存在，如果要用可以用rest参数代替

Generator函数用来返回generator对象，并且它符合可迭代协议和迭代器协议，是一个协程函数，它通过yield命令来暂停某个进程，执行其他线程，通过Generator函数实现异步避免回调地狱，但是因为切换下个状态都要用next方法，所以不常用，一般用es6中async函数解决。

Async Function用来处理异步操作，避免了回调地狱，让异步代码看起来更友好。

Async函数调用async的时候会异步执行

在async函数中使用await的时候，会执行当前代码才会往下继续执行其他代码，实现按照指定顺序执行异步操作

async函数会返回一个Promise

apply和call都是函数对象的方法，两者都可以改变函数运行时的this，这个是apply和call的主要使用的功能

apply和call不同在于，提供的参数格式不一样：apply需要的是一个参数数组，call需要的是参数列表

bind与apply和call不同的是，apply和call是在每次调用的时候动态指定被调用函数的this和实参，apply与call自动帮我们对目标函数进行调用，而bind是创建一个新的绑定函数固定了目标函数的this值和部分实参

立即执行函数表达式，是一个在定义时就会立即执行的JS函数(function(){})()

第一部分是包围在圆括号运算符（）里的一个匿名函数，这个匿名函数拥有独立的词法作用域。这不仅避免了外界访问IIFE中的变量，而且也不会污染全局作用域。

第二部分再一次使用（）创建了立即执行函数表达式，javaScript引擎到此直接执行函数

符合以下两点的函数就是纯函数

相同输入总是会返回相同的输出。返回的结果只依赖于输入的参数且于外部系统状态无关

没有副作用。不会影响该函数作用域以外的外部状态（比如全局变量、参数）

柯里化就是把接受多个参数的函数变成接受一个单一参数（最初函数的第一个参数）的函数，并且返回一个新的函数的技术，新函数接受余下参数并返回运算结果

#### Date

```javascript
//-8小时
var cur=new Date(2018,2,25,14,6,38); //0~11代表1月~12月
var year=cur.getUTCFullYear(); 
var month=cur.getUTCMonth();  
var day=cur.getUTCDate();
var hour=cur.getUTCHours();
var minutes=cur.getUTCMinutes();
var seconds=cur.getUTCSeconds();
var mseconds=cur.getUTCMilliseconds();
console.log("时间为："+year+"-"+(month+1)+"-"+day+" "+hour+":"+minutes+":"+seconds+":"+mseconds);
//打印结果
时间为：2018-3-25 6:6:38:0

var cur=new Date(2018,2,25,14,6,38); //0~11代表1月~12月
var week=cur.getDay();
var arr=["星期一","星期二","星期三","星期四","星期五","星期六","星期天"]
console.log("本地时间是："+arr[week]);
var weekUTC=cur.getUTCDay();
console.log("格林威治时间是："+arr[weekUTC]);
//打印结果
本地时间是：星期一
格林威治时间是：星期一
```

#### Regex

![image-20221213170250905](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213170250905.png)

g:global--全文搜索，不添加，搜索到第一个匹配为止

i:ignore case--忽略小写写，默认大小写敏感

m:multiplelines--多行搜索

#### Error

SyntaxError 语法错误。多半是哪里的符号写错了

ReFerenceError 引用错误。根本没有创建过就去使用

TypeError 类型错误。不是你的方法你去调用了

RangeError 范围错误

```javascript
try{
//可能出错的代码;
}catch(){
console.log(err);//可以提示用户错误的原因是什么
}//后续代码正常执行
throw new Error("抛出一个自定义的错误")
```

### 值类型（基本类型）

#### number

**浮点数**

数值包含小数点，而且小数点后面必须至少有一个数字。

经典问题：0.1+0.2 ==0.3吗？答案是不等于

因为在浮点数运算过程中存在舍入误差，之所以存在这种舍入错误，是因为使用了IEEE754，这种错误并非ECMAScript独有，只要是使用这种格式的语言都有这个问题

**值的范围**

正数，负数，0，Infinity

最小值：Number.MIN_VALUE = 5e-324

最大值：Number.MAX_VALUE = 1.797693134862315 7e+308

数值超出JavaScript表示的范围：Infinity（正无穷大）-Infinity（负无穷大）

确定数值是否为有限数：isFinite()函数

**NaN**

- 意思：不是数值
- 表示本来要返回数值的操作失败了（而不是抛出错误）
- 如何涉及NaN的操作始终返回NaN
- NaN不等于包括NaN在内的任何值
- isNaN()函数，判断传入其中的参数是否不是数值
- isNaN()会尝试转换成数值

**数值转换**

Number()函数，可用于任何数据类型

![image-20221213171131994](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213171131994.png)

pareInt()函数会忽略字符串最前面的空格，第一个非空格字符开始转换，如果第一个字符不是数值字符、加号或减号，parseInt()立即返回NaN

parseInt()函数可以接受第二个参数，用于指定底数

```javascript
parseInt("AF", 16); // 175 提供了16进制参数，可以省略0x
parseInt("AF"); // NaN , 没有提供第二个参数，就不可以
```

parseFloat()和pareInt()函数类似。区别在于parseFloat()识别有效的小数点（也就是第一次出现的小数点，在后面的小数点就都忽略了），还有就是忽略字符串开头的零；parseFloat()只解析十进制值，不能指定底数，十六进制数值始终返回0

toFixed()保留小数点后N位（最后的结果是字符串）

valueOf()方法返回对象的数字字面量

toString()方法将数字转换为字符串

toLocalString()方法将数字转换为本地惯例格式数字的字符串

toExponential()方法返回数值四舍五入后的指数表示法(e表示法)的字符串表示，参数表示转换后的小数位数

oPrecision()方法接收一个参数，即表示数值的所有数字的位数(不包括指数部分)，自动调用toFixed()或toExponential()

**string**

Javascript采用UTF-16编码的Unicode字符集，Javascript中的字符串是由一组无符号的16位值组成的序列，最常用的Unicode字符都是通过16位的内码来表示的，并代表字符串的单个字符

只要引用了字符传的属性，JavaScript就会将字符串通过new String()的方式转换为对象，这个对象继承了字符串的方法，一旦引用结束，这个新创建的对象就会被销毁。这个临时对象称之为包装对象，字符串（还有数字和布尔值）的属性都是只读的，并不能赋值，有别于其他的对象字符串是存放再堆内存里面的，一旦创建就不可更改，如果想改变某个变量保存的字符串，就必须销毁原来的字符串，再用一个新的来填充该变量

String类型是字符串的包装类型，可以用String构造函数来创建

```javascript
var stringObject = new String('hello world');
var stringText = 'hello world';
```

**String([value])**

```javascript
let a = 111
String(a) // '111'
```

[value].toString()

```javascript
转换数字的进制（2-36进制）
const a = 10
a.toString(2) //"1010"
a.toString(8) //"12"
a.toString(16) // "a"
Math.random().toString(36).subString(3,7) // 生成四位数的随机验证码
判断数据
Object.prototype.toString.call(Array) // "[object Function]"
Object.prototype.toString.call([]) // "[object Array]"
```

**charAt()**

```javascript
以单字符字符串的形式返回给定位置的那个字符
var stringValue = 'hello world';
console.log(stringValue.charAt(1)); // 'e'
```

**charCodeAt**

```
返回给定位置的字符所对应的字符编码
var stringValue = 'hello world';
console.log(stringValue.charCodeAt(1)); // '101'
```

**concat()**

```
将一个或者多个字符串拼接起来，返回拼接得到的字符串，可接受任意多个参数
var stringValue = 'hello world';
console.log(stringValue.charCodeAt(1)); // '101'
//实际情况中使用+拼接的情况更多
```

**slice(start,end)**

```
截取字符串，返回一个新的字符串（当传入负值时，会默认加上原数组的长度）
start：指定子字符串的起始位置（可不传，不传返回原字符串）
end：指定字符串到哪个位置结束（可不传，不传默认到原字符最后一个字符结束）
var stringValue = 'hello world';
console.log(stringValue.slice()); // 'hello world';
console.log(stringValue.slice(2)); // 'llo world'
console.log(stringValue.slice(2, 6)); // 'llo '
console.log(stringValue.slice(-9)); // 'llo world'
console.log(stringValue.slice(2, -5)); // 'llo '
```

**suubstr(start,length)**

```
截取字符串，返回一个新的子字符串（当第一个参数为负值时，会默认加上原数组的长度，第二个参数为负值时，会默认转为0）
start：指定子字符串的起始位置（可不传，不传则返回原字符串）
length：指定子字符串的长度（可不传，不传默认原字符串最后一个字符结束）
var stringValue = 'hello world';
console.log(stringValue.substr()); // 'hello world';
console.log(stringValue.substr(2)); // 'llo world'
console.log(stringValue.substr(2, 6)); // 'llo wo'
console.log(stringValue.substr(-2)); // 'ld'
console.log(stringValue.substr(2, -6)); // ''
```

**substring(start,end)**

```
截取字符串，返回一个新的子字符串（当传入负值时，会将所有负值转为0。如果start大于end,两个值会互相调换，保持start<end）
start：指定子字符串的起始位置（可不传，不传返回原字符串）
end：指定字符串到哪里结束（可不传，不传默认到原字符串最后一个字符结束）
var stringValue = 'hello world';
console.log(stringValue.substring()); // 'hello world';
console.log(stringValue.substring(2)); // 'llo world'
console.log(stringValue.substring(2, 6)); // 'llo '
console.log(stringValue.substring(-3)); // 'hello world'
console.log(stringValue.substring(2, -6)); // 'he'
```

**indexOf(char,start)和lastIndexOf(char, start)**

```
indexOf从字符串的开头向后搜索子字符串，返回第一个子字符串的位置（未找到返回-1）
lastIndexOf从字符串的末尾向前搜索子字符串，返回子字符串的位置（没找到则返回-1）
char：需要查找的字符串
start：从哪个位置开始向后查找，可不传
var stringValue = 'hello world';
console.log(stringValue.indexOf('o')); // 4
console.log(stringValue.lastIndexOf('o')); // 7
console.log(stringValue.indexOf('o', 6)); // 7
console.log(stringValue.lastIndexOf('o', 6)); // 4
```

**trim()**

```
去除原始字符串中的前置及后缀空格，返回一个新的字符串
var stringValue = '  hello world  ';
console.log(stringValue.trim()); // 'hello world'
console.log(stringValue); // '  hello world  '
```

**toLowerCase()**

```
将字符串转为小写
var stringValue = 'HELLO WORLD';
console.log(stringValue.toLowerCase()); // 'hello world'
console.log(stringValue.toLocaleLowerCase()); // 'hello world'
```

**toUpperCase()**

```
将字符串转为大写
var stringValue = 'hello world';
console.log(stringValue.toUpperCase()); // 'HELLO WORLD'
console.log(stringValue.toLocalUpperCase()); // 'HELLO WORLD'
```

**match()**

```
match只接受一个参数，要么是一个正则表达式，要么是一个RegExp对象，返回一个数组
var test = 'cat, bat, sat, fat';
var pattern = /.at/;
var matches = test.match(pattern);
console.log(matches); // 输出匹配到的东西
```

**search()**

```
search()只接受一个参数，要么是一个正则表达式，要么是一个RegExp对象
返回字符串中第一个匹配的索引，如果没有，则返回-1
var test = 'cat, bat, sat, fat';
var index = test.search(/at/);
console.log(index);
```

**replace()**

```
replace()接受两个参数，第一个参数可以是一个RegExp对象或一个字符串（这个字符串不会被转换成正则表达式），第二个参数可以是简化替换子字符串的操作
var test = 'cat, bat, sat, fat';
console.log(test.replace('at', 'ond')); // 'cond, bat, sat, fat'
console.log(test.replace(/at/g, 'ond')); // 'cond, bond, sond, fond'
```

**split()**

```
可以基于指定的分隔符将一个字符串分割成多个字符串，返回一个数组
第一个参数可以是一个字符串或一个RexExp对象
第二个参数用于指定数组的大小，可不传
var test = 'red, blue, green, yellow';
console.log(test.split(',')); // '['red', blue', 'green', 'yellow']
console.log(test.split(',', 2)); // ['red', blue']
console.log(test.split(/[^,]+/)); // ['', '', '', '']
```

##### **boolean**

boolean只有两个值true和fasle

JavaScript中的任何值都可以转换成布尔值，其中只有六个转换为false，他们分别是 “0、NaN、null、undefined、空字符串” ，其余的都是true

![image-20221213172416788](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/image-20221213172416788.png)

tostring(),把布尔值转为字符串，并返回结果

```
const a = new Boolean(1) // Boolean {true}
a.toString() //‘true’
```

valueOf()返回Boolean对象的原始值

```
const a = new Boolean(1) // Boolean {true}
a.valueOf() //true
```

**null/undefined**

1. undefined不是关键字，而null是关键字
2. undefined和null被转换为布尔值的时候，两者都为false;
3. undefined在和null进行==比较时两者相等，全等于比较时两者不等
4. 使用Number()对undefined和null进行类型转换,undefined为NaN，null为0
5. undefined本质上是window的一个属性，而null是一个对象；
6. null表示没有对象，即不应该有值，经常作用函数的参数，或作为原型链的重点。undefined表示缺少值，即应该有值，但是还没有赋予（变量提升时会默认赋值为undefined，函数参数为提供默认为undefined，函数的返回值默认为undefined）

**symbol**

ES6引入了一种新的原始数据类型Symbol，表示独一无二的值，最大的用法就是用来定义对象的唯一属性名

基本用法

Symbol函数不能用new命令，因为Symbol是原始数据类型，不是对象。可以接受一个字符串作为参数，为新建的Symbol提供描述，用来描述在控制台或者作为字符串的时候使用，便于区分

```
let sy = Symbol('yy') // Symbol(kk)
typeof(sy)  //"symbol"
//相同的参数Symbol（）放回的值不相等
let sy1 = Symbol('kk')
sy === sy1 //false
```

使用场景

作为属性名使用，由于每一个Symbol的值都是不相等的，所以Symbol作为对象的属性名，可以保证属性不重名

Symbol作为对象属性名时不能用.运算符，要用方括号。因为.运算符后面是字符串，所以取到的是字符串sy属性，而不是Symbol值sy属性

```
let syObject = {};
syObject[sy] = "kk";
syObject[sy];  // "kk"
syObject.sy;   // undefined
```

在定义常量的时候，因为用字符串不能保证常量是独特的，这样会引起一些问题，但是使用Symbol定义常量，这样就可以保证这一组值都不相等，Symbol的值是唯一的，所以不会出现相同值的常量，即可以保证switch按照代码预想的方式执行。

Symbol.for()

Symbol.for()类似单例模式，首先会在全局搜索被登记的Symbol中是否有该字符串参数作为名称的Symbol值，如果有即返回该Symbol值，若没有则新建并返回一个以该字符串参数为名的Symbol值，并登记在全局环境中拱搜索

```
let yellow = Symbol("Yellow");
let yellow1 = Symbol.for("Yellow");
yellow === yellow1;      // false
let yellow2 = Symbol.for("Yellow");
yellow1 === yellow2;     // true
```

Symbol.ketFor()

Symbol.keyFor()返回一个已经登记的Symbol类型值的key，用来检测该字符作为名称的Symbol值是否已被登记

```
let yellow1 = Symbol.for("Yellow");
Symbol.keyFor(yellow1);    // "Yellow"
```

### JSON API

#### JSON.stringify()

使用方法 JSON.stringify(value,replacer?,space?)

可选参数replacer用于转换参数value

节点访问函数，会在值被转为字符串之前转换树节点的值

```
//序列化时，碰到数值，则乘以2
function replacer(key, value){
if(typeof value === 'number'){
value = 2 * value
}
return value
}
//调用
JSON.stringify({ a: 5, b: [2, 3] }, replacer)
//结果
"{"a":10,"b":[4,6]}"
```

属性白名单，用于隐藏那些非数组对象内属性不在这个列表中的所有属性

```
JSON.stringify({ foo: 1, bar: {foo: 1, bar: 1} }, ['bar'])
//结果
"{"bar":{"bar":1}"
```

可选参数space会影响输出格式，可以插入新行并通过数组和对象的嵌套增加缩进

如果是数字，则在字符串化时每一级别缩进这个数字值的空格，小于0解释成0，大于10解释成10

如果是一个字符串，则每一个级别会比上一个级别用该字符串形成的缩进（或该字符串的前十个字符）

被JSON.stringify忽略的数据，只考虑自身枚举属性，忽略不被支持的值，即除了对象、数组、字符串、布尔值和null以外的任何值。如函数，Symbol值，undefined等，将返回undefined。如果属性值是这些值，该属性直接被忽略，在数组中被解析成null

#### toJSON(key)方法

如果一个被序列化的对象拥有toJSON方法，那么该toJSON方法就会覆盖该对象默认的序列化行为：不是那个对象被序列化，而是调用toJSON方法后的返回值会被序列化

#### JSON.parse()

使用方法JSON.parse(text,reviver?)

""string""是不被js支持的，尽管虽然是标准的JSON字符串。你可以使用'"string"'代替。如果确实需要这样的行hi，可以使用JSON.stringify("")

revier参数是一个节点访问函数。它可以用来转换解析后的数据

### Math API

Math.abs() 返回参数的绝对值

Math.ceil() 向上取整，接受一个参数，返回大于该参数的最小整数

Math.floor() 向下取整

Math.max(n,m1...) 可接受多个参数，返回最大值

Math.min(n,n1...) 可接受多个参数，返回最小值

Math.pow(n,e) 指数运算，返回第一个参数为底数、第二个参数为幂的指数值

Math.sqrt() 返回参数值的平方根。如果参数是一个负值，则返回NaN

Math.log() 返回以e为底的自然对数值

Math.exp() 返回e的指数，也就是常数e的参数次方

Math.round() 四舍五入

Math.random() 返回0-1之间的一个伪随机数，可能等于0，但是一定小于1

### ES标准

#### ES6

**类**

对于熟悉Java，object-c，c#等纯面向对象语言的开发者来说，都会对class有一种特殊的情怀。ES6引入了class，让JavaScript的面向对象编程更加简单和易于理解。

通过生成实例对象的传统方法是通过构造函数

```
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```

实际上，ES6的class可以看作是一个语法糖，它的绝大部分功能，ES5都能做到，新的calss写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已，上面的代码用ES6的class改写如下

```
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```

ES6的类，完全可以看作构造函数的另一种写法

```
class Point {
  // ...
}

typeof Point // "function"
Point === Point.prototype.constructor // true
```

上面的代码表明，类的数据类型就是函数，类本身就是指向构造函数

使用的时候，也是直接对类使用new命令，跟构造函数的用法完全一致

```
class Bar {
  doStuff() {
    console.log('stuff');
  }
}

const b = new Bar();
b.doStuff() // "stuff"
```

构造函数的prototype属性，在ES6的类上面继续存在。事实上，类的所有方法定义在类的prototype属性上面

```
class Point {
  constructor() {
    // ...
  }

  toString() {
    // ...
  }

  toValue() {
    // ...
  }
}

// 等同于

Point.prototype = {
  constructor() {},
  toString() {},
  toValue() {},
};
```

constructor()方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor()方法，如果没有显式定义，一个空的constructor()方法会被默认添加

类的实例

使用new命令生成类的实例，如果忘记加上new，像函数那样调用Class，将会报错

```
class Point {
  // ...
}

// 报错
var point = Point(2, 3);

// 正确
var point = new Point(2, 3);
```

#### 模块化

模块的功能主要由export和import组成。每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过export来规定模块对外暴露的接口，通过import来引用其他模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突

ES6允许在一个模块中使用export来导出多个变量或函数

ES6将一个文件视为一个模块，上面的模块通过export向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

一条import语句可以同时导入默认函数和其他变量

import defaultMethod，{otherMethod} from ‘xxx.js’

#### 箭头函数

箭头函数的箭头=>之前是一个空括号、单个的参数名、或用括号包括起的参数名额，而箭头之后可以是一个表达式（作为函数的返回值），或者是用花括号括起的函数体（需要自行通过return来返回值，否则返回的是undefined）

```
()=>1
v=>v+1
(a,b)=>a+b
()=>{alert("foo")}
e=>{if(e===0){
return 0}
return 1000/e
}
```

不论是箭头函数还是bind，每次被执行都返回的是一个新的函数引用，因此如果你还需要函数的引用去做一些别的事情（臂如卸载监听器），那么你必须自己保存这个引用。

#### 函数参数默认值

ES6支持在定义函数的时候为其设置默认值

```
function foo(height = 50,color = 'red'){}
```

#### 模板字符串

ES6支持模板字符串，使得字符串的拼接更加的简洁、直观

```
//不使用模板字符串
var name = 'Your name is' + first + '' + last + '.'
//使用模板字符串
var name = `Your name is ${first} ${last}.`
```

#### 解构赋值

结构赋值语法是JavaScript的一种表达式，可以方便的从数组或者对象中快速提取值赋给定义的变量

从数组中获取值并赋值到变量中，变量的顺序与数组中对象顺序对应

```
var foo ["one","two","three","four"]
var [one,two,three] = foo

//如果需要忽略某些值，可以按照下面的写法获取想要的值
var [fisrt, , , last] = foo
```

如果从数组中没有取到值，你可以为变量设置一个默认值

```
var a,b
[a=5,b=7] = [1]
```

通过解构赋值可以方便的交换两个变量的值

```
var a = 1
var b = 3
[a,b] = [b,a]
```

获取对象中的值

```
const student = {
    name:'Ming',
    age:'18',
    city:'Shanghai'
}
const {name.age,city} = student
```

#### 延展操作符

延展操作符...可以在函数调用/数组构造时，将数组表达式或者string在语法层面展开；还可以在构造对象时，将对象表达式按key-value的方式展开

```
//函数调用
myFunction(...iterableObj)
//数组构造或字符串
[...iterableObj,'4',...'hello',6]
//构造对象时，进行克隆或者属性拷贝（ECMAScript2018规范新特性）
let objClone = {...obj}
```

应用场景

```
//在函数调用时候使用延展操作符
function sum(x,y,z){
    return x+y+z
}
const numbers = [1,2,3]
console.log(sum.apply(null,numbers))
console.log(sum(...numbers))
//构造数组
const students = ['jine','Tom']
const persons = ['Tony',...students,'Aaron','Anna']
//数组拷贝
var arr = [1,2,3]
var arr2 = [...arr]
arr2.push(4)
//连接多个数组
var arr1 = [0,1,2]
var arr2 = [3,4,5]
var arr3 = [...arr1,...arr2]
//等同于
var arr4 = arr1.concat(arr2)
//在ECMAScript 2018中延展操作符增加了对对象的支持
var obj1 = {foo:'bar',x:42}
var obj2 = {foo:'bar',y:13}
var cloneObj = {...obj1}
var mergeObj = {..obj1,...obj2}
<CustomComponent name:'jine',age={21} />
//等同于
const params = {
    name:'jine',
    age:21
}
<CustomComponent ...params />
//配合解构赋值避免传入一些不需要的参数
var params = {
    name：'123',
    title:'456',
    type:'aaa'
}
var {type,...other} = params
<CustomComponent type='normal' number={2},{...ohter} />
//等同于
<CustomComponent type='normal' number={2} name='123',title='456' />
```

#### 对象属性简写

在ES6中允许我们在设置一个对象的属性的时候不指定属性名

```
//不使用ES6
const name='Ming',age='18',city='Shanghai'
const student = {
    name:name,
    age:age,
    city:city
}
//使用ES6
const name='Ming',age:'18',city='Shanghai'
const student = {
    name,
    age,
    city
}
```

#### Promise

Promise是异步编程的一种解决方案，比传统的解决方案callback更加的优雅。它最早由社区提出和实现的，ES6将其写进了语言标准，统一了用法，原生提供了Promise对象

```
//不适用ES6，嵌套两个setTimeout
settimeout(function(){
    console.log('Hello')
    setTimeout(function(){
        console.log('Hello')
        setTimeout(function(){
            console.log('Hi')        
        },1000)    
    })
},1000)
//使用ES6
var waitSecond = new Promise(function(resolve,reject){
    setTimeout(resolve,1000)
})
waisecond
    .then(function(){
        console.log("Hello")
        return waitSecond    
    })
    .then(function(){
        console.log("Hi")    
  })
```

#### Let与Const

在之前JS是没有块级作用域的，const与let填补了这方面的空白，const与let都是块级作用域

### Qs API

Qs是一个流行的查询参数序列化和解析库。可以将一个普通的object序列化成一个查询字符串，或者反过来将一个查询字符串解析成一个object，而且支持复杂的嵌套

Qs.parse('x[]=1') //{x:['1']}

QS.stringify({x:[1]}) //x%5B0%5D=1

ignoreQueryPrefix和addQueryPrefix

ignoreQueryPrefix这个参数可以自动帮我们过滤掉location.search前面的？，然后再解析，addQueryPrefix设为true可以在序列化的时候给我们加上？

```
/ 解析
Qs.parse('?x=1') // {?x: "1"}
Qs.parse('?x=1', {ignoreQueryPrefix: true}) //  {x: "1"}
// 序列化
Qs.stringify({x: "1"}) //  x=1
Qs.parse({x: "1"}, {addQueryPrefix: true}) //  ?x=1
```

数组解析和序列化

数组序列化有几种方式：indices，brackets，repeat，comma，用来控制字符串的生成格式

```
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'indices' })
// 'a[0]=b&a[1]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'brackets' })
// 'a[]=b&a[]=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'repeat' })
// 'a=b&a=c'
qs.stringify({ a: ['b', 'c'] }, { arrayFormat: 'comma' })
// 'a=b,c'
```

以上四种方式，序列化得到的结果越来越来精简，但是当面对嵌套数组时，却会导致不同程序的信息丢失，而且丢失的越来月严重

```
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'indices' })) // { a: [['b'], 'c'] }
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'brackets' })) // {a: ["b", "c"]}
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'repeat' })) // {a: ["b", "c"]}
qs.parse(qs.stringify({ a: [['b'], 'c'] }, { arrayFormat: 'comma' })) // {a: "b,c"}
```

delimiter可以控制哪种字符作为分隔符，由于cookie的格式是使用，一个使用的例子是用来解析cookie

```
document.cookie // "_ga=GA1.2.806176131.1570244607; _jsuid=1335121594; _gid=GA1.2.1453554609.1575990858"
Qs.parse(document.cookie, {delimiter:'; '})
```

### 进制

#### 八进制

八进制字面值的第一位必须是0，然后是八进制数字序列（0-7）.如果字面值中的数值超出了范围，那么前导0将被忽略，后面的数值被当作十进制数解析

注意由于某些JavaScript的实现不支持八进制字面量，且八进制数字面量在严格模式下是无效，会导致JavaScript抛出错误

#### 十六进制

十六进制字面量的前两位必须是0X，后跟十六进制数字序列（0-9，a-f），字母可大可小。果十六进制中的数值超出范围，如出现g、h等会报错

#### 二进制

二进制字面值的前两位必须0b，如果出现除0、1以外的数字会报错

### JS模块化

#### commomJS

**特点**

获取依赖模块用同步加载方式，适合服务端，在浏览器使用会出现浏览器假死的情况，因为在服务端，所有的模块都存放在本地硬盘，可以同步加载完成，等待时间就是硬盘的读取时间

模块可以多次加载（多次使用require加载），但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果

**使用**

```
/*定义模块*/
//example.js
var n = 1;
function sayHello( name ){
    var name = name || "Tom";
    return "Hello~"+name
}
function addFn(val){
    var val = val.x+val.y;
    return val
}
/*使用module.exports的方法*/
module.exports ={
    n:n,
    sayHello:sayHello,
    addFn:addFn
}
/*
    使用exports的方法
    exports.n=n;
    exports.sayHello=sayHello
    exports.addFn=addFn

/*
    两种输出方式是等价的
*/

/*使用模块*/
//main.js
var example = require('./example.js');/*同步执行*/
var addNum = {
    "x":10,
    "y":5
}
console.log( example )//查看example输出的对外模块接口；
console.log( example.n )//1;
console.log( example.sayHello("Jack") )// "Hello~ Jack";
console.log( example.addFn(addNum) ) //15;
```

#### AMD

**特点**

获取依赖模块异步加载方式，适合浏览器端

**使用**

```
/*定义模块*/
/*
    define(id?, dependencies?, factory)
    id:字符串，模块名称(可选)
    dependencies: 是我们要载入的依赖模块(可选)，使用相对路径。,注意是数组格式
    factory: 工厂方法，返回一个模块函数
*/
//example.js
/*在定义模块时，也使用了其他依赖模块*/
define(['Lib'], function(Lib){
　　　　function foo(){
　　　　　　Lib.doSomething();
　　　　}
　　　　return {
　　　　　　foo : foo
　　　　};
　　});
　　
/*使用模块*/
/*
require( dependencies, factory)
    dependencies: 是我们要载入的依赖模块(可选)，使用相对路径。,注意是数组格式
    factory: 在这里使用模块完成业务
*/
/*
    将依赖的模块全部加载执行以后执行回调
*/
require(['./a', './b'], function (m1,m2) {
　m1.add(2, 3);
  m2.add(2, 3);
});
```

#### CMD

**特点**

延迟加载执行

**使用**

```
define(function(require, exports, module) {
  // 模块代码
    var a = require('./a');
  //require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口。
  
    //异步加载一个模块，在加载完成时，执行回调
    require.async('./b', function(b) {
        b.doSomething();
    });
    
    //异步加载多个模块，在加载完成时，执行回调
    require.async(['./c', './d'], function(c, d) {
        c.doSomething();
        d.doSomething();
    });
    
    
    //模块输出
     return {
        foo: 'bar',
        doSomething: function() {}
     };
    
    // 对外提供 foo 属性
    exports.foo = 'bar';

    // 对外提供 doSomething 方法
    exports.doSomething = function() {};
    
    // 错误用法！！!
      exports = {
        foo: 'bar',
        doSomething: function() {}
      };
    // 正确写法
      module.exports = {
        foo: 'bar',
        doSomething: function() {}
      };
/*
    exports 仅仅是 module.exports 的一个引用。在 factory 内部给 exports 重新赋值时，并不会改变 module.exports 的值。因此给 exports 赋值是无效的，不能用来更改模块接口。
*/
});
```

#### ES6 Moubule

**特点**

export指令导出接口，以import引入模块

import的语法和require不同，而且import必须放在文件的最开始，且前面不允许有其他逻辑代码，这和其他所有的编程语言风格一致

**使用**

```
export var m = 1;
// 等价于
var m = 1;
export { m }

export const student = {
  name: 'Megan',
  age: 18
}
// 等价于
const obj = {
  id: 1,
  value: 'lalala'
};
export { obj };

export function sun(a, b) {
  return a + b;
}
// 等价于
function sum(a, b){
  return a + b;
}
export { sum };
import { sum } from xxxx

export default function() {}
 
// 等效于：
function a() {};
export {a as default};

import  xxx  from xxxx //可以省去花括号{}。
// 等效于，或者说就是下面这种写法的简写，是同一个意思
import { default as xxx } from xxxx;
```

```
//一个文件即模块中只能存在一个export default语句，导出一个当前模块的默认对外接口
export default var i = 0;
//使用默认式
import variable from './exportDemo';
//同时使用命名式和默认式
import variable, { sum, boy } from './exportDemo';
```

```
//导入一个模块，但不进行任何绑定：
import "my-module";
```

```
//在同一个模块可以同时使用两种导出方式
export function sun(a, b) {
  return a + b;
}
export default {
  install,
  DottedTitle,
};
```

