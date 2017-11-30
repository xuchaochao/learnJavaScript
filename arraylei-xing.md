虽然 ECMAScript 数组与其他语言中的数组都是数据的有序列表，但与其他语言不同的是， ECMAScript 数组的每一项可以保存任何类型的数据。而且， ECMAScript 数组的大小是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。

```js
var array = new Array();
array[0] = 1;
array[1] = "22";
array[2] = true;
alert(array); // [1, "22", true]
```

同样创建数组也有两种基本方式：  
new方法创建

```js
var colors = new Array();
var colors = new Array(20); // 创建length值为20的数组
var colors = new Array("red", "blue", "green") // 创建带初始值的数组 

// 省略new
var colors = Array(3); // 创建一个包含 3 项的数组
var names = Array("Greg"); // 创建一个包含 1 项，即字符串"Greg"的数组
```

数组字面表示法，但不会调用构造函数。

```js
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
var names = []; // 创建一个空数组
var values = [1,2,]; // 不要这样！这样会创建一个包含 2 或 3 项的数组
var options = [,,,,,]; // 不要这样！这样会创建一个包含 5 或 6 项的数组
```



