#### BOM（Browser Object Model）浏览器对象模型

#### window

代表整个浏览器窗口，window也是整个网页的全局对象，所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法

```javascript
window.open("http://www.wrox.com/", "topFrame"); 
//等同于< a href="http://www.wrox.com" target="topFrame"></a> 
window.close();//只能关闭window.open打开的窗口。
```

#### navigator

代表当前浏览器的信息，可以用来检测一些插件

```js
navigator.userAgent //返回一个字符串，包含描述浏览器的信息，可以用来识别不同的浏览器
var ua =navigator.userAgent;
if(/firefox/i.text(ua)){火狐}
else if(/chrome/i.text(ua)){谷歌}
else if(/msie/i.text(ua)){ie}
else if("ActiveXObject" in window){ie11}
```

#### history

代表浏览器历史记录， 由于隐私原因，只可以操作向前向后翻页

```js
history.back();//和浏览器回退按钮一样
history.forward();//前进
history.go(n);//前进n个页面，负数就是向后跳转n个
```

#### location

代表当前浏览器的地址栏信息，可以获取地址栏信息或操作浏览器跳转页面

```js
location="http://www.baidu.com"; //执行后会跳转到指定地址，也可以填相对地址
location.href="http://www.baidu.com"//执行后会跳转到指定地址，效果与location.href相当。
location.assign("http://www.baidu.com")//执行后会跳转到指定地址
location.reload();//刷新当前页面
location.reload(true);//如果传了true会强制从服务器页面，因为浏览器会缓存页面
location.repalce("http://www.baidu.com") //刷新页面，但不记录历史，所以不能回退
```

#### onload

```js
//当页面所有元素创建完成，外部文件下载完毕才会执行。而用script标签在body后不需要等外部文件下载结束。
onload = function(){
  console.log('页面加载完成')
}
//也可以加在标签上
//onunload 当用户卸载文档时执行
```

#### 定时器

```javascript
var time=setInterval(function(){},1000) //每隔1000毫秒执行一次函数，会返回一个数字
clearInterval(time);//关闭对应的time号定时器
```


```javascript
var time=setTimeout(function(){},3000) //延时调用，3000毫秒后调用，只调用一次
clearTimeout(time) //关闭延时调用
```
注意：1.开启定时器前，先关闭定时器，以解决多次开启定时器加速问题

2.为了防止定时器的冲突，最好不要定义全局变量定时器，可以给对象加一个定时器属性，比如 obj.time=setInterval 然后clearInterval(obj.time)

##### 定时器的第三个以后的参数

 定时器启动时候，第三个以后的参数是作为第一个`func()`的参数传进去。

```js
function add(x,y){
  console.log(x+y)
}
setTimeout(add,1000,1,2)//3
```

screen
代表用户屏幕信息，可以获得用户显示器相关信息