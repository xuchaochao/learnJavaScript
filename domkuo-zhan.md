### 选择符 API
Selectors API（www.w3.org/TR/selectors-api/）是由 W3C 发起制定的一个标准，致力于让浏览器原生支持 CSS 查询。
Selectors API Level 1 的核心是两个方法： querySelector()和 querySelectorAll()。在兼容的浏览器中，可以通过 Document 及 Element 类型的实例调用它们。目前已完全支持 Selectors API Level 1 的浏览器有 IE 8+、 Firefox 3.5+、 Safari 3.1+、 Chrome 和 Opera 10+。

#### querySelector()方法
```js
//取得 body 元素
var body = document.querySelector("body");
//取得 ID 为"myDiv"的元素
var myDiv = document.querySelector("#myDiv");
//取得类为"selected"的第一个元素
var selected = document.querySelector(".selected");
//取得类为"button"的第一个图像元素
var img = document.body.querySelector("img.button");
```

#### querySelectorAll()方法
这个方法返回的是一个 NodeList 的实例。
```js
//取得某<div>中的所有<em>元素（类似于 getElementsByTagName("em")）
var ems = document.getElementById("myDiv").querySelectorAll("em");
//取得类为"selected"的所有元素
var selecteds = document.querySelectorAll(".selected");
//取得所有<p>元素中的所有<strong>元素
var strongs = document.querySelectorAll("p strong");
```

#### matchesSelector()方法
Selectors API Level 2 规范为 Element 类型新增了一个方法 matchesSelector()。这个方法接收一个参数，即 CSS 选择符，如果调用元素与该选择符匹配，返回 true；否则，返回 false。
```js
if (document.body.matchesSelector("body.page1")){
    //true
}
```

### 元素遍历
Element Traversal API 为 DOM 元素添加了以下 5 个属性。
- childElementCount：返回子元素（不包括文本节点和注释）的个数。
- firstElementChild：指向第一个子元素； firstChild 的元素版。
- lastElementChild：指向最后一个子元素； lastChild 的元素版。
- previousElementSibling：指向前一个同辈元素； previousSibling 的元素版。
- nextElementSibling：指向后一个同辈元素； nextSibling 的元素版。

> 支持 Element Traversal 规范的浏览器有 IE 9+、 Firefox 3.5+、 Safari 4+、 Chrome 和 Opera 10+。

### HTML5
HTML5 规范则围绕如何使用新增标记定义了大量 JavaScript API。其中一些 API 与 DOM 重叠，定义了浏览器应该支持的 DOM 扩展。

#### 与类相关的扩充
HTML5 新增了很多 API，致力于简化 CSS 类的用法。
1.getElementsByClassName()方法
```js
//取得所有类中包含"username"和"current"的元素，类名的先后顺序无所谓
var allCurrentUsernames = document.getElementsByClassName("username current");
//取得 ID 为"myDiv"的元素中带有类名"selected"的所有元素
var selected = document.getElementById("myDiv").getElementsByClassName("selected");
```
2.classList 属性
在操作类名时，需要通过 className 属性添加、删除和替换类名。HTML5 新增了一种操作类名的方式，可以让操作更简单也更安全，那就是为所有元素添加 classList 属性。这个 classList 属性是新集合类型 DOMTokenList 的实例。这个新类型还定义如下方法。
- add(value)：将给定的字符串值添加到列表中。如果值已经存在，就不添加了。
- contains(value)：表示列表中是否存在给定的值，如果存在则返回 true，否则返回 false。
- remove(value)：从列表中删除给定的字符串。
- toggle(value)：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。
```js
//删除"disabled"类
div.classList.remove("disabled");
//添加"current"类
div.classList.add("current");
//切换"user"类
div.classList.toggle("user");
//确定元素中是否包含既定的类名
if (div.classList.contains("bd") && !div.classList.contains("disabled")){
//执行操作
)
```
#### 焦点管理
HTML5 也添加了辅助管理 DOM 焦点的功能。首先就是 document.activeElement 属性，这个属性始终会引用 DOM 中当前获得了焦点的元素。
```js
var button = document.getElementById("myButton");
button.focus();
alert(document.activeElement === button); //true
```
默认情况下，文档刚刚加载完成时， document.activeElement 中保存的是 document.body 元素的引用。文档加载期间， document.activeElement 的值为 null。
另外就是新增了 document.hasFocus()方法，这个方法用于确定文档是否获得了焦点。

#### HTMLDocument的变化 
1.readyState 属性
- loading，正在加载文档；
- complete，已经加载完文档。

```js
if (document.readyState == "complete"){
    //执行操作
}
```

2.兼容模式
在标准模式下， document.compatMode 的值等于"CSS1Compat"，而在混杂模式下， document.compatMode 的值等于"BackCompat"。

3.head 属性
HTML5 新增了 document.head 属性，引用文档的<head>元素。实现 document.head 属性的浏览器包括 Chrome 和 Safari 5。

#### 字符集属性
HTML5 新增了几个与文档字符集有关的属性。其中， charset 属性表示文档中实际使用的字符集，也可以用来指定新字符集。默认情况下，这个属性的值为"UTF-16"，但可以通过`<meta>`元素、响应头部或直接设置 charset 属性修改这个值。

#### 自定义数据属性
HTML5 规定可以为元素添加非标准的属性，但要添加前缀 data-，目的是为元素提供与渲染无关的信息，或者提供语义信息。
```js
var div = document.getElementById("myDiv");
//取得自定义属性的值
var appId = div.dataset.appId;
var myName = div.dataset.myname;
//设置值
div.dataset.appId = 23456;
div.dataset.myname = "Michael";
//有没有"myname"值呢？
if (div.dataset.myname){
    alert("Hello, " + div.dataset.myname);
}
```

#### 插入标记
1.innerHTML 属性
在读模式下， innerHTML 属性返回与调用元素的所有子节点（包括元素、注释和文本节点）对应的 HTML 标记。在写模式下， innerHTML 会根据指定的值创建新的 DOM 树，然后用这个 DOM 树完全替换调用元素原先的所有子节点。
```js
div.innerHTML = "Hello world!";
```
2.outerHTML 属性
在读模式下， outerHTML 返回调用它的元素及所有子节点的 HTML 标签。在写模式下， outerHTML 会根据指定的 HTML 字符串创建新的 DOM 子树，然后用这个 DOM 子树完全替换调用元素。

3.insertAdjacentHTML()方法
插入标记的最后一个新增方式是 insertAdjacentHTML()方法。这个方法最早也是在 IE中出现的，它接收两个参数：插入位置和要插入的 HTML 文本。第一个参数必须是下列值之一：
- "beforebegin"，在当前元素之前插入一个紧邻的同辈元素；
- "afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前再插入新的子元素；
- "beforeend"，在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的子元素；
- "afterend"，在当前元素之后插入一个紧邻的同辈元素。

```js
//作为前一个同辈元素插入
element.insertAdjacentHTML("beforebegin", "<p>Hello world!</p>");
//作为第一个子元素插入
element.insertAdjacentHTML("afterbegin", "<p>Hello world!</p>");
//作为最后一个子元素插入
element.insertAdjacentHTML("beforeend", "<p>Hello world!</p>");
//作为后一个同辈元素插入
element.insertAdjacentHTML("afterend", "<p>Hello world!</p>");
```

#### scrollIntoView()方法
如何滚动页面也是 DOM 规范没有解决的一个问题。在各种专有方法中， HTML5 最终选择了 scrollIntoView()作为标准方法。
```js
//让元素可见
document.forms[0].scrollIntoView();
```

### 专有扩展

#### children属性
children 属性与 childNodes 没有什么区别，即在元素只包含元素子节点时，这两个属性的值相同。
```js
var childCount = element.children.length;
var firstChild = element.children[0];
```
#### contains()方法
```js
alert(document.documentElement.contains(document.body)); //true
```

#### 插入文本
1.innerText 属性
2.outerText 属性

### 样式