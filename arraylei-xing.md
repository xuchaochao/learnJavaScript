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

ECMAScript 数组也提供了一种让数组的行为类似于其他数据结构的方法。具体说来，数组可以表现得就像栈一样，后者是一种可以限制插入和删除项的数据结构。栈是一种 LIFO（Last-In-First-Out，后进先出）的数据结构，也就是最新添加的项最早被移除。而栈中项的插入（叫做推入）和移除（叫做弹出），只发生在一个位置——栈的顶部。 ECMAScript 为数组专门提供了push\(\)和 pop\(\)方法，以便实现类似栈的行为。

```js
var colors = new Array();
colors.push("red", "green") //推入栈 ["red", "green"]
colors.pop()    // 弹出栈 ["red"]
colors.pop()    // []
```

#### 队列方法

栈数据结构的访问规则是 LIFO （后进先出），而队列数据结构的访问规则是 FIFO （First-In-First-Out，先进先出）。结合使用 shift\(\)和 push\(\)方法，可以像使用队列一样使用数组。

```js
var colors = new Array(); //创建一个数组
var count = colors.push("red", "green"); //推入两项
alert(count); //2
count = colors.push("black"); //推入另一项
alert(count); //3
var item = colors.shift(); //取得第一项
alert(item); //"red"
alert(colors.length); //2
```

ECMAScript 还为数组提供了一个 unshift\(\)方法。顾名思义，unshift\(\)与 shift\(\)的用途相反：它能在数组前端添加任意个项并返回新数组的长度。

```js
var colors = new Array(); //创建一个数组
var count = colors.unshift("red", "green"); //推入两项
```

#### 重排序方法

数组中存在两个重排序方法：reverse\(\)和sort\(\).  
reverse\(\)是降序排列：

```js
var values = [1, 2, 3, 4, 5];
values.reverse();
alert(values); //5,4,3,2,1
```

sort\(\)默认按升序排列，内部调用toString\(\)方法，比较字符串。

```js
var values = [0, 1, 5, 10, 15];
values.sort();
alert(values); //0,1,10,15,5
```

但是sort还是提供参数输入，作为排序的比较规则。

```js
// 升序
var values = [1,2,7,4,9,3]
values.sort(function(value1, value2){
    return value1 - value2
}) // [ 1, 2, 3, 4, 7, 9 ]

// 降序 使用reverse更快
values.sort(function(value1, value2){
    return value2 - value1
}) //[ 9, 7, 4, 3, 2, 1 ]
```

#### 操作方法

concat\(\) 数组的拼接

```js
var colors = ["red", "green", "blue"];
var colors2 = colors.concat("yellow", ["black", "brown"]);
alert(colors); //red,green,blue
alert(colors2); //red,green,blue,yellow,black,brown
```

slice\(\) 数组的截取，接受两个参数（返回项的起始和结束位置）但不包含结束

```js
var colors = ["red", "green", "blue", "yellow", "purple"];
var colors2 = colors.slice(1);
var colors3 = colors.slice(1,4);
alert(colors2); //green,blue,yellow,purple
alert(colors3); //green,blue,yellow
```

splice\(\) 功能强大，能实现删除、插入和替换

* 删除：可以删除任意数量的项，只需指定 2 个参数：要删除的第一项的位置和要删除的项数。
  例如， splice\(0,2\)会删除数组中的前两项。
* 插入：可以向指定位置插入任意数量的项，只需提供 3 个参数：起始位置、 0（要删除的项数）
  和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。例如，
  splice\(2,0,"red","green"\)会从当前数组的位置 2 开始插入字符串"red"和"green"。
* 替换：可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数：起
  始位置、要删除的项数和要插入的任意数量的项。插入的项数不必与删除的项数相等。例如，
  splice \(2,1,"red","green"\)会删除当前数组位置 2 的项，然后再从位置 2 开始插入字符串
  ```js
  var colors = ["red", "green", "blue"];
  var removed = colors.splice(0,1); // 删除第一项
  alert(colors); // green,blue
  alert(removed); // red，返回的数组中只包含一项
  removed = colors.splice(1, 0, "yellow", "orange"); // 从位置 1 开始插入两项
  alert(colors); // green,yellow,orange,blue
  alert(removed); // 返回的是一个空数组
  removed = colors.splice(1, 1, "red", "purple"); // 插入两项，删除一项
  alert(colors); // green,red,purple,orange,blue
  alert(removed); // yellow，返回的数组中只包含一项
  ```

#### 位置方法

indexOf\(\) ：接受下标作参数，从左到右的方式查找   
lastIndexOf\(\) ：接受下标作参数，从右到左的方式查找。  
在没有找到的情况下返回-1

```js
var numbers = [1,2,3,4,5,4,3,2,1];
alert(numbers.indexOf(4)); //3
alert(numbers.lastIndexOf(4)); //5
alert(numbers.indexOf(4, 4)); //5
alert(numbers.lastIndexOf(4, 4)); //3
var person = { name: "Nicholas" };
var people = [{ name: "Nicholas" }];
var morePeople = [person];
alert(people.indexOf(person)); //-1
alert(morePeople.indexOf(person)); //0
```

#### 迭代方法

ECMAScript 5 为数组定义了 5 个迭代方法。每个方法都接收两个参数：要在每一项上运行的函数和  
（可选的）运行该函数的作用域对象——影响 this 的值。传入这些方法中的函数会接收三个参数：数  
组项的值、该项在数组中的位置和数组对象本身。根据使用的方法不同，这个函数执行后的返回值可能  
会也可能不会影响方法的返回值。以下是这 5 个迭代方法的作用。

* every\(\)：对数组中的每一项运行给定函数，如果该函数对每一项都返回 true，则返回 true。
* filter\(\)：对数组中的每一项运行给定函数，返回该函数会返回 true 的项组成的数组。
* forEach\(\)：对数组中的每一项运行给定函数。这个方法没有返回值。
* map\(\)：对数组中的每一项运行给定函数，返回每次函数调用的结果组成的数组。
* some\(\)：对数组中的每一项运行给定函数，如果该函数对任一项返回 true，则返回 true。
  以上方法都不会修改数组中的包含的值。

```js
// every
var numbers = [1,2,3,4,5,4,3,2,1];
var everyResult = numbers.every(function(item, index, array){
  return (item > 2);
});
alert(everyResult); //false
// some
var someResult = numbers.some(function(item, index, array){
    return (item > 2);
});
alert(someResult); //true
// filter
var numbers = [1,2,3,4,5,4,3,2,1];
var filterResult = numbers.filter(function(item, index, array){
    return (item > 2);
});
alert(filterResult); //[3,4,5,4,3]
// map
var numbers = [1,2,3,4,5,4,3,2,1];
var mapResult = numbers.map(function(item, index, array){
    return item * 2;
});
alert(mapResult); //[2,4,6,8,10,8,6,4,2]
// foreach
var numbers = [1,2,3,4,5,4,3,2,1];
numbers.forEach(function(item, index, array){
    //执行某些操作
});
```


#### 归并方法

ECMAScript 5 还新增了两个归并数组的方法： reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，然后构建一个最终返回的值。唯一不同的就是reduce从左到右，reduceRight()从右到左。它们都接收 4 个参数：前一个值、当前值、项的索引和数组对象。
```js
// reduce
var values = [1,2,3,4,5];
var sum = values.reduce(function(prev, cur, index, array){
    return prev + cur;
});
alert(sum); //15

// reduceRight
var values = [1,2,3,4,5];
var sum = values.reduceRight(function(prev, cur, index, array){
    return prev + cur;
});
alert(sum); //15
```


