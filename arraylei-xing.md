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

Array的访问：

```js
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
colors.length = 2;
alert(colors[2]); //undefined

colors.length = 4;
alert(colors[3]); //undefined
```

我们向 colors 数组的位置 99 插入了一个值，结果数组新长度（length）就是 100（99+1）。而位置 3 到位置 98 实际上都是不存在的，所以访问它们都将返回 undefined。

#### 检测数组

**instanceof**方法

```js
if (value instanceof Array){
    //对数组执行某些操作
}
```

但有时在网页中存在不同的框架，这种方法可能就不能很好的工作。

**isArray**方法

```js
if (Array.isArray(value)){
    //对数组执行某些操作
}
```

支持 Array.isArray\(\)方法的浏览器有 IE9+、 Firefox 4+、 Safari 5+、 Opera 10.5+和 Chrome。

#### 转化方法

所有对象都具有 toLocaleString\(\)、 toString\(\)和 valueOf\(\)方法。其中，调用数组的 toString\(\)方法会返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串。而调用 valueOf\(\)返回的还是数组。

```js
var colors = ["red", "blue", "green"]; // 创建一个包含 3 个字符串的数组
alert(colors.toString()); // red,blue,green
alert(colors.valueOf()); // red,blue,green
alert(colors); // red,blue,green
```

一般转化默认都是使用**toString\(\)**, 但当指定转化函数之后则不同。

```js
var person1 = {
    toLocaleString : function () {
        return "Nikolaos";
    },
    toString : function() {
        return "Nicholas";
    }
};
var person2 = {
    toLocaleString : function () {
        return "Grigorios";
    },
    toString : function() {
        return "Greg";
    }
};
var people = [person1, person2];
alert(people); //Nicholas,Greg
alert(people.toString()); //Nicholas,Greg
alert(people.toLocaleString()); //Nikolaos,Grigorios
```
#### 栈方法

ECMAScript 数组也提供了一种让数组的行为类似于其他数据结构的方法。具体说来，数组可以表现得就像栈一样，后者是一种可以限制插入和删除项的数据结构。栈是一种 LIFO（Last-In-First-Out，后进先出）的数据结构，也就是最新添加的项最早被移除。而栈中项的插入（叫做推入）和移除（叫做弹出），只发生在一个位置——栈的顶部。 ECMAScript 为数组专门提供了push()和 pop()方法，以便实现类似栈的行为。
```js
var colors = new Array();
colors.push("red", "green") //推入栈 ["red", "green"]
colors.pop()    // 弹出栈 ["red"]
colors.pop()    // []
```

