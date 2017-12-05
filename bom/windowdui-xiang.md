BOM 的核心对象是 window，它表示浏览器的一个实例。在浏览器中， window 对象有双重角色，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象。这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象，因此有权访问parseInt()等方法。

#### 全局作用域
由于 window 对象同时扮演着 ECMAScript 中 Global 对象的角色，因此所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法。
```js
var age =29;
function sayAge(){
    alert(this.age);
}
alert(window.age)    // 29
sayAge();    // 29
window.sayAge();    // 29
```
抛开全局变量会成为 window 对象的属性不谈，定义全局变量与在 window 对象上直接定义属性还是有一点差别：全局变量不能通过 delete 操作符删除，而直接在 window 对象上的定义的属性可以。
```js
var age = 29;
window.color = "red";
//在 IE < 9 时抛出错误，在其他所有浏览器中都返回 false
delete window.age;
//在 IE < 9 时抛出错误，在其他所有浏览器中都返回 true
delete window.color; //returns true
alert(window.age); //29
alert(window.color); //undefined
```
> 尝试访问未声明的变量会抛出错误，但是通过查询 window 对象，可以知道某个可能未声明的变量是否存在。

#### 窗口关系及框架
如果页面中包含框架，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中。
top 对象始终指向最高（最外）层的框架，也就是浏览器窗口。
parent（父）对象始终指向当前框架的直接上层框架。
self 对象，它始终指向 window；

#### 窗口位置
IE、 Safari、 Opera 和 Chrome 都提供了screenLeft 和 screenTop 属性，分别用于表示窗口相对于屏幕左边和上边的位置。 Firefox 则在 screenX 和 screenY 属性中提供相同的窗口位置信息， Safari 和 Chrome 也同时支持这两个属性。 Opera 虽然也支持 screenX 和 screenY 属性，但与 screenLeft 和 screenTop 属性并不对应，因此建议不要在 Opera 中使用它们。使用下列代码可以跨浏览器取得窗口左边和上边的位置。
```js
var leftPos = (typeof window.screenLeft == "number") ?
            window.screenLeft : window.screenX;
var topPos = (typeof window.screenTop == "number") ?
            window.screenTop : window.screenY;
```

#### 窗口大小
IE9+、 Firefox、 Safari、 Opera 和 Chrome 均为此提
供了 4 个属性： innerWidth、 innerHeight、outerWidth 和 outerHeight。在 IE9+、 Safari 和 Firefox 中， outerWidth 和 outerHeight 返回浏览器窗口本身的尺寸（无论是从最外层的 window 对象还是从某个框架访问） 。在 Opera 中，这两个属性的值表示页面视图容器①的大小。而 innerWidth 和 innerHeight 则表示该容器中页面视图区的大小（减去边框宽度） 。在 Chrome 中， outerWidth、 outerHeight 与
innerWidth、 innerHeight 返回相同的值，即视口（viewport）大小而非浏览器窗口大小。
虽然最终无法确定浏览器窗口本身的大小，但却可以取得页面视口的大小。
```js
var pageWidth = window.innerWidth,
    pageHeight = window.innerHeight;
if (typeof pageWidth != "number"){
    if (document.compatMode == "CSS1Compat"){
        pageWidth = document.documentElement.clientWidth;
        pageHeight = document.documentElement.clientHeight;
    } else {
        pageWidth = document.body.clientWidth;
        pageHeight = document.body.clientHeight;
    }
}
```
使用 resizeTo()和 resizeBy()方法可以调整浏览器窗口的大小。这两个方法都接收两个参数，其中 resizeTo()接收浏览器窗口的新宽度和新高度，而 resizeBy()接收新窗口与原窗口的宽度和高度之差。
```js
//调整到 100× 100
window.resizeTo(100, 100);
//调整到 200× 150
window.resizeBy(100, 50);
//调整到 300× 300
window.resizeTo(300, 300);
```

#### 导航和打开窗口
使用 window.open()方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。这个方法可以接收 4 个参数：要加载的 URL、窗口目标、一个特性字符串以及一个表示新页面是否取代浏览
器历史记录中当前加载页面的布尔值。
```js
//等同于< a href="http://www.wrox.com" target="topFrame"></a>
window.open("http://www.wrox.com/", "topFrame");
```
第三个参数是一个逗号分隔的设置字符串，表示在新窗口中都显示哪些特性。
![](/assets/深度截图_选择区域_20171205085102.png)
```js
window.open("http://www.wrox.com/","wroxWindow",
    "height=400,width=400,top=10,left=10,resizable=yes");
```
```js
var wroxWin = window.open("http://www.wrox.com/","wroxWindow",
"height=400,width=400,top=10,left=10,resizable=yes");
//调整大小
wroxWin.resizeTo(500,500);
//移动位置
wroxWin.moveTo(100,100);
// 关闭新打开的窗口。
wroxWin.close();
```
> 这个方法仅适用于通过 window.open()打开的弹出窗口。对于浏览器的主窗口，如果没有得到用户的允许是不能关闭它的。

#### 间接调用和超时调用
JavaScript 是单线程语言，但它允许通过设置超时值和间歇时间值来调度代码在特定的时刻执行。前者是在指定的时间过后执行代码，而后者则是每隔指定的时间就执行一次代码。
超时调用需要使用 window 对象的 setTimeout()方法，它接受两个参数：要执行的代码和以毫秒表示的时间（即在执行代码前需要等待多少毫秒）。
```js
//不建议传递字符串！
setTimeout("alert('Hello world!') ", 1000);
//推荐的调用方式
setTimeout(function() {
    alert("Hello world!");
}, 1000);
```
JavaScript 是一个单线程序的解释器，因此一定时间内只能执行一段代码。为了控制要执行的代码，就有一个 JavaScript 任务队列。这些任务会按照将它们添加到队列的顺序执行。setTimeout()的第二个参数告诉 JavaScript 再过多长时间把当前任务添加到队列中。如果队列是空的，那么添加的代码会立即执行；如果队列不是空的，那么它就要等前面的代码执行完了以后再执行。
调用 setTimeout()之后，该方法会返回一个数值 ID，表示超时调用。这个超时调用 ID 是计划执行代码的唯一标识符，可以通过它来取消超时调用。要取消尚未执行的超时调用计划，可以调用
clearTimeout()方法并将相应的超时调用 ID 作为参数传递给它。
```js
//设置超时调用
var timeoutId = setTimeout(function() {
    alert("Hello world!");
}, 1000);
//注意：把它取消
clearTimeout(timeoutId);
```
间歇调用与超时调用类似，只不过它会按照指定的时间间隔重复执行代码，直至间歇调用被取消或者页面被卸载。设置间歇调用的方法是 setInterval()，它接受的参数与 setTimeout()相同。
```js
//不建议传递字符串！
setInterval ("alert('Hello world!') ", 10000);
//推荐的调用方式
setInterval (function() {
    alert("Hello world!");
}, 10000);
```
同时也支持取消操作。
```js
var num = 0;
var max = 10;
var intervalId = null;
function incrementNumber() {
    num++;
    //如果执行次数达到了 max 设定的值，则取消后续尚未执行的调用
    if (num == max) {
        clearInterval(intervalId);
        alert("Done");
    }
}
intervalId = setInterval(incrementNumber, 500);
```
#### 系统对话框
浏览器通过 alert()、 confirm()和 prompt()方法可以调用系统对话框向用户显示消息。
调用 alert()方法的结果就是向用户显示一个系统对话框，其中包含指定的文本和一个 OK（“确定”）按钮。
```js
alert("Hello world!")
```
第二种对话框是调用 confirm()方法生成的。从向用户显示消息的方面来看，这种“确认”对话 框很像是一个“警告”对话框。但二者的主要区别在于“确认”对话框除了显示 OK 按钮外，还会显示一个 Cancel（“取消”）按钮，两个按钮可以让用户决定是否执行给定的操作。
```js
if (confirm("Are you sure?")) {
    alert("I'm so glad you're sure! ");
} else {
    alert("I'm sorry to hear you're not sure. ");
}
```
最后一种对话框是通过调用 prompt()方法生成的，这是一个“提示”框，用于提示用户输入一些文本。提示框中除了显示 OK 和 Cancel 按钮之外，还会显示一个文本输入域，以供用户在其中输入内容。prompt()方法接受两个参数：要显示给用户的文本提示和文本输入域的默认值（可以是一个空字符串）。
```js
var result = prompt("What is your name? ", "");
if (result !== null) {
    alert("Welcome, " + result);
}
```