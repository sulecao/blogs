#### DOM(document object model)文档对象模型

![1564499829807](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\1564499829807.png)

DOM把html当做一个文档树,文档所有内容都可称为节点,标签称为元素，根元素html,根据节点找到父节点，子节点，兄弟节点

#### 获取节点

```javascript
document.getElementById("btn"); //通过ID获取某个元素，如果没有返回null
[context].getElementsByClassName //通过class类名获取,返回伪数组
[context].getElementsByTagName //通过标签名获取,返回伪数组
document.getElementsByName //通过name获取,返回数组,很少用,所有浏览器兼容,一般用来操作表单元素
document.body //获取整个body对象
document.all //获取所有标签
//下面两个方法都是用document对象来调用，传递一个选择器字符串作为参数，
//会自动根据选择器字符串去网页中查找元素。
document.querySelector('#id')  //只会返回找到的第一个元素
document.querySelectorAll('.class')//返回所有符合条件的元素。返回数组。
```
#### 节点关系

```javascript
someNode.childNodes[0] //表示当前节点的第一个子节点，类数组数据
someNode.firstChild //表示当前节点的第一个子节点
someNode.lastChild //表示当前节点的最后一个子节点
someNode.parentNode //表示当前节点的父节点
someNode.previousSibling //表示当前节点的前一个兄弟节点
someNode.nextSibling //表示当前节点的后一个兄弟节点
someNode.hasChildNodes()// 判断是否有子节点，返回布尔值
someNode.ownerDocument //表示整个文档的文档节点，即最顶端。
```
#### 操作节点

```javascript
var li = document.createElement("li")
var text = document.createTextNode("广州")
someNode.appendChild(li);  //插入节点在最后面
someNode.insertBefore(li, bj)  //要插入的节点，参照节点
someNode.replaceChild(li, bj) //要插入的节点，要替换节点
someNoderemoveChild(bj)  //移除节点
someNode.parentNode.removeChild(bj)
city.innerHTML += "<li> 广州 </li>"  
someNode.cloneNode() //接受一个布尔值是否深度复制，true会复制子节点，反之不会。
//操作子父节点时需要注意table里有个隐藏的tbody标签
```
读取和设置style
 都是内联样式的值

```javascript
box1.style.width = "300px"
box1.style.backgroundColor = "blue"
alert(box1.style.width) //带有单位px
获取当前生效样式，
getComputedStyle传递元素获得一个对象封装了元素的属性，第二个参数用于伪元素
alert(getComputedStyle(box1, null).width)
box1.currentStyle.width （只有ie支持）
```
#### 获取可见宽度和高度

```javascript
box1.clientHeight //元素的客户区大小，包括内容区和padding，不包括边框和margin
box1.clientWidth
box1.offsetHeight //元素垂直方向占用空间大小，包括内容区和padding和border
box1.offsetWidth
box1.offsetLeft  //获取元素左边框和最近的开启定位的祖先元素的水平距离
box1.offsetTop
box1.offsetParent  //获取到最近的开启定位的祖先元素
box1.scrollWidth  //元素的总宽度（包含被隐藏的）
box1.scrollHeigh
box1.scrollLeft  //元素水平滚动条滚动的距离（被隐藏在左侧的像素数）
box1.scrollTop
当scrollHeigh-scrollTop==clientHeight//说明滚动条滚到低了
var st =document.body.scrollTop ||document.documentElement.scrollTop//(对于取得整个页面的滚动条，兼容性处理，后一个支持chrome)
 element.getBoundingClientRect();//获取元素相对于视口的位置，返回一个矩形对象包含bottom，top，left，right，width，height属性。
```