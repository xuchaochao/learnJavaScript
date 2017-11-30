创建Object实例的方式有两种。  
第一种是使用new操作符后跟object构造函数。

```js
var person = new Object(); // 同var person = {};
person.name = "John"
person.age = 29;
```

另一种是使用对象字面量表示。但它不会调用构造函数。

```js
var person = {
    name: "John",
    age: 29
}
```

在这个例子中，左边的花括号（{）表示对象字面量的开始，因为它出现在了表达式上下文（expression context）中。 ECMAScript 中的表达式上下文指的是能够返回一个值（表达式）。赋值操作符表示后面是一个值，所以左花括号在这里表示一个表达式的开始。同样的花括号，如果出现在一个语句上下文（statement context）中，例如跟在 if 语句条件的后面则表示一个语句块的开始。

一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言中通用的语法。不过，在 JavaScript 也可以使用方括号表示法来访问对象的属性。

```js
// 方括号法
alert(person["name"]); // "John"
// 点表示法
alert(person.name) // "John"
```

通常，除非必须使用变量来访问属性，否则我们建议使用点表示法。

