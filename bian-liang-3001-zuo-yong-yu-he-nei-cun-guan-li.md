## 基本类型和引用类型的值

**ECMAScript**变量可能包含两种不同数据类型的值：基本类型值和引用类型值。**基本类型值**值的是简单的数据段，而**引用类型值**是指那些可能由一个值构成的对象。

#### 1.动态的属性

对于**引用类型值**我们可以为其添加属性和方法，也可以改变和删除其属性和方法。对**基本类型值**则不行。

```js
var person = new Object();
person.name = "John";
alert(person.name); // "John"
// 分隔线
var name = "John";
name.age = 27;
alert(name.age); //undefined
```

#### 2.复制变量值

**基本类型值**复制时后是完全独立的，两个变量不会相互影响。

**引用类型值**复制是复制一份指针，相互影响。

![](/assets/深度截图_选择区域_20171129210712.png)![](/assets/深度截图_选择区域_20171129210745.png)

#### 3.传递参数

**ECMAScript**中所有函数的参数都是按值传递的。**基本类型值**的传递如同基本类型变量的复制一样，而**引用类型值**的传递，则如同引用类型变量的复制一样。

```js
function addTen(num){
    num += 10;
    return num;
}
var count = 20;
var result = addTen(count);
alert(count); // 20
alert(result); // 30
// 分隔线
function setName(obj) {
    obj.name = "John";
}
var person = new Object();
setName(person);
alert(person.name); //"John"
```

#### 4.检测类型

使用_typeof_操作符确定一个变量是基本类型值还是引用类型值。

```js
var s = "Nicholas";
var b = true;
var i = 22;
var u;
var n = null;
var o = new Object();
alert(typeof s); //string
alert(typeof i); //number
alert(typeof b); //boolean
alert(typeof u); //undefined
alert(typeof n); //object
alert(typeof o); //object
```

但_typeof_只能判断引用类型值为_object_，不能再进一步的判断了。为此**ECMAScript**还提供了_instanceof_操作符。  
语法：`result = variable instanceof constructor`

```js
alert(person instanceof Object); // 变量 person 是 Object 吗？
alert(colors instanceof Array); // 变量 colors 是 Array 吗？
alert(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```

## 执行环境及作用域

**执行环境**（有时也称为“ 环境”）是 JavaScript 中最为重要的一个概念。执行环境定义了变量或函数有权访问的其他数据，决定了它们各自的行为。每个执行环境都有一个与之关联的变量对象 ，环境中定义的所有变量和函数都保存在这个对象中。

全局执行环境是最外围的一个执行环境。根据**ECMAScript**实现所在的宿主环境不同，表示执行环境的对象也不一样。在 Web 浏览器中，全局执行环境被认为是 window 对象，因此所有全局变量和函数都是作为 window 对象的属性和方法创建的。某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量和函数定义也随之销毁（全局执行环境直到应用程序退出——例如关闭网页或浏览器——时才会被销毁）。

每个函数都有自己的执行环境。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境**ECMAScript**程序中的执行流正是由这个方便的机制控制着。

当代码在一个环境中执行时，会创建变量对象的一个作用域链（scope chain）。作用域链的用途，是保证对执行环境有权访问的所有变量和函数的有序访问。

```js
var color = "blue";
function changeColor(){
    var anotherColor = "red";
    function swapColors(){
        var tempColor = anotherColor;
        anotherColor = color;
        color = tempColor;
        // 这里可以访问 color、 anotherColor 和 tempColor
    }
    // 这里可以访问 color 和 anotherColor，但不能访问 tempColor
    swapColors();
}
// 这里只能访问 color
changeColor();
```

![](/assets/深度截图_选择区域_20171129214649.png)

图 中的矩形表示特定的执行环境。其中，内部环境可以通过作用域链访问所有的外部环境，但  
外部环境不能访问内部环境中的任何变量和函数。

#### 1.延长作用域链

使用_with_和_try-catch的catch块_延长作用域链。

```js
function buildUrl() {
    var qs = "?debug=true";
    with(location){
        var url = href + qs;
    }
    return url;
}
```

#### 2.没有块级作用域

JavaScript 没有块级作用域。在_if_和_for_语句中并不会得到想要的结果

```js
if (true) {
    var color = "blue";
}
alert(color); //"blue"
// 分隔线
for (var i=0; i < 10; i++){
    doSomething(i);
}
alert(i); //10
```

变量未声明而先使用的情况

```js
function add(num1, num2) {
    var sum = num1 + num2;
    return sum;
}
var result = add(10, 20); //30
alert(sum); //由于 sum 不是有效的变量，因此会导致错误
// 分隔线
function add(num1, num2) {
    sum = num1 + num2;
    return sum;
}
var result = add(10, 20); //30
alert(sum); //30
```

查询标识符

当在某个环境中为了读取或写入而引用一个标识符时，必须通过搜索来确定该标识符实际代表什  
么。搜索过程从作用域链的前端开始，向上逐级查询与给定名字匹配的标识符。如果在局部环境中找到  
了该标识符，搜索过程停止，变量就绪。如果在局部环境中没有找到该变量名，则继续沿作用域链向上  
搜索。搜索过程将一直追溯到全局环境的变量对象。如果在全局环境中也没有找到这个标识符，则意味  
着该变量尚未声明。

```js
var color = "blue";
    function getColor(){
    return color;
}
alert(getColor()); //"blue"
// 分隔线
var color = "blue";
function getColor(){
    var color = "red";
    return color;
}
alert(getColor()); //"red"
```

## 垃圾收集

JavaScript 具有自动垃圾收集机制，也就是说，执行环境会负责管理代码执行过程中使用的内存。

#### 1.标记清除

JavaScript 中最常用的垃圾收集方式是标记清除（mark-and-sweep）。

垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记（当然，可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。最后，垃圾收集器完成内存清除工作，销毁那些带标记的值并回收它们所占用的内存空间。

#### 2.引用计数

另一种不太常见的垃圾收集策略叫做引用计数（reference counting）。引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是 1。如果同一个值又被赋给另一个变量，则该值的引用次数加 1。相反，如果包含对这个值引用的变量又取得了另外一个值，则这个值的引用次数减 1。当这个值的引用次数变成 0 时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间回收回来。这样，当垃圾收集器下次再运行时，它就会释放那些引用次数为零的值所占用的内存。

#### 3.性能问题

#### 4.管理内存

优化内存占用的最佳方式，就是为执行中的代码只保存必要的数据。一旦数据不再有用，最好通过将其值设置为 null 来释放其引用——这个做法叫做解除引用（dereferencing）。这一做法适用于大多数全局变量和全局对象的属性。局部变量会在它们离开执行环境时自动被解除引用，如

```js
function createPerson(name){
    var localPerson = new Object();
    localPerson.name = name;
    return localPerson;
}
var globalPerson = createPerson("Nicholas");
// 手工解除 globalPerson 的引用
globalPerson = null;
```



