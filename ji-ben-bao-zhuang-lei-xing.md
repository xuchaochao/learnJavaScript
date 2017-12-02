为了便于操作基本类型值， ECMAScript 还提供了 3 个特殊的引用类型： Boolean、 Number 和 String。  
实际上，每当读取一个基本类型值的时候，后台就会创建一个对应的基本包装类型的对象，从而让我们能够调用一些方法来操作这些数据。

```js
var s1 = "some text";
var s2 = s1.substring(2);
```

简单的一个引用，但后台执行了一系列的处理：  
\(1\) 创建 String 类型的一个实例；  
\(2\) 在实例上调用指定的方法；  
\(3\) 销毁这个实例

引用类型与基本包装类型的主要区别就是对象的生存期。使用 new 操作符创建的引用类型的实例，在执行流离开当前作用域之前都一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁。这意味着我们不能在运行时为基本类型值添加属性和方法。

```js
var s1 = "some text";
s1.color = "red";
alert(s1.color); //undefined
```

#### Boolean类型

Boolean 类型是与布尔值对应的引用类型。要创建Boolean 对象，可以像下面这样调用 Boolean构造函数并传入 true 或 false 值。

```js
var booleanObject = new Boolean(true);
```

#### Number类型

Number 是与数字值对应的引用类型。要创建 Number 对象，可以在调用 Number 构造函数时向其中传递相应的数值。

```js
var numberObject = new Number(10);
```

Number类型的方法：  
Number 类型也重写了 valueOf\(\)、toLocaleString\(\)和 toString\(\)方法。重写后的 valueOf\(\)方法返回对象表示的基本类型的数值，另外两个方法则返回字符串形式的数值。

```js
var num = 10;
alert(num.toString()); //"10"
alert(num.toString(2)); //"1010"
alert(num.toString(8)); //"12"
alert(num.toString(10)); //"10"
alert(num.toString(16)); //"a"
```

toFixed\(\)方法会按照指定的小数位返回数值的字符串表示：

```js
var num = 10;
alert(num.toFixed(2)); //"10.00"

var num = 10.005;
alert(num.toFixed(2)); //"10.01"
```

toExponential\(\)和toPrecision\(\)方法方法返回以指数表示法（也称 e 表示法）表示的数值的字符串形式。

```js
var num = 10;
alert(num.toExponential(1)); //"1.0e+1"

var num = 99;
alert(num.toPrecision(1)); //"1e+2"
alert(num.toPrecision(2)); //"99"
alert(num.toPrecision(3)); //"99.0"
```

#### String类型

String 类型是字符串的对象包装类型，可以像下面这样使用 String 构造函数来创建。

```js
var stringObject = new String("hello world")
```

String类型的方法：  
继承的 valueOf\(\)、toLocaleString\(\)和 toString\(\)方法，都返回对象所表示的基本字符串值。  
1.字符方法

* charAt\(\)方法以单字符字符串的形式返回给定位置的那个字符
* charCodeAt\(\)方法以字符编码的形式返回给定位置的那个字符

```js
var stringValue = "hello world";
alert(stringValue.charAt(1)); //"e"
alert(stringValue.charCodeAt(1)); //输出"101"
```



2.字符串操作方法（不修改原来字符串）：  
concat\(\)，用于将一或多个字符串拼接起来，返回拼接得到的新字符串

```js
var stringValue = "hello ";
var result = stringValue.concat("world");
alert(result); //"hello world"
alert(stringValue); //"hello"

// 接收多个参数
var result = stringValue.concat("world", "!");
alert(result); //"hello world!"
alert(stringValue); //"hello"
```

slice\(\)、 substr\(\)和 substring\(\)。  
这三个方法都会返回被操作字符串的一个子字符串，而且也都接受一或两个参数。第一个参数指定子字符串的开始位置，第二个参数（在指定的情况下）表示子字符串到哪里结束。具体来说， slice\(\)和 substring\(\)的第二个参数指定的是子字符串最后一个字符后面的位置。而 substr\(\)的第二个参数指定的则是返回的字符个数。如果没有给这些方法传递第二个参数，则将字符串的长度作为结束位置。

```js
var stringValue = "hello world";
alert(stringValue.slice(3)); //"lo world"
alert(stringValue.substring(3)); //"lo world"
alert(stringValue.substr(3)); //"lo world"
alert(stringValue.slice(3, 7)); //"lo w"
alert(stringValue.substring(3,7)); //"lo w"
alert(stringValue.substr(3, 7)); //"lo worl"
```

在传递给这些方法的参数是负值的情况下，它们的行为就不尽相同了。其中， slice\(\)方法会将传入的负值与字符串的长度相加， substr\(\)方法将负的第一个参数加上字符串的长度，而将负的第二个参数转换为 0。最后，substring\(\)方法会把所有负值参数都转换为 0。

```js
var stringValue = "hello world";
alert(stringValue.slice(-3)); //"rld"
alert(stringValue.substring(-3)); //"hello world"
alert(stringValue.substr(-3)); //"rld"
alert(stringValue.slice(3, -4)); //"lo w"
alert(stringValue.substring(3, -4)); //"hel"
alert(stringValue.substr(3, -4)); //""（空字符串）
```



3.字符串位置方法  
indexOf\(\)和 lastIndexOf\(\)。这两个方法都是从一个字符串中搜索给定的子字符串，然后返子字符串的位置（如果没有找到该子字符串，则返回-1）。这两个方法的区别在于： indexOf\(\)方法从字符串的开头向后搜索子字符串，而 lastIndexOf\(\)方法是从字符串的末尾向前搜索子字符串。

```js
var stringValue = "hello world";
alert(stringValue.indexOf("o"));
alert(stringValue.lastIndexOf("o"));
```

这两个方法都可以接收可选的第二个参数，表示从字符串中的哪个位置开始搜索。

```js
var stringValue = "hello world";
alert(stringValue.indexOf("o", 6)); //7
alert(stringValue.lastIndexOf("o", 6)); //4
```



4.trim\(\)方法  
ECMAScript 5 为所有字符串定义了 trim\(\)方法。这个方法会创建一个字符串的副本，删除前置及后缀的所有空格，然后返回结果。

```js
var stringValue = " hello world ";
var trimmedStringValue = stringValue.trim();
alert(stringValue); //" hello world "
alert(trimmedStringValue); //"hello world"
```



5.字符串大小写转换方法  
toLowerCase\(\)、 toLocaleLowerCase\(\)、 toUpperCase\(\)和 toLocaleUpperCase\(\)

```js
var stringValue = "hello world";
alert(stringValue.toLocaleUpperCase()); //"HELLO WORLD"
alert(stringValue.toUpperCase()); //"HELLO WORLD"
alert(stringValue.toLocaleLowerCase()); //"hello world"
alert(stringValue.toLowerCase()); //"hello world"
```

一般来说，在不知道自己的代码将在哪种语言环境中运行的情况下，还是使用针对地区的方法更稳妥一些。  


6.字符串的模式匹配方法  
String 类型定义了几个用于在字符串中匹配模式的方法。第一个方法就是 match\(\)。  
match\(\)方法只接受一个参数，要么是一个正则表达式，要么是一个 RegExp 对象。

```js
var text = "cat, bat, sat, fat";
var pattern = /.at/;
//与 pattern.exec(text)相同
var matches = text.match(pattern);
alert(matches.index); //0
alert(matches[0]); //"cat"
alert(pattern.lastIndex); //0
```

另一个用于查找模式的方法是 search\(\)。这个方法的唯一参数与 match\(\)方法的参数相同。  
search\(\)方法返回字符串中第一个匹配项的索引；如果没有找到匹配项，则返回-1。而且， search\(\)方法始终是从字符串开头向后查找模式。

```js
var text = "cat, bat, sat, fat";
var pos = text.search(/at/);
alert(pos); //1
```

ECMAScript 提供了 replace\(\)方法。  
这个方法接受两个参数：第一个参数可以是一个 RegExp 对象或者一个字符串（这个字符串不会被转换成正则表达式），第二个参数可以是一个字符串或者一个函数。如果第一个参数是字符串，那么只会替换第一个子字符串。要想替换所有子字符串，唯一的办法就是提供一个正则表达式，而且要指定全局（g）标志。

```js
var text = "cat, bat, sat, fat";
var result = text.replace("at", "ond");
alert(result); //"cond, bat, sat, fat"
result = text.replace(/at/g, "ond");
alert(result); //"cond, bond, sond, fond"
```

```js
function htmlEscape(text){
    return text.replace(/[<>"&]/g, function(match, pos, originalText){
        switch(match){
            case "<":
                return "&lt;";
            case ">":
                return "&gt;";
            case "&":
                return "&amp;";
            case "\"":
                return "&quot;";
        }
    });
}
alert(htmlEscape("<p class=\"greeting\">Hello world!</p>"));
//&lt;p class=&quot;greeting&quot;&gt;Hello world!&lt;/p&gt;
```

最后一个与模式匹配有关的方法是 split\(\)，这个方法可以基于指定的分隔符将一个字符串分割成多个子字符串，并将结果放在一个数组中。分隔符可以是字符串，也可以是一个 RegExp 对象（这个方法不会将字符串看成正则表达式）。 split\(\)方法可以接受可选的第二个参数，用于指定数组的大小，以便确保返回的数组不会超过既定大小。

```js
var colorText = "red,blue,green,yellow";
var colors1 = colorText.split(","); //["red", "blue", "green", "yellow"]
var colors2 = colorText.split(",", 2); //["red", "blue"]
var colors3 = colorText.split(/[^\,]+/); //["", ",", ",", ",", ""]
```

7.localeCompare\(\)方法  
按字母表中的顺序比较字符串的大小。

* 如果字符串在字母表中应该排在字符串参数之前，则返回一个负数（大多数情况下是-1，具体的值要视实现而定）；
* 如果字符串等于字符串参数，则返回 0；
* 如果字符串在字母表中应该排在字符串参数之后，则返回一个正数（大多数情况下是 1，具体的值同样要视实现而定）。
  ```js
  var stringValue = "yellow";
  alert(stringValue.localeCompare("brick")); //1
  alert(stringValue.localeCompare("yellow")); //0
  alert(stringValue.localeCompare("zoo")); //-1
  ```

8.fromCharCode\(\)方法

这个方法的任务是接收一或多个字符编码，然后将它们转换成一个字符串。

```js
alert(String.fromCharCode(104, 101, 108, 108, 111)); //"hello"
```



