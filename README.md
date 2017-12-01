# 个人JavaScript学习笔记

记录个人学习 《JavaScript高级程序设计》的读书笔记。

#### JavaScript 实现

一个完整的 JavaScript 实现应该由下列三个不同的部分组成（见图 1-1）。

* 核心（ECMAScript）
* 文档对象模型（DOM）
* 浏览器对象模型（BOM）

![](/assets/深度截图_选择区域_20171201134558.png)

#### ECMAScript

欧洲计算机制造商协会（ECMA， EuropeanComputer Manufacturers Association）制定的JavaScript标准，提供核心语言功能  
大致说来，它规定了这门语言的下列组成部分：

* 语法
* 类型
* 语句
* 关键字
* 保留字
* 操作符
* 对象

#### 文档对象模型\(DOM\)

文档对象模型（DOM， Document Object Model）是针对 XML 但经过扩展用于 HTML 的应用程序编程接口（API， Application Programming Interface）。 它提供访问和操作网页内容的方法和接口；

```html
看下面这个
HTML 页面：
<html>
    <head>
        <title>Sample Page</title>
    </head>
    <body>
        <p>Hello World!</p>
    </body>
</html>
```

DOM1 级主要是映射文档的结构  
DOM2 级引入了下列新模块，也给出了众多新类型和新接口的定义

* DOM 视图（DOM Views）：定义了跟踪不同文档（例如，应用 CSS 之前和之后的文档）视图的
  接口；
* DOM 事件（DOM Events）：定义了事件和事件处理的接口；
* DOM 样式（DOM Style）：定义了基于 CSS 为元素应用样式的接口；
* DOM 遍历和范围（DOM Traversal and Range）：定义了遍历和操作文档树的接口。
  DOM3 级则进一步扩展了 DOM，引入了以统一方式加载和保存文档的方法

#### 浏览器对象模型\(BOM\)

BOM 提供与浏览器交互的方法和接口。BOM 只处理浏览器窗口和框架；但人们习惯上也把所有针对浏览器的 JavaScript 扩展算作 BOM 的一部分。下面就是一些这样的扩展：

* 弹出新浏览器窗口的功能；
* 移动、缩放和关闭浏览器窗口的功能；
* 提供浏览器详细信息的 navigator 对象；
* 提供浏览器所加载页面的详细信息的 location 对象；
* 提供用户显示器分辨率详细信息的 screen 对象；
* 对 cookies 的支持；
* 像 XMLHttpRequest 和 IE 的 ActiveXObject 这样的自定义对象



