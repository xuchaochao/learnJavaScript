定义函数的方式有两种：一种是函数声明，另一种就是函数表达式。  
函数声明的语法是这样的。

```js
function functionName(arg0, arg1, arg2) {
    //函数体
}
```

第二种创建函数的方式是使用函数表达式。

```js
var functionName = function(arg0, arg1, arg2){
    //函数体
};
```

种形式看起来好像是常规的变量赋值语句，即创建一个函数并将它赋值给变量 functionName。这种情况下创建的函数叫做匿名函数（anonymous function），因为 function 关键字后面没有标识符。（匿名函数有时候也叫拉姆达函数。）匿名函数的 name 属性是空字符串。  
`函数表达式与其他表达式一样，在使用前必须先赋值。`

#### 递归

递归函数是在一个函数通过名字调用自身的情况下构成的：

```js
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * factorial(num-1);
    }
}
```

这是一个经典的递归阶乘函数。虽然这个函数表面看来没什么问题，但下面的代码却可能导致它出错。

```js
var anotherFactorial = factorial;
factorial = null;
alert(anotherFactorial(4)); //出错！
```

以上代码先把 factorial\(\)函数保存在变量 anotherFactorial 中，然后将 factorial 变量设置为 null，结果指向原始函数的引用只剩下一个。但在接下来调用 anotherFactorial\(\)时，由于必须执行 factorial\(\)，而 factorial 已经不再是函数，所以就会导致错误。在这种情况下，使用 arguments.callee 可以解决这个问题。

```js
function factorial(num){
    if (num <= 1){
        return 1;
    } else {
        return num * arguments.callee(num-1);
    }
}
```

但在严格模式下，不能通过脚本访问 arguments.callee，访问这个属性会导致错误。不过，可以使用命名函数表达式来达成相同的结果。

```js
var factorial = (function f(num){
    if (num <= 1){
        return 1;
    } else {
        return num * f(num-1);
    }
});
```

#### 闭包

闭包是指有权访问另一个函数作用域中的变量的函数。  
而理解闭包就需要知道作用域链以及作用域链的作用。

```js
function createComparisonFunction(propertyName) {
    return function(object1, object2){
        var value1 = object1[propertyName];
        var value2 = object2[propertyName];
        if (value1 < value2){
            return -1;
        } else if (value1 > value2){
            return 1;
        } else {
            return 0;
        }
    };
}
```

当某个函数被调用时，会创建一个执行环境（execution context）及相应的作用域链。然后，使用 arguments 和其他命名参数的值来初始化函数的活动对象（activation object）。但在作用域链中，外部函数的活动对象始终处于第二位，外部函数的外部函数的活动对象处于第三位，……直至作为作用域链终点的全局执行环境。无论什么时候在函数中访问一个变量时，就会从作用域链中搜索具有相应名字的变量。一般来讲，当函数执行完毕后，局部活动对象就会被销毁，内存中仅保存全局作用域（全局执行环境的变量对象）。  
但闭包则不同：在另一个函数内部定义的函数会将包含函数（即外部函数）的活动对象添加到它的作用域链中。因此，在 createComparisonFunction\(\)函数内部定义的匿名函数的作用域链中，实际上将会包含外部函数 createComparisonFunction\(\)的活动对象。

```js
var compare = createComparisonFunction("name");
var result = compare({ name: "Nicholas" }, { name: "Greg" });
```

在匿名函数从 createComparisonFunction\(\)中被返回后，它的作用域链被初始化为包含createComparisonFunction\(\)函数的活动对象和全局变量对象。这样，匿名函数就可以访问在createComparisonFunction\(\)中定义的所有变量。更为重要的是，createComparisonFunction\(\)函数在执行完毕后，其活动对象也不会被销毁，因为匿名函数的作用域链仍然在引用这个活动对象。换句话说，当 createComparisonFunction\(\)函数返回后，其执行环境的作用域链会被销毁，但它的活  
动对象仍然会留在内存中；直到匿名函数被销毁后， createComparisonFunction\(\)的活动对象才会被销毁。

```js
//创建函数
var compareNames = createComparisonFunction("name");
//调用函数
var result = compareNames({ name: "Nicholas" }, { name: "Greg" });
//解除对匿名函数的引用（以便释放内存）
compareNames = null;
```

![](/assets/深度截图_选择区域_20171203214826.png)

> 由于闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存。过度使用闭包可能会导致内存占用过多，我们建议读者只在绝对必要时再考虑使用闭包。虽然像 V8 等优化后的 JavaScript 引擎会尝试回收被闭包占用的内存，但请大家还是要慎重使用闭包。

##### 闭包与变量

作用域链的这种配置机制引出了一个值得注意的副作用，即闭包只能取得包含函数中任何变量的最后一个值。

```js
function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){
        result[i] = function(){
            return i;
        };
    }
    return result;
}
```
实际上，上面的每个函数都返回 10。因为每个函数的作用域链中都保存着createFunctions\(\) 函数的活动对象，所以它们引用的都是同一个变量i 。当createFunctions\(\)函数返回后，变量 i 的值是10，此时每个函数都引用着保存变量 i 的同一个变量对象，所以在每个函数内部 i 的值都是 10。但是，我们可以通过创建另一个匿名函数强制让闭包的行为符合预期。
```js
function createFunctions(){
    var result = new Array();
    for (var i=0; i < 10; i++){
        result[i] = function(num){
            return function(){
                return num;
            };
        }(i);
    }
    return result;
}
```
**关于this对象**
this 对象是在运行时基于函数的执行环境绑定的：在全局函数中， this 等于 window，而当函数被作为某个对象的方法调用时， this 等于那个对象。不过，匿名函数的执行环境具有全局性，因此其 this 对象通常指向 window。
```js
var name = "The Window";
var object = {
    name : "My Object",
    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
};
alert(object.getNameFunc()()); //"My Object"
```
> this 和 arguments 也存在同样的问题。如果想访问作用域中的 arguments 对
象，必须将对该对象的引用保存到另一个闭包能够访问的变量中。

##### 模仿块级作用域
JavaScript 没有块级作用域的概念。所以私有的变量不会销毁。
```js
function outputNumbers(count){
    for (var i=0; i < count; i++){
        alert(i);
    }
    alert(i); //计数
}
```
匿名函数可以用来模仿块级作用域并避免这个问题。
```js
function outputNumbers(count){
    (function () {
        for (var i=0; i < count; i++){
            alert(i);
        }
    })();
    alert(i); //导致一个错误！
}
```
> 这种做法可以减少闭包占用的内存问题，因为没有指向匿名函数的引用。只要函数执行完毕，就可以立即销毁其作用域链了。

#### 私有变量
> 初始化未经声明的变量，总是会创建一个全局变量。
任何在函数中定义的变量，都可以认为是私有变量，因为不能在函数的外部访问这些变量。私有变量包括函数的参数、局部变量和在函数内部定义的其他函数。
我们把有权访问私有变量和私有函数的公有方法称为特权方法（privileged method）。
```js
(function(){
    //私有变量和私有函数
    var privateVariable = 10;
    function privateFunction(){
        return false;
    }
    //构造函数
    MyObject = function(){
    };
    //公有/特权方法
    MyObject.prototype.publicMethod = function(){
        privateVariable++;
        return privateFunction();
    };
})();
```