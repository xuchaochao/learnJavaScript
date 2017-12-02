其实ECMAScript的函数也是对象，每个函数都是Function类型的实例。  
函数的定义：

```js
function sum(num1, num2){
    return num1 + num2
}

var sum = function(num1, num2){
    return num1 + num2
}

var sum = new Function("num1", "num2", "return num1 + num2") // 不推荐这种方法，因为会解析两次语法，影响性能
```

#### 没有重载（深入理解）

因为函数名实际上是指针，所以第二个同名的函数会覆盖掉第一个的函数，即JavaScript中没有重载。

```js
function addSomeNumber(num){
    return num + 100;
}
function addSomeNumber(num) {
    return num + 200;
}
var result = addSomeNumber(100); //300
```

#### 函数声明与函数表达式

JavaScript解析器会率先读取函数声明，并使其在执行任何代码之前可用（可以访问）；至于函数表达式，则必须等到解析器执行到它所在的代码行，才会真正被解释执行。所以会出现以下代码的区别:

```js
// 这样的代码执行成功，因为函数的声明会被在开始时执行。
alert(sum(10,10));
function sum(num1, num2){
    return num1 + num2;
}

// 会出现致命的错误
alert(sum(10,10));
var sum = function(num1, num2){
    return num1 + num2;
};
```

#### 作为值的函数

因为ECMAScript中的函数名本身就是变量，所以函数也可以作为值来使用。

```js
function callSomeFunction(someFunction, someArgument){
    return someFunction(someArgument);
}

function add10(num){
    return num + 10;
}

var result1 = callSomeFunction(add10, 10);
alert(result1); //20
function getGreeting(name){
    return "Hello, " + name;
}
var result2 = callSomeFunction(getGreeting, "Nicholas");
alert(result2); //"Hello, Nicholas"
```

#### 函数内部属性

函数内部有两个特殊的对象：arguments和this。  
首先是arguments的主要用途是保存函数参数，但还有一个_callee_的属性，该属性是一个指针，指向拥有这个arguments对象的函数。

```js
// 一个简单的阶乘函数
function factorial(num){
    if (num <=1) {
        return 1;
    } else {
        return num * factorial(num-1)
    }
}

// 使用arguments的callee属性来重写该函数
function factorial(num){
    if(num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num-1)
    }
}
```

还有一个就是this。this引用的是函数执行的环境对象。简单的讲就是谁引用了该函数，this就指向谁。

```js
window.color = "red";
var o = { color: "blue" };
function sayColor(){
    alert(this.color);
}
sayColor(); //"red"
o.sayColor = sayColor;
o.sayColor(); //"blue"
```

而在ECMAScript5中又规范化另一个函数对象的属性_caller_。这个属性中保存着调用当前函数的函数的引用，如果是在全局作用域中调用当前函数，它的值为 null。

```js
function outer(){
    inner();
}
function inner(){
    alert(inner.caller);
}
outer();

// 为了实现更松散的耦合，也可以通过arguments.callee.caller实现
function outer(){
    inner();
}
function inner(){
    alert(arguments.callee.caller);
}
outer();
```

#### 函数属性和方法

每个函数都包含两个属性：length和prototype。  
length属性表示函数接受的命名参数的个数。

```js
function sayName(name){
    alert(name);
}
function sum(num1, num2){
    return num1 + num2;
}
function sayHi(){
    alert("hi");
}
alert(sayName.length); //1
alert(sum.length); //2
alert(sayHi.length); //0
```

对于ECMAScript 中的引用类型而言， prototype 是保存它们所有实例方法的真正所在。换句话说，诸如toString\(\)和 valueOf\(\)等方法实际上都保存在 prototype 名下，只不过是通过各自对象的实例访问罢了。在创建自定义引用类型以及实现继承时， prototype 属性的作用是极为重要的。  
每个函数都包含两个非继承而来的方法： apply\(\)和 call\(\)。这两个方法的用途都是在特定的作用域中调用函数，实际上等于设置函数体内 this 对象的值。首先， apply\(\)方法接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组。其中，第二个参数可以是 Array 的实例，也可以是arguments 对象。例如：

```js
function sum(num1, num2){
    return num1 + num2;
}
function callSum1(num1, num2){
    return sum.apply(this, arguments); // 传入 arguments 对象
}
function callSum2(num1, num2){
    return sum.apply(this, [num1, num2]); // 传入数组
}
alert(callSum1(10,10)); //20
alert(callSum2(10,10)); //20
```

> 在严格模式下，未指定环境对象而调用函数，则 this 值不会转型为 window。除非明确把函数添加到某个对象或者调用 apply\(\)或 call\(\)，否则 this 值将是undefined。

call\(\)方法与 apply\(\)方法的作用相同，它们的区别仅在于接收参数的方式不同。在使用call\(\)方法时，传递给函数的参数必须逐个列举出来:

```js
function sum(num1, num2){
    return num1 + num2;
}
function callSum(num1, num2){
    return sum.call(this, num1, num2);
}
alert(callSum(10,10)); //20
```
事实上，传递参数并非 apply()和 call()真正的用武之地；它们真正强大的地方是能够扩充函数赖以运行的作用域:
```js
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    alert(this.color)
}

sayColor(); //red

sayColor.call(this); //red
sayColor.call(window); //red
sayColor.call(o) //blue
```
ECMAScript 5 还定义了一个方法： bind()。这个方法会创建一个函数的实例，其 this 值会被绑定到传给 bind()函数的值。
```js
window.color = "red";
var o = { color: "blue" };

function sayColor(){
    alert(this.color);
}
var objectSayColor = sayColor.bind(o);
objectSayColor(); //blue
```




