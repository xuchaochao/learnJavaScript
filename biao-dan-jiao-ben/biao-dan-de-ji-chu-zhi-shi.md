在 HTML 中，表单是由&lt;form&gt;元素来表示的，而在 JavaScript 中，表单对应的则是 HTMLFormElement 类型。 HTMLFormElement 继承了 HTMLElement，因而与其他 HTML 元素具有相同的默认属性。不过， HTMLFormElement 也有它自己下列独有的属性和方法。
- acceptCharset：服务器能够处理的字符集；等价于 HTML 中的 accept-charset 特性。
- action：接受请求的 URL；等价于 HTML 中的 action 特性。
- elements：表单中所有控件的集合（HTMLCollection）。
- enctype：请求的编码类型；等价于 HTML 中的 enctype 特性。
- length：表单中控件的数量。
- method：要发送的 HTTP 请求类型，通常是"get"或"post"；等价于 HTML 的 method 特性。
- name：表单的名称；等价于 HTML 的 name 特性。
- reset\(\)：将所有表单域重置为默认值。
- submit\(\)：提交表单。
- target：用于发送请求和接收响应的窗口名称；等价于 HTML 的 target 特性。

取得`<form>`元素引用的方式有好几种。
1.使用 getElementById()
```js
var form = document.getElementById("form1");
```

2.document.forms 可以取得页面中所有的表单。
```js
var firstForm = document.forms[0]; //取得页面中的第一个表单
var myForm = document.forms["form2"]; //取得页面中名称为"form2"的表单
```

#### 提交表单
用户单击提交按钮或图像按钮时，就会提交表单。
```html
<!-- 通用提交按钮 -->
<input type="submit" value="Submit Form">
<!-- 自定义提交按钮 -->
<button type="submit">Submit Form</button>
<!-- 图像按钮 -->
<input type="image" src="graphic.gif">
```

以这种方式提交表单时，浏览器会在将请求发送给服务器之前触发 submit 事件。调用 prevetnDefault()方法阻止了表单提交。一般来说，在表单数据无效而不能发送给服务器时，可以使用这一技术。在 JavaScript 中，以编程方式调用 submit()方法也可以提交表单。而且，这种方式无需表单包含提交按钮，任何时候都可以正常提交表单。
```js
var form = document.getElementById("myForm");
//提交表单
form.submit();
```
提交表单时可能出现的最大问题，就是重复提交表单。解决这一问题的办法有两个：在第一次提交表单后就禁用提交按钮，或者利用 onsubmit 事件处理程序取消后续的表单提交操作。

#### 重置表单
在用户单击重置按钮时，表单会被重置。
```html
<!-- 通用重置按钮 -->
<input type="reset" value="Reset Form">
<!-- 自定义重置按钮 -->
<button type="reset">Reset Form</button>
```
用户单击重置按钮重置表单时，会触发 reset 事件。利用这个机会，我们可以在必要时取消重置操作。
```js
var form = document.getElementById("myForm");
EventUtil.addHandler(form, "reset", function(event){
    //取得事件对象
    event = EventUtil.getEvent(event);
    //阻止表单重置
    EventUtil.preventDefault(event);
});
```
```js
与提交表单一样，也可以通过 JavaScript 来重置表单，如下面的例子所示。
var form = document.getElementById("myForm");
//重置表单
form.reset();
```

#### 表单字段
可以像访问页面中的其他元素一样，使用原生 DOM 方法访问表单元素。此外，每个表单都有elements 属性，该属性是表单中所有表单元素（字段）的集合。这个 elements 集合是一个有序列表，其中包含着表单中的所有字段，例如`<input>`、 `<textarea>`、 `<button>`和`<fieldset>`。
```js
var form = document.getElementById("form1");
//取得表单中的第一个字段
var field1 = form.elements[0];
//取得名为"textbox1"的字段
var field2 = form.elements["textbox1"];
//取得表单中包含的字段的数量
var fieldCount = form.elements.length;
```
1.共有的表单字段属性
表单字段共有的属性如下。
- disabled：布尔值，表示当前字段是否被禁用。
- form：指向当前字段所属表单的指针；只读。
- name：当前字段的名称。
- readOnly：布尔值，表示当前字段是否只读。
- tabIndex：表示当前字段的切换（tab）序号。
- type：当前字段的类型，如"checkbox"、 "radio"，等等。
- value：当前字段将被提交给服务器的值。对文件字段来说，这个属性是只读的，包含着文件

在计算机中的路径。除了 form 属性之外，可以通过 JavaScript 动态修改其他任何属性。
```js
var form = document.getElementById("myForm");
var field = form.elements[0];
//修改 value 属性
field.value = "Another value";
//检查 form 属性的值
alert(field.form === form); //true
//把焦点设置到当前字段
field.focus();
//禁用当前字段
field.disabled = true;
//修改 type 属性（不推荐，但对<input>来说是可行的）
field.type = "checkbox";
```
```js
//避免多次提交表单
EventUtil.addHandler(form, "submit", function(event){
    event = EventUtil.getEvent(event);
    var target = EventUtil.getTarget(event);
    //取得提交按钮
    var btn = target.elements["submit-btn"];
    //禁用它
    btn.disabled = true;
});
```
除了`<fieldset>`之外，所有表单字段都有 type 属性。对于`<input>元素，这个值等于 HTML 特性 type 的值。对于其他元素，这个 type 属性的值如下表所列。
![](/assets/深度截图_选择区域_20171211083954.png)

2.共有的表单字段方法
每个表单字段都有两个方法： focus()和 blur()。其中， focus()方法用于将浏览器的焦点设置到表单字段，即激活表单字段，使其可以响应键盘事件。
```js
EventUtil.addHandler(window, "load", function(event){
    document.forms[0].elements[0].focus();
});
```
要注意的是，如果第一个表单字段是一个<input>元素，且其 type 特性的值为"hidden"，那么
以上代码会导致错误。另外，如果使用 CSS 的 display 和 visibility 属性隐藏了该字段，同样也会导致错误。
HTML5 为表单字段新增了一个 autofocus 属性。在支持这个属性的浏览器中，只要设置这个属性，不用 JavaScript 就能自动把焦点移动到相应字段。
```html
<input type="text" autofocus>
```
与 focus()方法相对的是 blur()方法，它的作用是从元素中移走焦点。
```js
document.forms[0].elements[0].blur();
```
3.共有的表单字段事件
除了支持鼠标、键盘、更改和 HTML 事件之外，所有表单字段都支持下列 3 个事件。
- blur：当前字段失去焦点时触发。
- change：对于`<input>`和`<textarea>`元素，在它们失去焦点且 value 值改变时触发；对于`<select>`元素，在其选项改变时触发。
- focus：当前字段获得焦点时触发。