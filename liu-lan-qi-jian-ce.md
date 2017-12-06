迄今为止，客户端检测仍然是 Web 开发领域中一个饱受争议的话题。一谈到这个话题，人们总会不约而同地提到浏览器应该支持一组最常用的公共功能。在理想状态下，确实应该如此。但是，在现实当中，浏览器之间的差异以及不同浏览器的“怪癖”（quirk），多得简直不胜枚举。因此，客户端检测除了是一种补救措施之外，更是一种行之有效的开发策略。

### 能力检测

最常用也最为人们广泛接受的客户端检测形式是能力检测（又称特性检测）。能力检测的目标不是识别特定的浏览器，而是识别浏览器的能力。采用这种方式不必顾及特定的浏览器如何如何，只要确定浏览器支持特定的能力，就可以给出解决方案。能力检测的基本模式如下：

```js
if (object.propertyInQuestion){
    //使用 object.propertyInQuestion
}
```

#### 更可靠的能力检测

在可能的情况下，要尽量使用 typeof 进行能力检测。特别是，宿主对象没有义务让 typeof 返回合理的值。

```js
//这样更好：检查 sort 是不是函数
function isSortable(object){
    return typeof object.sort == "function";
}
```

#### 能力检测，不是浏览器检测

根据浏览器不同将能力组合起来是更可取的方式。如果你知道自己的应用程序需要使用某些特定的浏览器特性，那么最好是一次性检测所有相关特性，而不要分别检测。

```js
//确定浏览器是否支持 Netscape 风格的插件
var hasNSPlugins = !!(navigator.plugins && navigator.plugins.length);
//确定浏览器是否具有 DOM1 级规定的能力
var hasDOM1 = !!(document.getElementById && document.createElement &&
                document.getElementsByTagName);
```

### 怪癖检测

与能力检测类似， 怪癖检测（quirks detection）的目标是识别浏览器的特殊行为。但与能力检测确认浏览器支持什么能力不同，怪癖检测是想要知道浏览器存在什么缺陷（“怪癖”也就是 bug）。  
例如， IE8 及更早版本中存在一个 bug，即如果某个实例属性与\[\[Enumerable\]\]标记为 false 的某个原型属性同名，那么该实例属性将不会出现在fon-in 循环当中。可以使用如下代码来检测这种“怪癖”。

```js
var hasDontEnumQuirk = function(){
    var o = { toString : function(){} };
        for (var prop in o){
        if (prop == "toString"){
            return false;
        }
    }
    return true;
}();
```

以上代码通过一个匿名函数来测试该“怪癖”，函数中创建了一个带有 toString\(\)方法的对象在正确的 ECMAScript 实现中， toString 应该在 for-in 循环中作为属性返回。另一个经常需要检测的“怪癖”是 Safari 3 以前版本会枚举被隐藏的属性。可以用下面的函数来检测该“怪癖”。

```js
var hasEnumShadowsQuirk = function(){
    var o = { toString : function(){} };
    var count = 0;
    for (var prop in o){
        if (prop == "toString"){
            count++;
        }
    }
    return (count > 1);
}();
```

如果浏览器存在这个 bug，那么使用 for-in 循环枚举带有自定义的 toString\(\)方法的对象，就会返回两个 toString 的实例。一般来说，“怪癖”都是个别浏览器所独有的，而且通常被归为 bug。在相关浏览器的新版本中，这些问题可能会也可能不会被修复。由于检测“怪癖”涉及运行代码，因此我们建议仅检测那些对你有直接影响的“怪癖”，而且最好在脚本一开始就执行此类检测，以便尽早解决问题。

### 用户代理检测

第三种，也是争议最大的一种客户端检测技术叫做用户代理检测。用户代理检测通过检测用户代理  
字符串来确定实际使用的浏览器。在每一次 HTTP 请求过程中，用户代理字符串是作为响应首部发送的，而且该字符串可以通过 JavaScript 的 navigator.userAgent 属性访问。

1.识别呈现引擎  
2.识别浏览器  
3.识别平台  
4.识别 Windows 操作系统
5.识别移动设备  
6.识别游戏系统

#### 完整的代码

以下是完整的用户代理字符串检测脚本，包括检测呈现引擎、平台、 Windows 操作系统、移动设备和游戏系统。

```js
var client = function() {
    //呈现引擎
    var engine = {
        ie: 0,
        gecko: 0,
        webkit: 0,
        khtml: 0,
        opera: 0,
        //完整的版本号
        ver: null
    };
    //浏览器
    var browser = {
        //主要浏览器
        ie: 0,
        firefox: 0,
        safari: 0,
        konq: 0,
        opera: 0,
        chrome: 0,
        //具体的版本号
        ver: null
    };
    //平台、设备和操作系统
    var system = {
        win: false,
        mac: false,
        x11: false,
        //移动设备
        iphone: false,
        ipod: false,
        ipad: false,
        ios: false,
        android: false,
        nokiaN: false,
        winMobile: false,
        //游戏系统
        wii: false,
        ps: false
    };
    //检测呈现引擎和浏览器
    var ua = navigator.userAgent;
    if (window.opera) {
        engine.ver = browser.ver = window.opera.version();
        engine.opera = browser.opera = parseFloat(engine.ver);
    } else if (/AppleWebKit\/(\S+)/.test(ua)) {
        engine.ver = RegExp["$1"];
        engine.webkit = parseFloat(engine.ver);
        //确定是 Chrome 还是 Safari
        if (/Chrome\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.chrome = parseFloat(browser.ver);
        } else if (/Version\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.safari = parseFloat(browser.ver);
        } else {
            //近似地确定版本号
            var safariVersion = 1;
            if (engine.webkit < 100) {
                safariVersion = 1;
            } else if (engine.webkit < 312) {
                safariVersion = 1.2;
            } else if (engine.webkit < 412) {
                safariVersion = 1.3;
            } else {
                safariVersion = 2;
            }
            browser.safari = browser.ver = safariVersion;
        }
    } else if (/KHTML\/(\S+)/.test(ua) || /Konqueror\/([^;]+)/.test(ua)) {
        engine.ver = browser.ver = RegExp["$1"];
        engine.khtml = browser.konq = parseFloat(engine.ver);
    } else if (/rv:([^\)]+)\) Gecko\/\d{8}/.test(ua)) {
        engine.ver = RegExp["$1"];
        engine.gecko = parseFloat(engine.ver);
        //确定是不是 Firefox
        if (/Firefox\/(\S+)/.test(ua)) {
            browser.ver = RegExp["$1"];
            browser.firefox = parseFloat(browser.ver);
        }
    } else if (/MSIE ([^;]+)/.test(ua)) {
        engine.ver = browser.ver = RegExp["$1"];
        engine.ie = browser.ie = parseFloat(engine.ver);
    }
    //检测浏览器
    browser.ie = engine.ie;
    browser.opera = engine.opera;
    //检测平台
    var p = navigator.platform;
    system.win = p.indexOf("Win") == 0;
    system.mac = p.indexOf("Mac") == 0;
    system.x11 = (p == "X11") || (p.indexOf("Linux") == 0);
    //检测 Windows 操作系统
    if (system.win) {
        if (/Win(?:dows )?([^do]{2})\s?(\d+\.\d+)?/.test(ua)) {
            if (RegExp["$1"] == "NT") {
                switch (RegExp["$2"]) {
                    case "5.0":
                        system.win = "2000";
                        break;
                    case "5.1":
                        system.win = "XP";
                        break;
                    case "6.0":
                        system.win = "Vista";
                        break;
                    case "6.1":
                        system.win = "7";
                        break;
                    default:
                        system.win = "NT";
                        break;
                }
            } else if (RegExp["$1"] == "9x") {
                system.win = "ME";
            } else {
                system.win = RegExp["$1"];
            }
        }
    }
    //移动设备
    system.iphone = ua.indexOf("iPhone") > -1;
    system.ipod = ua.indexOf("iPod") > -1;
    system.ipad = ua.indexOf("iPad") > -1;
    system.nokiaN = ua.indexOf("NokiaN") > -1;
    //windows mobile
    if (system.win == "CE") {
        system.winMobile = system.win;
    } else if (system.win == "Ph") {
        if (/Windows Phone OS (\d+.\d+)/.test(ua)) {;
            system.win = "Phone";
            system.winMobile = parseFloat(RegExp["$1"]);
        }
    }
    //检测 iOS 版本
    if (system.mac && ua.indexOf("Mobile") > -1) {
        if (/CPU (?:iPhone )?OS (\d+_\d+)/.test(ua)) {
            system.ios = parseFloat(RegExp.$1.replace("_", "."));
        } else {
            system.ios = 2; //不能真正检测出来，所以只能猜测
        }
    }
    //检测 Android 版本
    if (/Android (\d+\.\d+)/.test(ua)) {
        system.android = parseFloat(RegExp.$1);
    }
    //游戏系统
    system.wii = ua.indexOf("Wii") > -1;
    system.ps = /playstation/i.test(ua);
    //返回这些对象
    return {
        engine: engine,
        browser: browser,
        system: system
    };
}();
```



