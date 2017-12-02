类的创建：

```js
var person = new Object();
person.name = "Nicholas";
person.age = 29;
person.job = "Software Engineer";
person.sayName = function(){
    alert(this.name);
};
```

```js
var person = {
    name: "Nicholas",
    age: 29,
    job: "Software Engineer",
    sayName: function(){
        alert(this.name);
    }
};
```

#### 属性类型

ECMA-262 第 5 版在定义只有内部才用的特性（attribute）时，描述了属性（property）的各种特征。  
ECMAScript中有两种属性：数据属性和访问器属性。

1.数据属性  
数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有 4 个描述其行为的特性。

* \[\[Configurable\]\]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特
  性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的
  这个特性默认值为 true。
* \[\[Enumerable\]\]：表示能否通过 for-in 循环返回属性。像前面例子中那样直接在对象上定
  义的属性，它们的这个特性默认值为 true。
* \[\[Writable\]\]：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的
  这个特性默认值为 true。
* \[\[Value\]\]：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，
  把新值保存在这个位置。这个特性的默认值为 undefined。

对于像前面例子中那样直接在对象上定义的属性，它们的\[\[Configurable\]\]、 \[\[Enumerable\]\]和\[\[Writable\]\]特性都被设置为 true，而\[\[Value\]\]特性被设置为指定的值。

```js
var person = {
    name: "Nicholas"
}
```
要修改属性默认的特性，必须使用 ECMAScript 5 的 Object.defineProperty()方法。这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。其中，描述符（descriptor）对象的属性必须是： configurable、 enumerable、 writable 和 value。设置其中的一或多个值，可以修改对应的特性值。
```js
var person = {};
Object.defineProperty(person, "name", {
    writable: false,
    value: "Nicholas"
});

alert(person.name); //"Nicholas"
person.name = "Greg";
alert(person.name); //"Nicholas"
```
这个例子创建了一个名为 name 的属性，它的值"Nicholas"是只读的。这个属性的值是不可修改的，如果尝试为它指定新值，则在非严格模式下，赋值操作将被忽略；在严格模式下，赋值操作将会导致抛出错误。
类似的规则也适用于不可配置的属性。
```js
var person = {};
Object.defineProperty(person, "name", {
    configurable: false,
    value: "Nicholas"
});
alert(person.name); //"Nicholas"
delete person.name;
alert(person.name); //"Nicholas
```

2.访问器属性
访问器属性不包含数据值；它们包含一对儿 getter 和 setter 函数（不过，这两个函数都不是必需的）。在读取访问器属性时，会调用 getter 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用setter 函数并传入新值，这个函数负责决定如何处理数据。访问器属性有如下 4 个特性。
- [[Configurable]]：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特
性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为
true。
- [[Enumerable]]：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这
个特性的默认值为 true。
- [[Get]]：在读取属性时调用的函数。默认值为 undefined。
- [[Set]]：在写入属性时调用的函数。默认值为 undefined。

访问器属性不能直接定义，必须使用Object.defineProperty()来定义。
```js
var book = {
    _year: 2004,
    edition: 1
};
Object.defineProperty(book, "year", {
    get: function(){
        return this._year;
    },
    set: function(newValue){
        if (newValue > 2004) {
            this._year = newValue;
            this.edition += newValue - 2004;
        }
    }
});
book.year = 2005;
alert(book.edition); //2
```

#### 定义多个属性
由于为对象定义多个属性的可能性很大， ECMAScript 5 又定义了一个 Object.defineProperties()方法。利用这个方法可以通过描述符一次定义多个属性。这个方法接收两个对象参数：第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应。
```js
var book = {};
Object.defineProperties(book, {
    _year: {
        value: 2004
    },
    
    edition: {
        value: 1
    },
    
    year: {
        get:function(){
            return this._year;
        },
        set:function(newValue){
            if(newValue > newValue){
                this._year = newValue,
                this.edition += newValue - 2004;
            }
        }
    }
})
```
#### 读取属性的特性
使用 ECMAScript 5 的 Object.getOwnPropertyDescriptor()方法，可以取得给定属性的描述符。这个方法接收两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable、 enumerable、 get 和 set；如果是数据属性，这个对象的属性有 configurable、 enumerable、 writable 和 value。
```js
var book = {};
Object.defineProperties(book, {
    _year: {
        value: 2004
    },
    edition: {
        value: 1
    },
    year: {
        get: function(){
            return this._year;
        },
        set: function(newValue){
            if (newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }
        }
    }
});
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value); //2004
alert(descriptor.configurable); //false

alert(typeof descriptor.get); //"undefined"
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value); //undefined
alert(descriptor.enumerable); //false
alert(typeof descriptor.get); //"function"
```