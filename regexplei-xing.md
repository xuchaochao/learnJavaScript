ECMAScript 通过 RegExp 类型来支持正则表达式。  
创建正则表达式：

```js
var expression = / pattern / flags;
var expression = new RegExp(pattern, flags);
```

正则表达式的匹配模式支持下列 3 个标志。

* g：表示全局（global）模式，即模式将被应用于所有字符串，而非在发现第一个匹配项时立即停止；
* i：表示不区分大小写（case-insensitive）模式，即在确定匹配项时忽略模式与字符串的大小写；
* m：表示多行（multiline）模式，即在到达一行文本末尾时还会继续查找下一行中是否存在与模式匹配的项。

```js
/*
* 匹配字符串中所有"at"的实例
*/
var pattern1 = /at/g;
/*
* 匹配第一个"bat"或"cat"，不区分大小写
*/
var pattern2 = /[bc]at/i;
/*
* 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
*/
var pattern3 = /.at/gi;
```

与其他语言中的正则表达式类似，模式中使用的所有元字符都必须转义。正则表达式中的元字符包括：  
`( [ { \ ^ $ | ) ? * + .]}`  
这些元字符在正则表达式中都有一或多种特殊用途，因此如果想要匹配字符串中包含的这些字符，就必须对它们进行转义。下面给出几个例子。

```js
/*
* 匹配第一个"bat"或"cat"，不区分大小写
*/
var pattern1 = /[bc]at/i;
/*
* 匹配第一个" [bc]at"，不区分大小写
*/
var pattern2 = /\[bc\]at/i;
/*
* 匹配所有以"at"结尾的 3 个字符的组合，不区分大小写
*/
var pattern3 = /.at/gi;
/*
* 匹配所有".at"，不区分大小写
*/
var pattern4 = /\.at/gi;
```

#### RegExp实例属性（了解就好）

RegExp 的每个实例都具有下列属性，通过这些属性可以取得有关模式的各种信息。

* global：布尔值，表示是否设置了 g 标志。
* ignoreCase：布尔值，表示是否设置了 i 标志。
* lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从 0 算起。
* multiline：布尔值，表示是否设置了 m 标志。
* source：正则表达式的字符串表示，按照字面量形式而非传入构造函数中的字符串模式返回。

```js
var pattern1 = /\[bc\]at/i;
alert(pattern1.global); //false
alert(pattern1.ignoreCase); //true
alert(pattern1.multiline); //false
alert(pattern1.lastIndex); //0
alert(pattern1.source); //"\[bc\]at"

var pattern2 = new RegExp("\\[bc\\]at", "i");
alert(pattern2.global); //false
alert(pattern2.ignoreCase); //true
alert(pattern2.multiline); //false
alert(pattern2.lastIndex); //0
alert(pattern2.source); //"\[bc\]at"
```

#### RegExp实例方法

**exec\(\)方法**

RegExp 对象的主要方法是 exec\(\)，exec\(\)接受一个参数，即要应用模式的字符串，然后返回包含第一个匹配项信息的数组；或者在没有匹配项的情况下返回 null。  
exec\(\) 包含两个额外的属性： index 和 input。其中， index 表示匹配项在字符串中的位置，而 input 表示应用正则表达式的字符串。在数组中，第一项是与整个模式匹配的字符串，其他项是与模式中的捕获组匹配的字符串（如果模式中没有捕获组，则该数组只包含一项）。

```js
var text = "mom and dad and baby";
var pattern = /mom( and dad( and baby)?)?/gi;

var matches = pattern.exec(text);
alert(matches.index); // 0
alert(matches.input); // "mom and dad and baby"
alert(matches[0]); // "mom and dad and baby"
alert(matches[1]); // " and dad and baby"
alert(matches[2]); // " and baby"
```

对 exec\(\)方法而言，即使在模式中设置了全局标志（g），它每次也只会返回一个匹配项。在不设置全局标志的情况下，在同一个字符串上多次调用 exec\(\)将始终返回第一个匹配项的信息。而在设置全局标志的情况下，每次调用 exec\(\)则都会在字符串中继续查找新匹配项，如下面的例子所示。

```js
var text = "cat, bat, sat, fat";
var pattern1 = /.at/;

var matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0

matches = pattern1.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern1.lastIndex); //0

var pattern2 = /.at/g;

var matches = pattern2.exec(text);
alert(matches.index); //0
alert(matches[0]); //cat
alert(pattern2.lastIndex); //3

matches = pattern2.exec(text);
alert(matches.index); //5
alert(matches[0]); //bat
alert(pattern2.lastIndex); //8
```

** IE 的 JavaScript 实现在 lastIndex 属性上存在偏差，即使在非全局模式下，lastIndex 属性每次也会变化。**

** test\(\)方法 **

正则表达式的第二个方法是 test\(\)，它接受一个字符串参数。在模式与该参数匹配的情况下返回true；否则，返回 false。

```js
var text = "000-00-0000";
var pattern = /\d{3}-\d{2}-\d{4}/;

if (pattern.test(text)){
    alert("The pattern was matched.");
}
```

#### RegExp 构造函数属性

* input （$\_） 最近一次要匹配的字符串。 Opera未实现此属性

* lastMatch （$&） 最近一次的匹配项。 Opera未实现此属性

* lastParen （$+） 最近一次匹配的捕获组。 Opera未实现此属性

* leftContext （$\`） input字符串中lastMatch之前的文本

* multiline （$\*） 布尔值，表示是否所有表达式都使用多行模式。 IE和Opera未实现此属性

* rightContext （$'） Input字符串中lastMatch之后的文本

```js
var text = "this has been a short summer";
var pattern = /(.)hort/g;
/*
* 注意： Opera 不支持 input、 lastMatch、 lastParen 和 multiline 属性
* Internet Explorer 不支持 multiline 属性
*/
if (pattern.test(text)){
    alert(RegExp.input); // this has been a short summer
    alert(RegExp.leftContext); // this has been a
    alert(RegExp.rightContext); // summer
    alert(RegExp.lastMatch); // short
    alert(RegExp.lastParen); // s
    alert(RegExp.multiline); // false
}

if (pattern.test(text)){
    alert(RegExp.$_); // this has been a short summer
    alert(RegExp["$`"]); // this has been a
    alert(RegExp["$'"]); // summer
    alert(RegExp["$&"]); // short
    alert(RegExp["$+"]); // s
    alert(RegExp["$*"]); // false
}
```

以及9个用于存储捕获组的构造函数属性。RegExp.$1、 RegExp.$2…RegExp.$9

```js
var text = "this has been a short summer";
var pattern = /(..)or(.)/g;
if (pattern.test(text)){
    alert(RegExp.$1); //sh
    alert(RegExp.$2); //t
}
```

#### 模式的局限性
EXMAScript的正则表达式还有一些不支持的特性。
- 匹配字符串开始和结尾的\A 和\Z 锚
- 向后查找（lookbehind） 
- 并集和交集类
- 原子组（atomic grouping）
- Unicode 支持（单个字符除外，如\uFFFF）
- 命名的捕获组
- s（single，单行）和 x（free-spacing，无间隔）匹配模式
- 条件匹配
- 正则表达式注释




