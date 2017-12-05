location 是最有用的 BOM 对象之一，它提供了与当前窗口中加载的文档有关的信息，还提供了一  
些导航功能。事实上， location 对象是很特别的一个对象，因为它既是 window 对象的属性，也是document 对象的属性；换句话说， window.location 和 document.location 引用的是同一个对象。  
![](/assets/深度截图_选择区域_20171205124810.png)

#### 查询字符串参数

```js
function getQueryStringArgs(){
    //取得查询字符串并去掉开头的问号
    var qs = (location.search.length > 0 ? location.search.substring(1) : ""),
    //保存数据的对象
    args = {},
    //取得每一项
    items = qs.length ? qs.split("&") : [],
    item = null,
    name = null,
    value = null,
    //在 for 循环中使用
    i = 0,
    len = items.length;
    //逐个将每一项添加到 args 对象中
    for (i=0; i < len; i++){
        item = items[i].split("=");
        // 因为查询字符串应该是被编码过
        name = decodeURIComponent(item[0]);
        value = decodeURIComponent(item[1]);
        if (name.length) {
            args[name] = value;
        }
    }
    return args;
}
//假设查询字符串是?q=javascript&num=10
var args = getQueryStringArgs();
alert(args["q"]); //"javascript"
alert(args["num"]); //"10"
```

#### 位置操作

使用 location 对象可以通过很多方式来改变浏览器的位置。首先，也是最常用的方式，就是使用assign\(\)方法并为其传递一个 URL。

```js
location.assign("http://www.wrox.com");
```

```js
// 其实也是调用assign()方法
window.location = "http://www.wrox.com";
location.href = "http://www.wrox.com";
```

修改location 对象的其他属性也可以改变当前加载的页面。

```js
//假设初始 URL 为 http://www.wrox.com/WileyCDA/
//将 URL 修改为"http://www.wrox.com/WileyCDA/#section1"
location.hash = "#section1";
//将 URL 修改为"http://www.wrox.com/WileyCDA/?q=javascript"
location.search = "?q=javascript";
//将 URL 修改为"http://www.yahoo.com/WileyCDA/"
location.hostname = "www.yahoo.com";
//将 URL 修改为"http://www.yahoo.com/mydir/"
location.pathname = "mydir";
//将 URL 修改为"http://www.yahoo.com:8080/WileyCDA/"
location.port = 8080;
```

以上修改URL的方式，浏览器的历史记录中就会生成一条新记录，因此用户通过单击“后退”按钮都会导航到前一个页面。要禁用这种行为，可以使用 replace\(\)方法。

```js
location.replace("http://www.wrox.com/");
```

与位置有关的最后一个方法是 reload\(\)，作用是重新加载当前显示的页面。

```js
location.reload(); //重新加载（有可能从缓存中加载）
location.reload(true); //重新加载（从服务器重新加载）
```



