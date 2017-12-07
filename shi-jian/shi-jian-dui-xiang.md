在触发 DOM 上的某个事件时，会产生一个事件对象 event，这个对象中包含着所有与事件有关的信息。包括导致事件的元素、事件的类型以及其他与特定事件相关的信息。

#### DOM中的事件对象
兼容 DOM 的浏览器会将一个 event 对象传入到事件处理程序中。无论指定事件处理程序时使用什么方法（DOM0 级或 DOM2 级） ，都会传入 event 对象。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert(event.type); //"click"
};
btn.addEventListener("click", function(event){
    alert(event.type); //"click"
}, false);
```
event 对象包含与创建它的特定事件有关的属性和方法。触发的事件类型不一样，可用的属性和方法也不一样。不过，所有事件都会有下表列出的成员。
![](/assets/深度截图_选择区域_20171207224851.png)
![](/assets/深度截图_选择区域_20171207224919.png)

在事件处理程序内部，对象 this 始终等于 currentTarget 的值，而 target 则只包含事件的实际目标。如果直接将事件处理程序指定给了目标元素，则 this、 currentTarget 和 target 包含相同的值。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert(event.currentTarget === this); //true
    alert(event.target === this); //true
};
```
要阻止特定事件的默认行为，可以使用 preventDefault()方法。例如，链接的默认行为就是在被单击时会导航到其 href 特性指定的 URL。如果你想阻止链接导航这一默认行为，那么通过链接的
onclick 事件处理程序可以取消它.
```js
var link = document.getElementById("myLink");
link.onclick = function(event){
    event.preventDefault();
};
```
只有 cancelable 属性设置为 true 的事件，才可以使用 preventDefault()来取消其默认行为。
stopPropagation()方法用于立即停止事件在 DOM 层次中的传播，即取消进一步的事件捕获或冒泡。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert("Clicked");
    event.stopPropagation();
};
document.body.onclick = function(event){
    alert("Body clicked");
};
```
对于这个例子而言，如果不调用 stopPropagation()，就会在单击按钮时出现两个警告框。
事件对象的 eventPhase 属性，可以用来确定事件当前正位于事件流的哪个阶段。如果是在捕获阶段调用的事件处理程序，那么 eventPhase 等于 1；如果事件处理程序处于目标对象上，则 eventPhase 等于 2；如果是在冒泡阶段调用的事件处理程序， eventPhase 等于 3。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(event){
    alert(event.eventPhase); //2
};
document.body.addEventListener("click", function(event){
    alert(event.eventPhase); //1
}, true);
document.body.onclick = function(event){
    alert(event.eventPhase); //3
};
```
> 只有在事件处理程序执行期间， event 对象才会存在；一旦事件处理程序执行完成， event 对象就会被销毁。

#### IE中的事件对象
与访问 DOM 中的 event 对象不同，要访问 IE 中的 event 对象有几种不同的方式，取决于指定事件处理程序的方法。在使用 DOM0 级方法添加事件处理程序时， event 对象作为 window 对象的一个属性存在。
```js
var btn = document.getElementById("myBtn");
btn.onclick = function(){
var event = window.event;
    alert(event.type); //"click"
};
```
IE 的 event 对象同样也包含与创建它的事件相关的属性和方法。其中很多属性和方法都有对应的或者相关的 DOM 属性和方法。与 DOM 的 event 对象一样，这些属性和方法也会因为事件类型的不同而不同，但所有事件对象都会包含下表所列的属性和方法。
![](/assets/深度截图_选择区域_20171207230547.png)