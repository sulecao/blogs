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