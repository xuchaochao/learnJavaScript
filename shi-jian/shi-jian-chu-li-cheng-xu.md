事件就是用户或浏览器自身执行的某种动作。诸如 click、 load 和 mouseover，都是事件的名字。而响应某个事件的函数就叫做事件处理程序（或事件侦听器）。事件处理程序的名字以"on"开头，因此click 事件的事件处理程序就是 onclick， load 事件的事件处理程序就是 onload。为事件指定处理程序的方式有好几种。

#### HTML事件处理程序

某个元素支持的每种事件，都可以使用一个与相应事件处理程序同名的 HTML 特性来指定。这个特性的值应该是能够执行的 JavaScript 代码。

```html
<input type="button" value="Click Me" onclick="alert('Clicked')" />
```

#### DOM0 级事件处理程序

通过 JavaScript 指定事件处理程序的传统方式，就是将一个函数赋值给一个事件处理程序属性。

```js
var btn = document.getElementById("myBtn");
btn.onclick = function(){
    alert("Clicked");
};
btn.onclick = null; //删除事件处理程序
```

#### DOM2 级事件处理程序

“ DOM2 级事件” 定义了两个方法，用于处理指定和删除事件处理程序的操作： addEventListener\(\)和 removeEventListener\(\)。所有 DOM 节点中都包含这两个方法，并且它们都接受 3 个参数：要处理的事件名、作为事件处理程序的函数和一个布尔值。最后这个布尔值参数如果是 true，表示在捕获阶段调用事件处理程序；如果是 false，表示在冒泡阶段调用事件处理程序。

```js
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function(){
    alert(this.id);
}, false);
```

使用 DOM2 级方法添加事件处理程序的主要好处是可以添加多个事件处理程序。

```js
var btn = document.getElementById("myBtn");
btn.addEventListener("click", function(){
    alert(this.id);
}, false);
btn.addEventListener("click", function(){
    alert("Hello world!");
}, false);
// 这两个事件处理程序会按照添加它们的顺序触发
```

通过 addEventListener\(\)添加的事件处理程序只能使用 removeEventListener\(\)来移除；移除时传入的参数与添加处理程序时使用的参数相同。

```js
var btn = document.getElementById("myBtn");
var handler = function(){
    alert(this.id);
};
btn.addEventListener("click", handler, false);
//这里省略了其他代码
btn.removeEventListener("click", handler, false); //有效！
```

#### IE事件处理程序

IE 实现了与 DOM 中类似的两个方法： attachEvent\(\)和 detachEvent\(\)。这两个方法接受相同的两个参数：事件处理程序名称与事件处理程序函数。

```js
// 通过attachEvent()添加的事件处理程序都会被添加到冒泡阶段。
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
    alert("Clicked");
});
```

在 IE 中使用 attachEvent\(\)与使用 DOM0 级方法的主要区别在于事件处理程序的作用域。在使用 DOM0 级方法的情况下，事件处理程序会在其所属元素的作用域内运行；在使用 attachEvent\(\)方法的情况下，事件处理程序会在全局作用域中运行，因此 this 等于 window。

```js
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
    alert(this === window); //true
});
// 在编写跨浏览器的代码时，牢记这一区别非常重要。
```

与 addEventListener\(\)类似， attachEvent\(\)方法也可以用来为一个元素添加多个事件处理程序。

```js
var btn = document.getElementById("myBtn");
btn.attachEvent("onclick", function(){
    alert("Clicked");
});
btn.attachEvent("onclick", function(){
    alert("Hello world!");
});
// 以相反的顺序被触发
```

使用 attachEvent\(\)添加的事件可以通过 detachEvent\(\)来移除，条件是必须提供相同的参数。

```js
var btn = document.getElementById("myBtn");
var handler = function(){
    alert("Clicked");
};
btn.attachEvent("onclick", handler);
//这里省略了其他代码
btn.detachEvent("onclick", handler);
```



