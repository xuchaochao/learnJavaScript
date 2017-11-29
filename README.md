## 基本类型和引用类型的值

**ECMAScript**变量可能包含两种不同数据类型的值：基本类型值和引用类型值。**基本类型值**值的是简单的数据段，而**引用类型值**指那些可能由一个值构成的对象是。

#### 1.动态的属性

对于**引用类型值**我们可以为其添加属性和方法，也可以改变和删除其属性和方法。对**基本类型值**则不行。

```js
var person = new Object();
person.name = "John";
alert(person.name); // "John"

var name = "John";
name.age = 27;
alert(name.age); //undefined
```





