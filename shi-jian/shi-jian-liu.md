在单击按钮的同时，你也单击了按钮的容器元素，甚至也单击了整个页面。事件流描述的是从页面中接收事件的顺序。IE 和 Netscape 开发团队居然提出了差不多是完全相反的事件流的概念。 IE 的事件流是事件冒泡流，而 Netscape Communicator 的事件流是事件捕获流。

#### 事件冒泡
IE 的事件流叫做事件冒泡（event bubbling），即事件开始时由最具体的元素（文档中嵌套层次最深的那个节点）接收，然后逐级向上传播到较为不具体的节点（文档）。
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Event Bubbling Example</title>
    </head>
    <body>
        <div id="myDiv">Click Me</div>
    </body>
</html>
```
如果你单击了页面中的<div>元素，那么这个 click 事件会按照如下顺序传播：
![](/assets/深度截图_选择区域_20171207085509.png)

> 所有现代浏览器都支持事件冒泡，IE9、 Firefox、 Chrome 和 Safari 则将事件一直冒泡到 window 对象。

#### 事件捕获
Netscape Communicator 团队提出的另一种事件流叫做事件捕获（event capturing）。事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于在事件到达预定目标之前捕获它。
![](/assets/深度截图_选择区域_20171207085537.png)

虽然事件捕获是 Netscape Communicator 唯一支持的事件流模型，但 IE9、 Safari、 Chrome、 Opera 和 Firefox 目前也都支持这种事件流模型。尽管“ DOM2 级事件”规范要求事件应该从 document 对象
开始传播，但这些浏览器都是从 window 对象开始捕获事件的。
> 由于老版本的浏览器不支持，因此很少有人使用事件捕获。我们也建议读者放心地使用事件冒泡，在有特殊需要时再使用事件捕获。

#### DOM事件流
“ DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。首先发生的是事件捕获，为截获事件提供了机会。然后是实际的目标接收到事件。最后一个阶段是冒泡阶段，可以在这个阶段对事件做出响应。
![](/assets/深度截图_选择区域_20171207085811.png)
> IE9、 Opera、 Firefox、 Chrome 和 Safari 都支持 DOM 事件流； IE8 及更早版本不支持 DOM 事件流。