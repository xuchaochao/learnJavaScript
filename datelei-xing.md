Date类型使用自 UTC（Coordinated Universal Time，国际协调时间） 1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期。

创建日期对象

```js
var now = new Date();
```

ECMAScript 提供了两个方法： Date.parse\(\)  
 和 Date.UTC\(\) 转换日期

Date.parse\(\)接受下列日期格式：

* “月/日/年”，如 6/13/2004；
* “英文月名 日,年”，如 January 12,2004；
* “英文星期几 英文月名 日 年 时:分:秒 时区”，如 Tue May 25 2004 00:00:00 GMT-0700。
* ISO 8601 扩展格式 YYYY-MM-DDTHH:mm:ss.sssZ（例如 2004-05-25T00:00:00）。只有兼容ECMAScript 5的实现支持这种格式。

例如，要为 2004年 5月 25日创建一个日期对象，可以使用下面的代码：

```js
var someDate = new Date(Date.parse("May 25, 2004"))
```

如果传入 Date.parse\(\)方法的字符串不能表示日期，那么它会返回 NaN。实际上，如果直接将表示日期的字符串传递给 Date 构造函数，也会在后台调用 Date.parse\(\)。

Date.UTC\(\)方法同样也返回表示日期的毫秒数，但它与 Date.parse\(\)在构建值时使用不同的信息。 Date.UTC\(\)的参数分别是年份、基于 0的月份（一月是 0，二月是 1，以此类推）、月中的哪一天（1 到 31）、小时数（0 到 23）、分钟、秒以及毫秒数。在这些参数中，只有前两个参数（年和月）是必需的。如果没有提供月中的天数，则假设天数为 1；如果省略其他参数，则统统假设为 0。以下是两个使用 Date.UTC\(\)方法的例子：

```js
// GMT 时间 2000 年 1 月 1 日午夜零时
var y2k = new Date(Date.UTC(2000, 0));
// GMT 时间 2005 年 5 月 5 日下午 5:55:55
var allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));
```

也可以这么写：

```js
// 本地时间 2000 年 1 月 1 日午夜零时
var y2k = new Date(2000, 0);
// 本地时间 2005 年 5 月 5 日下午 5:55:55
var allFives = new Date(2005, 4, 5, 17, 55, 55);
```

ECMAScript 5 添加了 Data.now\(\)方法，返回表示调用这个方法时的日期和时间的毫秒数。

```js
//取得开始时间
var start = Date.now();
//调用函数
doSomething();
//取得停止时间
var stop = Date.now(),
    result = stop – start;
```

#### 继承的方法

Date 类型也重写了 toLocaleString\(\)、toString\(\)和 valueOf\(\)方法  
Date 类型的 valueOf\(\)方法返回日期的毫秒表示。因此，可以方便使用比较操作符（小于或大于）来比较日期值（根据时间的毫秒数）。

```js
var date1 = new Date(2007, 0, 1); //"January 1, 2007"
var date2 = new Date(2007, 1, 1); //"February 1, 2007"
alert(date1 < date2); //true
alert(date1 > date2); //false
```

#### 日期格式化方法

Date 类型还有一些专门用于将日期格式化为字符串的方法，这些方法如下。

* toDateString\(\)——以特定于实现的格式显示星期几、月、日和年；
* toTimeString\(\)——以特定于实现的格式显示时、分、秒和时区；
* toLocaleDateString\(\)——以特定于地区的格式显示星期几、月、日和年；
* toLocaleTimeString\(\)——以特定于实现的格式显示时、分、秒；
* toUTCString\(\)——以特定于实现的格式完整的 UTC 日期。（推荐这个）

#### 日期/时间组件方法

* getTime\(\) —— 返回表示日期的毫秒数；与valueOf\(\)方法返回的值相同
* setTime\(毫秒\) —— 以毫秒数设置日期，会改变整个日期
* getFullYear\(\) —— 取得4位数的年份（如2007而非仅07）
* getUTCFullYear\(\) —— 返回UTC日期的4位数年份
* setFullYear\(年\) —— 设置日期的年份。传入的年份值必须是4位数字（如2007而非仅07）
* setUTCFullYear\(年\) —— 设置UTC日期的年份。传入的年份值必须是4位数字（如2007而非仅07）
* getMonth\(\) —— 返回日期中的月份，其中0表示一月， 11表示十二月
* getUTCMonth\(\) —— 返回UTC日期中的月份，其中0表示一月， 11表示十二月
* setMonth\(月\) —— 设置日期的月份。传入的月份值必须大于0，超过11则增加年份
* setUTCMonth\(月\) —— 设置UTC日期的月份。传入的月份值必须大于0，超过11则增加年份
* getDate\(\) —— 返回日期月份中的天数（1到31）
* getUTCDate\(\) —— 返回UTC日期月份中的天数（1到31）
* setDate\(日\) —— 设置日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份
* setUTCDate\(日\) —— 设置UTC日期月份中的天数。如果传入的值超过了该月中应有的天数，则增加月份
* getDay\(\) —— 返回日期中星期的星期几（其中0表示星期日， 6表示星期六）
* getUTCDay\(\) —— 返回UTC日期中星期的星期几（其中0表示星期日， 6表示星期六）
* getHours\(\) —— 返回日期中的小时数（0到23）
* getUTCHours\(\) —— 返回UTC日期中的小时数（0到23）
* setHours\(时\) —— 设置日期中的小时数。传入的值超过了23则增加月份中的天数
* setUTCHours\(时\) —— 设置UTC日期中的小时数。传入的值超过了23则增加月份中的天数
* getMinutes\(\) —— 返回日期中的分钟数（0到59）
* getUTCMinutes\(\) —— 返回UTC日期中的分钟数（0到59）
* setMinutes\(分\) —— 设置日期中的分钟数。传入的值超过59则增加小时数
* setUTCMinutes\(分\) —— 设置UTC日期中的分钟数。传入的值超过59则增加小时数
* getSeconds\(\) ——  返回日期中的秒数（0到59）
* getUTCSeconds\(\) —— 返回UTC日期中的秒数（0到59）
* setSeconds\(秒\)——  设置日期中的秒数。传入的值超过了59会增加分钟数
* setUTCSeconds\(秒\) —— 设置UTC日期中的秒数。传入的值超过了59会增加分钟数
* getMilliseconds\(\) —— 返回日期中的毫秒数
* getUTCMilliseconds\(\) —— 返回UTC日期中的毫秒数
* setMilliseconds\(毫秒\) —— 设置日期中的毫秒数



