history 对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。因为 history 是 window对象的属性，因此每个浏览器窗口、每个标签页乃至每个框架，都有自己的 history 对象与特定的window 对象关联。
使用go()方法可以在用户的实例记录中任意跳转。
```js
//后退一页
history.go(-1);
//前进一页
history.go(1);
//前进两页
history.go(2);
```
也可以给 go()方法传递一个字符串参数，此时浏览器会跳转到历史记录中包含该字符串的第一个
位置——可能后退，也可能前进，具体要看哪个位置最近。
```js
//跳转到最近的 wrox.com 页面
history.go("wrox.com");
//跳转到最近的 nczonline.net 页面
history.go("nczonline.net");
```
还可以使用两个简写方法 back()和 forward()来代替 go()。
```js
//后退一页
history.back();
//前进一页
history.forward();
```
history 对象还有一个 length 属性，保存着历史记录的数量。这个数量包括所有历史记录，即所有向后和向前的记录。对于加载到窗口、标签页或框架中的第一个页面而言，history.length 等于 0。
```js
if (history.length == 0){
    //这应该是用户打开窗口后的第一个页面
}
```