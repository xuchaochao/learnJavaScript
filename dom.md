DOM（文档对象模型）是针对 HTML 和 XML 文档的一个 API（应用程序编程接口）。 DOM 描绘了一个层次化的节点树，允许开发人员添加、移除和修改页面的某一部分。 DOM 脱胎于 Netscape 及微软公司创始的 DHTML（动态 HTML），但现在它已经成为表现和操作页面标记的真正的跨平台、语言中立的方式。

### 节点层次

DOM 可以将任何 HTML 或 XML 文档描绘成一个由多层节点构成的结构。节点分为几种不同的类型，每种类型分别表示文档中不同的信息及（或）标记。每个节点都拥有各自的特点、数据和方法，另外也与其他节点存在某种关系。节点之间的关系构成了层次，而所有页面标记则表现为一个以特定节点为根节点的树形结构。

```html
<html>
    <head>
        <title>Sample Page</title>
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>
```

文档节点是每个文档的根节点\(`<html>`\)。文档元素是文档的最外层元素，文档中的其他所有元素都包含在文档元素中。  
每一段标记都可以通过树中的一个节点来表示： HTML 元素通过元素节点表示，特性（attribute）通过特性节点表示，文档类型通过文档类型节点表示，而注释则通过注释节点表示。总共有 12 种节点类型，这些类型都继承自一个基类型。  
![](/assets/深度截图_选择区域_20171206130013.png)

#### Node类型

DOM1 级定义了一个 Node 接口，该接口将由 DOM 中的所有节点类型实现。这个 Node 接口在JavaScript 中是作为 Node 类型实现的；除了 IE 之外，在其他所有浏览器中都可以访问到这个类型。  
JavaScript 中的所有节点类型都继承自 Node 类型，因此所有节点类型都共享着相同的基本属性和方法。  
每个节点都有一个 nodeType 属性，用于表明节点的类型。节点类型由在 Node 类型中定义的下列12 个数值常量来表示，任何节点类型必居其一：

* Node.ELEMENT\_NODE\(1\)；
* Node.ATTRIBUTE\_NODE\(2\)；
* Node.TEXT\_NODE\(3\)；
* Node.CDATA\_SECTION\_NODE\(4\)；
* Node.ENTITY\_REFERENCE\_NODE\(5\)；
* Node.ENTITY\_NODE\(6\)；
* Node.PROCESSING\_INSTRUCTION\_NODE\(7\)；
* Node.COMMENT\_NODE\(8\)；
* Node.DOCUMENT\_NODE\(9\)；
* Node.DOCUMENT\_TYPE\_NODE\(10\)；
* Node.DOCUMENT\_FRAGMENT\_NODE\(11\)；
* Node.NOTATION\_NODE\(12\)

```js
if (someNode.nodeType == Node.ELEMENT_NODE){ //在 IE 中无效
    alert("Node is an element.");
}
if (someNode.nodeType == 1){ //适用于所有浏览器
    alert("Node is an element.");
}
```

1.nodeName 和 nodeValue 属性

```js
if (someNode.nodeType == 1){
    value = someNode.nodeName; //nodeName 的值是元素的标签名
}
```

2.节点关系  
节点间的各种关系可以用传统的家族关系来描述，相当于把文档树比喻成家谱。在 HTML 中，可以将`<body>`元素看成是`<html>`元素的子元素；相应地，也就可以将`<html>`元素看成是`<body>`元素的父元素。  
每个节点都有一个 childNodes 属性，其中保存着一个 NodeList 对象。 NodeList 是一种类数组对象，用于保存一组有序的节点，可以通过位置来访问这些节点。NodeList 对象的  
独特之处在于，它实际上是基于 DOM 结构动态执行查询的结果，因此 DOM 结构的变化能够自动反映在 NodeList 对象中。我们常说， NodeList 是有生命、有呼吸的对象，而不是在我们第一次访问它们的某个瞬间拍摄下来的一张快照。

```js
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var count = someNode.childNodes.length;

//在 IE8 及之前版本中无效
// 可以将 NodeList 对象转换为数组
var arrayOfNodes = Array.prototype.slice.call(someNode.childNodes,0);

// 所有浏览器中都可以运行
function convertToArray(nodes){
var array = null;
    try {
        array = Array.prototype.slice.call(nodes, 0); //针对非 IE 浏览器
    } catch (ex) {
        array = new Array();
        for (var i=0, len=nodes.length; i < len; i++){
            array.push(nodes[i]);
        }
    }
    return array;
}
```

每个节点都有一个 parentNode 属性，该属性指向文档树中的父节点。包含在 childNodes 列表中的所有节点都具有相同的父节点，因此它们的 parentNode 属性都指向同一个节点。通过使用列表中每个节点的 previousSibling和 nextSibling 属性，可以访问同一列表中的其他节点。列表中第一个节点的 previousSibling 属性值为 null，而列表中最后一个节点的 nextSibling 属性的值同样也为 null。  
父节点的 firstChild 和 lastChild 属性分别指向其 childNodes 列表中的第一个和最后一个节点。  
![](/assets/深度截图_选择区域_20171206131724.png)

3.操作节点  
appendChild\(\)，用于向 childNodes 列表的末尾添加一个节点。添加节点后， childNodes 的新增节点、父节点及以前的最后一个子节点的关系指针都会相应地得到更新。更新完成后， appendChild\(\)返回新增的节点。

```js
var returnedNode = someNode.appendChild(newNode);
alert(returnedNode == newNode); //true
alert(someNode.lastChild == newNode); //true
```

insertBefore\(\)方法。这个方法接受两个参数：要插入的节点和作为参照的节点。

```js
//插入后成为最后一个子节点
returnedNode = someNode.insertBefore(newNode, null);
alert(newNode == someNode.lastChild); //true
//插入后成为第一个子节点
var returnedNode = someNode.insertBefore(newNode, someNode.firstChild);
alert(returnedNode == newNode); //true
alert(newNode == someNode.firstChild); //true
//插入到最后一个子节点前面
returnedNode = someNode.insertBefore(newNode, someNode.lastChild);
alert(newNode == someNode.childNodes[someNode.childNodes.length-2]); //
```

replaceChild\(\)方法接受的两个参数是：要插入的节点和要替换的节点。

```js
//替换第一个子节点
var returnedNode = someNode.replaceChild(newNode, someNode.firstChild);
//替换最后一个子节点
returnedNode = someNode.replaceChild(newNode, someNode.lastChild);
```

如果只想移除而非替换节点，可以使用 removeChild\(\)方法。这个方法接受一个参数，即要移除的节点。被移除的节点将成为方法的返回值。

```js
//移除第一个子节点
var formerFirstChild = someNode.removeChild(someNode.firstChild);
//移除最后一个子节点
var formerLastChild = someNode.removeChild(someNode.lastChild);
```

cloneNode\(\)，用于创建调用这个方法的节点的一个完全相同的副本。 cloneNode\(\)方法接受一个布尔值参数，表示是否执行深复制。在参数为 true的情况下，执行深复制，也就是复制节点及其整个子节点树；在参数为 false 的情况下，执行浅复制，即只复制节点本身。

```js
ar deepList = myList.cloneNode(true);
alert(deepList.childNodes.length); //3（ IE < 9）或 7（其他浏览器）
var shallowList = myList.cloneNode(false);
alert(shallowList.childNodes.length); //0
```

#### Document类型

JavaScript 通过 Document 类型表示文档。在浏览器中， document 对象是 HTMLDocument（继承自 Document 类型）的一个实例，表示整个 HTML 页面。而且， document 对象是 window 对象的一个属性，因此可以将其作为全局对象来访问。 Document 节点具有下列特征：

* nodeType 的值为 9；
* nodeName 的值为"\#document"；
* nodeValue 的值为 null；
* parentNode 的值为 null；
* ownerDocument 的值为 null；
* 其子节点可能是一个 DocumentType （最多一个） 、 Element （最多一个） 、 ProcessingInstruction
  或 Comment。

1.文档的子节点
documentElement属性，该属性始终指向 HTML 页面中的`<html>`元素。
body 属性，直接指向<body>元素。
doctype 属性（在浏览器中是 document.doctype）来访问`<!DOCTYPE>`标签的信息。

2.文档信息
title，包含着<title>元素中的文本。
URL 属性中包含页面完整的 URL（即地址栏中显示的 URL）， domain 属性中只包含页面的域名，
referrer 属性中则保存着链接到当前页面的那个页面的 URL。

3.查找元素
getElementById()，接收一个参数：要取得的元素的 ID。如果找到相应的元素则返回该元素，如果不存在带有相应 ID 的元素，则返回 null。
```html
<div id="myDiv">Some text</div>
```
```js
var div = document.getElementById("myDiv"); //取得<div>元素的引用
```
> IE8 及较低版本不区分 ID 的大小写

getElementsByTagName()。这个方法接受一个参数，即要取得元素的标签名，而返回的是包含零或多个元素的 NodeList。
```js
var images = document.getElementsByTagName("img");
```
getElementsByClassName()。这个方法接受一个参数，即要取得元素的类名，而返回的是包含零或多个元素的 NodeList。

getElementsByName()。顾名思义，这个方法会返回带有给定 name 特性的所有元素。

4.特殊集合
document.anchors，包含文档中所有带 name 特性的`<a>`元素；
- document.applets，包含文档中所有的<applet>元素，因为不再推荐使用`<applet>`元素，
所以这个集合已经不建议使用了；
- document.forms，包含文档中所有的`<form>`元素，与 document.getElementsByTagName("form")
得到的结果相同；
- document.images，包含文档中所有的`<img>`元素，与 document.getElementsByTagName
("img")得到的结果相同；
- document.links，包含文档中所有带 href 特性的`<a>`元素。

5.DOM 一致性检测
hasFeature()方法接受两个参数：要检测的 DOM 功能的名称及版本号。如果浏览器支持给定名称和版本的功能，则该方法返回 true：
```js
var hasXmlDom = document.implementation.hasFeature("XML", "1.0");
```
6.文档写入
write()和 writeln()方法都接受一个字符串参数，即要写入到输出流中的文本。 write()会原样写入，而 writeln()则会在字符串的末尾添加一个换行符（\n）。
方法 open()和 close()分别用于打开和关闭网页的输出流。

#### Element类型
除了 Document 类型之外， Element 类型就要算是 Web 编程中最常用的类型了。 Element 类型用于表现 XML 或 HTML 元素，提供了对元素标签名、子节点及特性的访问。 Element 节点具有以下特征：
- nodeType 的值为 1；
- nodeName 的值为元素的标签名；
- nodeValue 的值为 null；
- parentNode 可能是 Document 或 Element；
- 其子节点可能是 Element、 Text、 Comment、 ProcessingInstruction、 CDATASection 或EntityReference。

1.HTML元素
所有 HTML 元素都由 HTMLElement 类型表示。每个 HTML 元素中都存在的下列标准特性。
id，元素在文档中的唯一标识符。
- title，有关元素的附加说明信息，一般通过工具提示条显示出来。
- lang，元素内容的语言代码，很少使用。
- dir，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左） ，也很少使用。
- className，与元素的 class 特性对应，即为元素指定的 CSS 类。

```js
var div = document.getElementById("myDiv");
alert(div.id); //"myDiv""
alert(div.className); //"bd"
alert(div.title); //"Body text"
alert(div.lang); //"en"
alert(div.dir); //"ltr"
```
2.取得特性
操作特性的 DOM 方法主要有三个，分别是 getAttribute()、 setAttribute()和 removeAttribute()。
```js
var div = document.getElementById("myDiv");
alert(div.getAttribute("id")); //"myDiv"
```
3.设置特性
```js
div.setAttribute("id", "someOtherId");// div.id = "someOtherId";
```
4.attributes 属性
Element 类型是使用 attributes 属性的唯一一个 DOM 节点类型。attributes 属性中包含一个NamedNodeMap，与 NodeList 类似，也是一个“动态”的集合。元素的每一个特性都由一个 Attr 节点表示，每个节点都保存在 NamedNodeMap 对象中。 NamedNodeMap 对象拥有下列方法。
- getNamedItem(name)：返回 nodeName 属性等于 name 的节点；
- removeNamedItem(name)：从列表中移除 nodeName 属性等于 name 的节点；
- setNamedItem(node)：向列表中添加节点，以节点的 nodeName 属性为索引；
- item(pos)：返回位于数字 pos 位置处的节点。

```js
var id = element.attributes.getNamedItem("id").nodeValue;
var id = element.attributes["id"].nodeValue;
element.attributes["id"].nodeValue = "someOtherId";
var oldAttr = element.attributes.removeNamedItem("id");
```

5.创建元素
使用 document.createElement()方法可以创建新元素。
```js
var div = document.createElement("div");
div.id = "myNewDiv";
div.className = "box";
// 创建后存内存中，还要插入到document中
document.body.appendChild(div);
```

6.元素的子节点
元素的 childNodes 属性中包含了它的所有子节点这些子节点有可能是元素、文本节点、注释或处理指令。
```js
for (var i=0, len=element.childNodes.length; i < len; i++){
    if (element.childNodes[i].nodeType == 1){
        //执行某些操作
    }
}
```

#### Text类型
文本节点由 Text 类型表示，包含的是可以照字面解释的纯文本内容。纯文本中可以包含转义后的 HTML 字符，但不能包含 HTML 代码。 Text 节点具有以下特征：
- nodeType 的值为 3；
- nodeName 的值为"#text"；
- nodeValue 的值为节点所包含的文本；
- parentNode 是一个 Element；
- 不支持（没有）子节点。
可以通过 nodeValue 属性或 data 属性访问 Text 节点中包含的文本，这两个属性中包含的值相
同。对 nodeValue 的修改也会通过 data 反映出来，反之亦然。使用下列方法可以操作节点中的文本。
- appendData(text)：将 text 添加到节点的末尾。
- deleteData(offset, count)：从 offset 指定的位置开始删除 count 个字符。
- insertData(offset, text)：在 offset 指定的位置插入 text。
- replaceData(offset, count, text)：用 text 替换从 offset 指定的位置开始到 offset+count 为止处的文本。
- splitText(offset)：从 offset 指定的位置将当前文本节点分成两个文本节点。
- substringData(offset, count)：提取从 offset 指定的位置开始到 offset+count 为止处的字符串。
> 每个可以包含内容的元素最多只能有一个文本节点。

```html
<!-- 没有内容，也就没有文本节点 -->
<div></div>
<!-- 有空格，因而有一个文本节点 -->
<div> </div>
<!-- 有内容，因而有一个文本节点 -->
<div>Hello World!</div>
```

1.创建文本节点
```js
var textNode = document.createTextNode("<strong>Hello</strong> world!");
```
2.规范化文本节点
normalize()方法，则会将所有文本节点合并成一个节点。

3.分割文本节点
splitText()。这个方法会将一个文本节点分成两个文本节点，即按照指定的位置分割 nodeValue 值。
```js
var element = document.createElement("div");
element.className = "message";
var textNode = document.createTextNode("Hello world!");
element.appendChild(textNode);
document.body.appendChild(element);
var newNode = element.firstChild.splitText(5);
alert(element.firstChild.nodeValue); //"Hello"
alert(newNode.nodeValue); //" world!"
alert(element.childNodes.length); //2
```
#### Comment类型
注释在 DOM 中是通过 Comment 类型来表示的。 Comment 节点具有下列特征：
- nodeType 的值为 8；
- nodeName 的值为"#comment"；
- nodeValue 的值是注释的内容；
- parentNode 可能是 Document 或 Element；
- 不支持（没有）子节点。

```html
<div id="myDiv"><!--A comment --></div>
```
```js
var div = document.getElementById("myDiv");
var comment = div.firstChild;
alert(comment.data); //"A comment"
```
使用 document.createComment()并为其传递注释文本也可以创建注释节点

### DOM操作技术
#### 动态脚本
动态加载的外部 JavaScript 文件能够立即运行，比如下面的<script>元素：
```html
<script type="text/javascript" src="client.js"></script>
```
```js
var script = document.createElement("script");
script.type = "text/javascript";
script.src = "client.js";
document.body.appendChild(script);
```
```js
// 封装成函数
function loadScript(url){
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = url;
    document.body.appendChild(script);
}
```
#### 动态样式
```js
function loadStyles(url){
    var link = document.createElement("link");
    link.rel = "stylesheet";
    link.type = "text/css";
    link.href = url;
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(link);
}
```

#### 使用NodeList
理解 NodeList 及其“近亲” NamedNodeMap 和 HTMLCollection，是从整体上透彻理解 DOM 的关键所在。这三个集合都是“动态的”；换句话说，每当文档结构发生变化时，它们都会得到更新。因此，它们始终都会保存着最新、最准确的信息。从本质上说，所有 NodeList 对象都是在访问 DOM 文档时实时运行的查询。例如，下列代码会导致无限循环：
```js
var divs = document.getElementsByTagName("div"),
    i,
    div;
for (i=0; i < divs.length; i++){
    div = document.createElement("div");
    document.body.appendChild(div);
}
```