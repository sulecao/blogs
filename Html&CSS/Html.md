# [为什么要使用href=”javascript:void(0);”](https://www.cnblogs.com/cyjy/p/6182587.html)



```
href=”javascript:void(0);”这个的含义是，让超链接去执行一个js函数，而不是去跳转到一个地址，而void(0)表示一个空的方法，也就是不执行js函数。
```

## 为什么要使用href=”javascript:void(0);”

javascript:是伪协议，表示url的内容通过javascript执行。void(0)表示不作任何操作，这样会防止链接跳转到其他页面。这么做往往是为了保留链接的样式，但不让链接执行实际操作，

<a href="javascript：void(0)" onClick="window.open()"> 点击链接后，页面不动，只打开链接

<a href="#" onclick="javascript:return false;"> 作用一样，但不同浏览器会有差异。

 

**href=”javascript:void(0);”与href=”#"的区别**

<a href="javascript:void(0)">点击</a>点击链接后不会回到网页顶部 <a href="#">点击</a> 点击后会回到网面顶部

"＃"其实是包含了位置信息，例如默认的锚点是＃top 也就是网页的上端

而javascript:void(0) 仅仅表示一个死链接这就是为什么有的时候页面很长浏览链接明明是＃可是跳动到了页首

而javascript:void(0) 则不是如此所以调用脚本的时候最好用void(0)

 

使用javascript的方法

<a href="#" onclick="javascript:方法;return false;">文字</a>

<a href="javascript:void(0)" onclick="javascript:方法;return false;">文字</a>

 

**补充 <a href="javascript:hanshu();"这样点击a标签就可以执行hanshu()函数了。**



单独设置图片宽高是按等比例调整的，如果要更改比例可以同时设置宽高。

表格

设置表格高度和宽度

设置表格边框

cellspacing 单元格与边框的距离
cellpadding 单元格内容与边框距离
rowspan 用来合并行单元格
<td rowspan="2">B4</td>
colspan用来合并列单元格
<td colspan="2">D3</td>

![1564812232541](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1564812232541.png)

表单

```
<form  method="post"   action="save.php" enctype=">
</form>
input框要加name属性
```
autofocus 自动获得焦点
multiple 多文件上传
autocomplete="on/off" 记录用户上次输入的信息，再次输入时首字母会提示，需要有提交按钮 需要有name属性
required 必填项
accesskey="s" 用户按alt+s光标会跳到对应输入框