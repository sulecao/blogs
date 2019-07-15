#### BOM浏览器对象模型

window
代表整个浏览器窗口，window也是整个网页的全局对象，所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法
navigator
代表当前浏览器的信息，通过对象可以识别不同的浏览器
location
代表当前浏览器的地址栏信息，可以获取地址栏信息或操作浏览器跳转页面
history
代表浏览器历史记录， 由于隐私原因，只可以操作向前向后翻页
screen
代表用户屏幕信息，可以获得用户显示器相关信息

##### window

```javascript
window.open("http://www.wrox.com/", "topFrame"); 
//等同于< a href="http://www.wrox.com" target="topFrame"></a> 
window.close();//只能关闭window.open打开的窗口。
```
##### 定时器

```javascript
var time=setInterval(function(){},1000) //每隔1000毫秒执行一次函数，会返回一个数字
clearInterval(time);//关闭对应的time号定时器
```
开启定时器前，先关闭定时器，以解决多次开启定时器加速问题

```javascript
var time=setTimeout(function(){},3000) //延时调用，3000毫秒后调用，只调用一次
clearTimeout(time) //关闭延时调用
```
为了防止定时器的冲突，最好不要定义全局变量定时器，可以给对象加一个定时器属性，比如 obj.time=setInterval 然后clearInterval(obj.time)

```
确认或取消对话框
confirm("Are you sure?")
```
```
输入对话框，确定返回输入的内容，Cancel 或没有单击OK 而是通过其他方式关闭了对话框，则该方法返回 null
prompt("What's your name?","Michael")
```
#### navigator

可以用来检测一些插件
navigator.userAgent 返回一个字符串，包含描述浏览器的信息
var ua =navigator.userAgent;
if(/firefox/i.text(ua)){火狐}
else if(/chrome/i.text(ua)){谷歌}
else if(/msie/i.text(ua)){ie}
else if("ActiveXObject" in window){ie11}
因为各浏览器的差异，使用能力检测来判断浏览器版本以使用不同的逻辑代码
callback &&callback() 有callback()函数就执行，没有就不执行

#### history

history 获取到当次访问的连接数量
history.back();和浏览器回退按钮一样
history.forward();前进
history.go(n);前进n个页面，负数就是向后跳转n个

#### location

可以解析url
location="http://www.baidu.com"; 修改会跳转到指定地址，也可以填相对地址
location.assign("http://www.baidu.com")
location.reload();刷新当前页面
location.reload(true);如果传了true会强制清空缓存刷新页面
location.repalce("http://www.baidu.com") 也是去新的网页，但不能回退