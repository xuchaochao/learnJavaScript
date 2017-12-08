Web 浏览器中可能发生的事件有很多类型。如前所述，不同的事件类型具有不同的信息，而“ DOM3级事件”规定了以下几类事件。

* UI（User Interface，用户界面）事件，当用户与页面上的元素交互时触发；
* 焦点事件，当元素获得或失去焦点时触发；
* 鼠标事件，当用户通过鼠标在页面上执行操作时触发；
* 滚轮事件，当使用鼠标滚轮（或类似设备）时触发；
* 文本事件，当在文档中输入文本时触发；
* 键盘事件，当用户通过键盘在页面上执行操作时触发；
* 合成事件，当为 IME（Input Method Editor，输入法编辑器）输入字符时触发；
* 变动（mutation）事件，当底层 DOM 结构发生变化时触发。

#### UI事件

UI 事件指的是那些不一定与用户操作有关的事件。这些事件在 DOM 规范出现之前，都是以这种或那种形式存在的，而在 DOM 规范中保留是为了向后兼容。现有的 UI 事件如下。

* DOMActivate：表示元素已经被用户操作（通过鼠标或键盘）激活。这个事件在 DOM3 级事件中被废弃，但 Firefox 2+和 Chrome 支持它。考虑到不同浏览器实现的差异，不建议使用这个事件。
* load：当页面完全加载后在 window 上面触发，当所有框架都加载完毕时在框架集上面触发，当图像加载完毕时在`<img>`元素上面触发，或者当嵌入的内容加载完毕时在`<object>`元素上面触发。
* unload：当页面完全卸载后在 window 上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在`<object>`元素上面触发。
* abort：在用户停止下载过程时，如果嵌入的内容没有加载完，则在`<object>`元素上面触发。
* error：当发生 JavaScript 错误时在 window 上面触发，当无法加载图像时在`<img>`元素上面触发，当无法加载嵌入内容时在`<object>`元素上面触发，或者当有一或多个框架无法加载时在框架集上面触发。
* select：当用户选择文本框（`<input>`或`<texterea>`）中的一或多个字符时触发。
* resize：当窗口或框架的大小变化时在 window 或框架上面触发。
* scroll：当用户滚动带滚动条的元素中的内容时，在该元素上面触发。 `<body>`元素中包含所加载页面的滚动条。

1.load 事件  
JavaScript 中最常用的一个事件就是 load。当页面完全加载后（包括所有图像、 JavaScript 文件、CSS 文件等外部资源），就会触发 window 上面的 load 事件。有两种定义 onload 事件处理程序的方式。

```js
document.onload = function(event){
    alert("Loaded!");
};
```

```html
<!DOCTYPE html>
<html>
    <head>
        <title>Load Event Example</title>
    </head>
    <body onload="alert('Loaded!')">

    </body>
</html>
```

> 根据“ DOM2 级事件”规范，应该在 document 而非 window 上面触发 load 事件。但是，所有浏览器都在 window 上面实现了该事件，以确保向后兼容。

图像上面也可以触发 load 事件，无论是在 DOM 中的图像元素还是 HTML 中的图像元素。

```html
<img src="smile.gif" onload="alert('Image loaded.')">
```

2.unload事件  
与 load 事件对应的是 unload 事件，这个事件在文档被完全卸载后触发。只要用户从一个页面切换到另一个页面，就会发生 unload 事件。而利用这个事件最多的情况是清除引用，以避免内存泄漏。与 load 事件类似，也有两种指定 onunload 事件处理程序的方式。

3.resize 事件  
当浏览器窗口被调整到一个新的高度或宽度时，就会触发 resize 事件。这个事件在 window（窗口）上面触发，因此可以通过 JavaScript 或者`<body>`元素中的 onresize 特性来指定事件处理程序。

> 浏览器窗口最小化或最大化时也会触发 resize 事件。

4.scroll 事件  
虽然 scroll 事件是在 window 对象上发生的，但它实际表示的则是页面中相应元素的变化。在混杂模式下，可以通过`<body>`元素的 scrollLeft 和 scrollTop 来监控到这一变化；而在标准模式下，  
除 Safari 之外的所有浏览器都会通过`<html>`元素来反映这一变化（Safari 仍然基于`<body>`跟踪滚动位置）

> 与 resize 事件类似， scroll 事件也会在文档被滚动期间重复被触发，所以有必要尽量保持事件处理程序的代码简单。

#### 焦点事件

焦点事件会在页面元素获得或失去焦点时触发。利用这些事件并与 document.hasFocus\(\)方法及document.activeElement 属性配合，可以知晓用户在页面上的行踪。有以下 6 个焦点事件。

* blur：在元素失去焦点时触发。这个事件不会冒泡；所有浏览器都支持它。
* DOMFocusIn：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。只有Opera 支持这个事件。 DOM3 级事件废弃了 DOMFocusIn，选择了 focusin。
* DOMFocusOut：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。只有 Opera支持这个事件。 DOM3 级事件废弃了 DOMFocusOut，选择了 focusout。
* focus：在元素获得焦点时触发。这个事件不会冒泡；所有浏览器都支持它。
* focusin：在元素获得焦点时触发。这个事件与 HTML 事件 focus 等价，但它冒泡。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。
* focusout：在元素失去焦点时触发。这个事件是 HTML 事件 blur 的通用版本。支持这个事件的浏览器有 IE5.5+、 Safari 5.1+、 Opera 11.5+和 Chrome。

当焦点从页面中的一个元素移动到另一个元素，会依次触发下列事件：  
\(1\) focusout 在失去焦点的元素上触发；  
\(2\) focusin 在获得焦点的元素上触发；  
\(3\) blur 在失去焦点的元素上触发；  
\(4\) DOMFocusOut 在失去焦点的元素上触发；  
\(5\) focus 在获得焦点的元素上触发；  
\(6\) DOMFocusIn 在获得焦点的元素上触发。

要确定浏览器是否支持这些事件，可以使用如下代码：

```js
var isSupported = document.implementation.hasFeature("FocusEvent", "3.0");
```

### 鼠标与滚轮书事件

鼠标事件是 Web 开发中最常用的一类事件，毕竟鼠标还是最主要的定位设备。 DOM3 级事件中定义了 9 个鼠标事件，简介如下。

* click：在用户单击主鼠标按钮（一般是左边的按钮）或者按下回车键时触发。这一点对确保易访问性很重要，意味着 onclick 事件处理程序既可以通过键盘也可以通过鼠标执行。
* dblclick：在用户双击主鼠标按钮（一般是左边的按钮）时触发。从技术上说，这个事件并不是 DOM2 级事件规范中规定的，但鉴于它得到了广泛支持，所以 DOM3 级事件将其纳入了标准。
* mousedown：在用户按下了任意鼠标按钮时触发。不能通过键盘触发这个事件。
* mouseenter：在鼠标光标从元素外部首次移动到元素范围之内时触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发。 DOM2 级事件并没有定义这个事件，但 DOM3 级事件将它
  纳入了规范。 IE、 Firefox 9+和 Opera 支持这个事件。
* mouseleave：在位于元素上方的鼠标光标移动到元素范围之外时触发。这个事件不冒泡，而且在光标移动到后代元素上不会触发。 DOM2 级事件并没有定义这个事件，但 DOM3 级事件将它
  纳入了规范。 IE、 Firefox 9+和 Opera 支持这个事件。
* mousemove：当鼠标指针在元素内部移动时重复地触发。不能通过键盘触发这个事件。
* mouseout：在鼠标指针位于一个元素上方，然后用户将其移入另一个元素时触发。又移入的另一个元素可能位于前一个元素的外部，也可能是这个元素的子元素。不能通过键盘触发这个事件。
* mouseover：在鼠标指针位于一个元素外部，然后用户将其首次移入另一个元素边界之内时触发。不能通过键盘触发这个事件。
* mouseup：在用户释放鼠标按钮时触发。不能通过键盘触发这个事件。

页面上的所有元素都支持鼠标事件。除了 mouseenter 和 mouseleave，所有鼠标事件都会冒泡，也可以被取消，而取消鼠标事件将会影响浏览器的默认行为。  
事件触发的顺序始终如下：  
\(1\) mousedown  
\(2\) mouseup  
\(3\) click  
\(4\) mousedown  
\(5\) mouseup  
\(6\) click  
\(7\) dblclick

使用以下代码可以检测浏览器是否支持以上 DOM2 级事件（除 dbclick、 mouseenter 和 mouseleave 之外）：

```js
var isSupported = document.implementation.hasFeature("MouseEvents", "2.0");
```

要检测浏览器是否支持上面的所有事件，可以使用以下代码：

```js
var isSupported = document.implementation.hasFeature("MouseEvent", "3.0")
```

> 注意， DOM3 级事件的 feature 名是"MouseEvent"，而非"MouseEvents"。

1.客户区坐标位置  
鼠标事件都是在浏览器视口中的特定位置上发生的。这个位置信息保存在事件对象的 clientX 和 clientY 属性中。所有浏览器都支持这两个属性，它们的值表示事件发生时鼠标指针在视口中的水平和垂直坐标。  
![](/assets/深度截图_选择区域_20171208090303.png)

可以使用类似下列代码取得鼠标事件的客户端坐标信息：

```js
var div = document.getElementById("myDiv");
EventUtil.addHandler(div, "click", function(event){
    event = EventUtil.getEvent(event);
    alert("Client coordinates: " + event.clientX + "," + event.clientY);
});
```

2.页面坐标位置  
通过客户区坐标能够知道鼠标是在视口中什么位置发生的，而页面坐标通过事件对象的 pageX 和 pageY 属性，能告诉你事件是在页面中的什么位置发生的。

```js
var div = document.getElementById("myDiv");
EventUtil.addHandler(div, "click", function(event){
    event = EventUtil.getEvent(event);
    alert("Page coordinates: " + event.pageX + "," + event.pageY);
});
```

3.屏幕坐标位置  
鼠标事件发生时，不仅会有相对于浏览器窗口的位置，还有一个相对于整个电脑屏幕的位置。而通过 screenX 和 screenY 属性就可以确定鼠标事件发生时鼠标指针相对于整个屏幕的坐标信息。  
![](/assets/深度截图_选择区域_20171208090626.png)

```js
var div = document.getElementById("myDiv");
EventUtil.addHandler(div, "click", function(event){
    event = EventUtil.getEvent(event);
    alert("Screen coordinates: " + event.screenX + "," + event.screenY);
});
```
4.鼠标事件
DOM 的 button 属性可能有如下 3 个值： 0 表示主鼠标按钮， 1 表示中间的鼠标按钮（鼠标滚轮按钮） ， 2 表示次鼠标按钮。在常规的设置中，主鼠标按钮就是鼠标左键，而次鼠标按钮就是鼠标右键。

5.鼠标滚轮事件
IE 6.0 首先实现了 mousewheel 事件。此后， Opera、 Chrome 和 Safari 也都实现了这个事件。当用户通过鼠标滚轮与页面交互、在垂直方向上滚动页面时（无论向上还是向下），就会触发 mousewheel
事件。这个事件可以在任何元素上面触发，最终会冒泡到 document（IE8）或 window（IE9、 Opera、Chrome 及 Safari）对象。
```js
EventUtil.addHandler(document, "mousewheel", function(event){
    event = EventUtil.getEvent(event);
    alert(event.wheelDelta);
});
```
与 mousewheel 事件对应的 event 对象除包含鼠标事件的所有标准信息外，还包含一个特殊的 wheelDelta 属性。当用户向前滚动鼠标滚轮时， wheelDelta 是 120 的倍数；当用户向后滚动鼠标滚轮时， wheelDelta 是 `-120` 的倍数。

6.触摸设备
iOS 和 Android 设备的实现非常特别，因为这些设备没有鼠标。在面向 iPhone 和 iPod 中的 Safari 开发时，要记住以下几点。
- 不支持 dblclick 事件。双击浏览器窗口会放大画面，而且没有办法改变该行为。
- 轻击可单击元素会触发 mousemove 事件。如果此操作会导致内容变化，将不再有其他事件发生；如果屏幕没有因此变化，那么会依次发生 mousedown、 mouseup 和 click 事件。轻击不可单击的元素不会触发任何事件。可单击的元素是指那些单击可产生默认操作的元素（如链接） ，或者那些已经被指定了 onclick 事件处理程序的元素。
- mousemove 事件也会触发 mouseover 和 mouseout 事件。
- 两个手指放在屏幕上且页面随手指移动而滚动时会触发 mousewheel 和 scroll 事件。

#### 键盘与文本事件