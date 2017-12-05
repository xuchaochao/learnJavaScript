最早由 Netscape Navigator 2.0 引入的 navigator 对象，现在已经成为识别客户端浏览器的事实标准。下表列出了存在于所有浏览器中的属性和方法，以及支持它们的浏览器版本。
![](/assets/深度截图_选择区域_20171205130336.png)
![](/assets/深度截图_选择区域_20171205130418.png)

#### 检测插件
对于非 IE 浏览器，可以使用plugins 数组来达到这个目的。该数组中的每一项都包含下列属性。
- name：插件的名字。
- description：插件的描述。
- filename：插件的文件名。
- length：插件所处理的 MIME 类型数量。

```js
//检测插件（在 IE 中无效）
function hasPlugin(name){
    name = name.toLowerCase();
    for (var i=0; i < navigator.plugins.length; i++){
        if (navigator. plugins [i].name.toLowerCase().indexOf(name) > -1){
            return true;
        }
    }
    return false;
}
//检测 Flash
alert(hasPlugin("Flash"));
//检测 QuickTime
alert(hasPlugin("QuickTime"));
```
检测 IE 中的插件比较麻烦，因为 IE 不支持 Netscape 式的插件。在 IE 中检测插件的唯一方式就是使用专有的 ActiveXObject 类型，并尝试创建一个特定插件的实例。 
```js
//检测 IE 中的插件
function hasIEPlugin(name){
    try {
        new ActiveXObject(name);
        return true;
    } catch (ex){
        return false;
    }
}
//检测 Flash
alert(hasIEPlugin("ShockwaveFlash.ShockwaveFlash"));
//检测 QuickTime
alert(hasIEPlugin("QuickTime.QuickTime"));
//检测所有浏览器中的 Flash
function hasFlash(){
    var result = hasPlugin("Flash");
    if (!result){
        result = hasIEPlugin("ShockwaveFlash.ShockwaveFlash");
    }
    return result;
}
//检测所有浏览器中的 QuickTime
function hasQuickTime(){
    var result = hasPlugin("QuickTime");
    if (!result){
        result = hasIEPlugin("QuickTime.QuickTime");
    }
    return result;
}
//检测 Flash
alert(hasFlash());
//检测 QuickTime
alert(hasQuickTime());
```
#### 注册处理程序
Firefox 2 为 navigator 对象新增了 registerContentHandler()和 registerProtocolHandler()方法。
其中， registerContentHandler()方法接收三个参数：要处理的 MIME 类型、可以处理该 MIME类型的页面的 URL 以及应用程序的名称。举个例子，要将一个站点注册为处理 RSS 源的处理程序，可以使用如下代码。
```js
navigator.registerContentHandler("application/rss+xml",
    "http://www.somereader.com?feed=%s", "Some Reader");
```
类似的调用方式也适用于 registerProtocolHandler()方法，它也接收三个参数：要处理的协议（例如， mailto 或 ftp）、处理该协议的页面的 URL 和应用程序的名称。例如，要想将一个应用程序注册为默认的邮件客户端，可以使用如下代码。
```js
navigator.registerProtocolHandler("mailto",
    "http://www.somemailclient.com?cmd=%s", "Some Mail Client");
```
