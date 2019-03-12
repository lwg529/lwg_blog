---
title: js数据类型判断和转换
date: 2019-03-11 #文章生成时间
categories: "javascript" #文章分类目录 可以省略
tags: #文章标签 可以省略
     - js
     - 数据类型
#description: js数据类型判断和转换
---
## 前言

无论笔试还是面试，总会问到数据类型和隐式转换。今天彻底整理一下这块的知识，希望对大家有帮助。


看到下面的题，是不是已经蒙了，读完这篇文章，就能顺利通关了
```
console.log([] == 0) //true 

console.log(![] == 0)  //true 

console.log([] == ![]) //true

console.log([] == []) //false

console.log({} == {}) //false

console.log({} == !{}) //false

console.log([] == false) //true

console.log({} == false) //false

if([]) {console.log(3)} //3 

if([1] == [1]){console.log(4)} //没有输出

console.log('2' > 10) //false

console.log('2' > '10') //true

```


## 数据类型判断
### 数据类型
js数据类型一共有7种，undefined、 null、 boolean 、string、 number、 object、 Symbol

### 类型判断

#### typeof

```
typeof undefined //undefined
typeof true //boolean
typeof 42 //number
typeof '42' //string
typeof { life: 42 } //object
let s = Symbol();
typeof s //symbol

//特殊情况
typeof [1,2,3,4]// object
typeof null //object
typeof new Date()  //object
typeof function () {}  //function
```
**由此可以看出，typeof不能区分数组， null和对象**

#### Object.prototype.toString.call


```
let getType=Object.prototype.toString;

getType.call(undefined) //[object Undefined]

console.log(getType.call(true)) //[object Boolean]

console.log(getType.call(42)) //[object Number]

console.log(getType.call(Symbol()))//[object Symbol]

console.log(getType.call([1,2,3,4])) //[object Array]

console.log(getType.call(null))//[object Null]

console.log(getType.call(new Date())) //[object Date]

console.log(getType.call(function () {} )) //[object Function]
```



#### instanceof
instanceof运算符返回一个布尔值，表示对象是否为某个构造函数的实例。

instanceof运算符的左边是**实例对象**，右边是构造函数。它会检查右边构建函数的原型对象（prototype），是否在左边对象的原型链上。



```
new Date instanceof Date //true
[1,2,3] instanceof Array //true
```

**instanceof运算符只能用于对象，不适用原始类型的值。**

#### constructor 属性

prototype对象有一个constructor属性，默认指向prototype对象所在的构造函数。

```
function P() {}
P.prototype.constructor === P // true
```


```
[1,2].constructor === Array //true
'123'.constructor === String //true
```

## 面试常问

### 怎么判断是不是数组

instanceof、constructor、Object.prototype.toString.call、Array.isArray


```
[1,2] instanceof Array //true
[1,2].constructor === Array //true
Object.prototype.toString.call([1,2]) === '[object Array]' //true
Array.isArray([1,2]) //true
```

### 如何判断一个对象是不是空对象

转换成json字符串判断
```
JSON.stringify({}) == "{}"
```
for in 循环判断


```
let isEmptyObject = function(obj) {
    for (let key in obj) {
        return false;
    }
    return true;
}
console.log(isEmptyObject(obj));//true
```

使用ES6的Object.keys()

```
Object.keys({}).length === 0
```


### 类似的数组转化成数组

类数组和数组都可以读写，获取长度，遍历，但是类数组不能调用数组的方法，比如push等

```
Array.prototype.slice.call(arguments)
```

或者`Array.from(arguments)`

### 字符串翻转

'abc'.split('').reverse().join('')


### 字符串和数组转换

`['a', 'b', 'c'].join('')  //'abc'`
```
'abc'.split('') //['a', 'b', 'c']
```

## 类型转换


### 显示类型转换

#### 转成数字，Number()、parseInt()、parseFloat()


```
// 数值：转换后还是原来的值
Number(324) // 324

// 字符串：如果可以被解析为数值，则转换为相应的数值
Number('324') // 324

// 字符串：如果不可以被解析为数值，返回 NaN
Number('324abc') // NaN

// 空字符串转为0
Number('') // 0

// 布尔值：true 转成 1，false 转成 0
Number(true) // 1
Number(false) // 0

// undefined：转成 NaN
Number(undefined) // NaN

// null：转成0
Number(null) // 0

//对象转化
Number({a: 1}) // NaN
Number({}) //NaN 

//数组
Number([1, 2, 3]) // NaN
Number([5]) // 5
Number([]) //0
```

**Number方法参数是对象时转换规则**

第一步，调用对象自身的valueOf方法。如果返回原始类型的值，则直接对该值使用Number函数，不再进行后续步骤。

第二步，如果valueOf方法返回的还是对象，则改为调用对象自身的toString方法。如果toString方法返回原始类型的值，则对该值使用Number函数，不再进行后续步骤。

第三步，如果toString方法返回的是对象，就报错。

转换规则示例：

```
var obj = {x: 1};
Number(obj) // NaN

// 等同于
if (typeof obj.valueOf() === 'object') {
  Number(obj.toString());
} else {
  Number(obj.valueOf());
}
```
先使用valueOf返回了对象本身，再代用toString()返回了"[object Object]"

**注意：**
任何涉及NaN的操作都返回NaN，NaN和任何值不相等

#### Boolean

除了以下值的转换结果为false，其他的值全部为true。

`false, '', 0, NaN, null, undefined`


#### String函数可以将任意类型的值转化成字符串

（1）原始类型值

```
//数值：转为相应的字符串。
String(123) // "123"

//字符串：转换后还是原来的值。
String('abc') // "abc"

//布尔值：true转为字符串"true"，false转为字符串"false"。
String(true) // "true"

//undefined：转为字符串"undefined"。
String(undefined) // "undefined"


//null：转为字符串"null"。
String(null) // "null"
```
（2）对象


```
//String方法的参数如果是对象，返回一个类型字符串
String({a: 1}) // "[object Object]"


//如果是数组，返回该数组的字符串形式。
String([1, 2, 3]) // "1,2,3"
```

**String方法背后的转换规则，与Number方法基本相同，只是互换了valueOf方法和toString方法的执行顺序。**


### 隐式类型转换

#### 自动转化为布尔值

if条件，while,

#### 自动转换为字符串

主要发生在字符串加法，一个值为字符串，另一个非字符串，则后者直接转为字符串


```
'5' + 1 // '51'
'5' + true // "5true"
'5' + {} // "5[object Object]"
```

#### 自动转化为数值

除了加法运算符（+）有可能把运算子转为字符串，其他运算符都会把运算子自动转成数值。


```
++/--(自增自减运算符) 

+ - * / %(算术运算符) 

+ > < >= <= == != === !=== (关系运算符)

```


```
'5' - '2' // 3
'5' * '2' // 10
true - 1  // 0
false / '5' // 0

//'abc'转为数值为NaN,NaN任何运算都是NaN
'abc' - 1   // NaN

//null进行Number运算转成0
null + 1 // 1

//undefined转为数值时为NaN
undefined + 1 // NaN
```

#### == 运算符

（1）原始类型的数据会转换成数值类型再进行比较。


```
1 == true // true
// 等同于 1 === Number(true)

0 == false // true
// 等同于 0 === Number(false)

2 == true // false
// 等同于 2 === Number(true)


'true' == true // false
// 等同于 Number('true') === Number(true)
// 等同于 NaN === 1

'' == 0 // true
// 等同于 Number('') === 0
// 等同于 0 === 0

'' == false  // true
// 等同于 Number('') === Number(false)
// 等同于 0 === 0

'1' == true  // true
// 等同于 Number('1') === Number(true)
// 等同于 1 === 1

'\n  123  \t' == 123 // true
// 因为字符串转为数字时，省略前置和后置的空格
```
（2）对象与原始类型值比较

对象（这里指广义的对象，包括数组和函数）与原始类型的值比较时，对象转化成原始类型的值，再进行比较。



```
[1] == 1 // true
// 等同于 Number([1]) == 1

[1] == '1' // true
// 等同于 Number([1]) == Number('1')

[1] == true // true
// 等同于 Number([1]) == Number(true)
```

## 实战练习


```
//true,空数组valueOf还是空数组，toString()转成得到空字符串，空字符串调用Number转成0
console.log([] == 0) //true 

//true，非的运算级别高，空数组转为布尔值为true,所以![]得到的false，Number转换为0， 最后结果还是true
console.log(![] == 0)  //true 

//true,前面是调用valueOf()后调用toString()转成false，后边是非转成false
console.log([] == ![]) 


//false，2个数组放在堆里面，栈中存储的是地址
console.log([] == []) 

//引用类型存储在堆中，栈中的是地址，所以是false
console.log({} == {}) 

//{}.valueOf().toString()得到的是[object, Object], !{}得到的是false，Number转换后不相等
console.log({} == !{})

//数组的valueOf().toString()后为空，所以是真
console.log([] == false) //true

//因为对象调用valueOf后为{}, toString后转为[object, Object]，Number后是NaN,
//任何涉及NaN的操作都返回NaN，NaN和任何值不相等
console.log({} == false) //false

//空数组的boolean值为true
if([]) {console.log(3)} //3 

//2个数组的栈地址不同
if([1] == [1]){console.log(4)} //没有输出

//false,转成2>10
console.log('2' > 10) 

//都是字符串，按照字符串的unicode转换，'2'.charCodeAt() > '10'.charCodeAt = 50 > 49
console.log('2' > '10') 

//都是字符串，按照字符串的unicode转换
console.log('abc' > 'b') //false
```

参考：http://javascript.ruanyifeng.com/grammar/conversion.html

