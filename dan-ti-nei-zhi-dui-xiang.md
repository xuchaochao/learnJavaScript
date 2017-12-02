ECMA-262 对内置对象的定义是：“由 ECMAScript 实现提供的、不依赖于宿主环境的对象，这些对  
象在 ECMAScript 程序执行之前就已经存在了。”例如 Object、 Array 和 String。  
ECMA-262 还定义了两个单体内置对象： Global 和 Math。

#### Global对象

ECMAScript 中的 Global 对象在某种意义上是作为一个终极的“兜底儿对象”来定义的。所有在全局作用域中定义的属性和函数，都是 Global 对象的属性。

1.URL编码方法。  
Global 对象的 encodeURI\(\)和 encodeURIComponent\(\)方法可以对 URI（Uniform Resource Identifiers，通用资源标识符）进行编码，以便发送给浏览器。

```js
var uri = "http://www.wrox.com/illegal value.htm#start";
//"http://www.wrox.com/illegal%20value.htm#start"
alert(encodeURI(uri));
//"http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start"
alert(encodeURIComponent(uri));
```

> 一般来说，我们使用encodeURIComponent\(\)方法的时候要比使用encodeURI\(\)更多，因为在实践中更常见的是对查询字符串参数而不是对基础 URI进行编码。

对应的 decodeURI\(\)和 decodeURIComponent\(\)用于对URL进行解码。

```js
var uri = "http%3A%2F%2Fwww.wrox.com%2Fillegal%20value.htm%23start";
//http%3A%2F%2Fwww.wrox.com%2Fillegal value.htm%23start
alert(decodeURI(uri));
//http://www.wrox.com/illegal value.htm#start
alert(decodeURIComponent(uri));
```

2.eval\(\)方法  
eval\(\)  
方法就像是一个完整的 ECMAScript 解析器，它只接受一个参数，即要执行的 ECMAScript （或 JavaScript）字符串。

```js
eval("alert('hi')");
// 这行代码的作用等价于下面这行代码：
alert("hi");
```

> 严格模式下，在外部访问不到 eval\(\)中创建的任何变量或函数，为 eval 赋值也会导致错误。

3.Global 对象的属性  
![](/assets/深度截图_选择区域_20171202162306.png)

4.window对象  
ECMAScript 虽然没有指出如何直接访问 Global 对象，但 Web 浏览器都是将这个全局对象作为 window 对象的一部分加以实现的。因此，在全局作用域中声明的所有变量和函数，就都成为了 window 对象的属性。

```js
var color = "red";
function sayColor(){
    alert(window.color);
}
window.sayColor(); // "red"
```

#### Math对象

ECMAScript 还为保存数学公式和信息提供了一个公共位置，即 Math 对象。  
1.Math对象的属性  
![](/assets/深度截图_选择区域_20171202162632.png)

2.min\(\)和max\(\)方法  
min\(\)和 max\(\)方法用于确定一组数值中的最小值和最大值。这两个方法都可以接收任意多个数值参数。

```js
var max = Math.max(3, 54, 32, 16);
alert(max); //54

var min = Math.min(3, 54, 32, 16);
alert(min); //3
```

要找到数组中的最大或最小值，可以像下面这样使用 apply\(\)方法。

```js
var values = [1,2,3,4,5,6,7,8];
var max = Math.max.apply(Math, value)
```

3.舍入方法

* Math.ceil\(\)执行向上舍入，即它总是将数值向上舍入为最接近的整数；
* Math.floor\(\)执行向下舍入，即它总是将数值向下舍入为最接近的整数；
* Math.round\(\)执行标准舍入，即它总是将数值四舍五入为最接近的整数。
  ```js
  alert(Math.ceil(25.9)); //26
  alert(Math.ceil(25.5)); //26
  alert(Math.ceil(25.1)); //26
  alert(Math.round(25.9)); //26
  alert(Math.round(25.5)); //26
  alert(Math.round(25.1)); //25
  alert(Math.floor(25.9)); //25
  alert(Math.floor(25.5)); //25
  alert(Math.floor(25.1)); //25
  ```

4.random\(\)方法

Math.random\(\)方法返回大于等于 0 小于 1 的一个随机数。一般有这样的公式：

`值 = Math.floor(Math.random() * 可能值的总数 + 第一个可能的值)`

```js
// 如果你想选择一个 1 到 10 之间的数值
var num = Math.floor(Math.random() * 10 + 1);

// 如果想要选择一个介于 2 到 10 之间的值
var num = Math.floor(Math.random() * 9 + 2);
```

5.其他的方法  
![](/assets/深度截图_选择区域_20171202163711.png)

