#### DOM(document object model)文档对象模型

DOM把html当做一个文档树,文档所有内容都可称为节点,标签称为元素节点，

#### 节点类型

可以通过nodeType判断对应的节点类型。

```
元素节点  console.log(document.body.nodeType) //1
文本节点  3
注释节点  8
```

#### 获取元素节点

```javascript
document.getElementById("btn"); //通过ID获取某个元素，如果没有返回null
[context].getElementsByClassName() //通过class类名获取,返回伪数组
[context].getElementsByTagName() //通过标签名获取,返回伪数组
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
someNode.childNodes //返回元素子节点的NodeList,类数组。空格节点也会有
someNode.children//只返回元素节点
someNode.firstChild //返回当前节点的第一个子节点
someNode.firstElemenChild //返回当前节点的第一个元素子节点
someNode.lastChild //返回当前节点的最后一个子节点
someNode.lastElementChild//返回当前节点的最后一个元素子节点
someNode.parentNode //返回当前节点的父节点
someNode.previousSibling //返回当前节点的前一个兄弟节点
someNode.previousElementChild//返回当前节点的前一个兄弟元素节点
someNode.nextSibling //返回当前节点的后一个兄弟节点
someNode.nextElementChild//返回当前节点的后一个兄弟元素节点
someNode.hasChildNodes()// 判断是否有子节点，返回布尔值
someNode.ownerDocument //表示整个文档的文档节点，即最顶端。
//一个处理如someNode.children兼容性的函数
  function getFirstElementChild(element) {
    var nodes = element.childNodes, i = 0
    var length = nodes.length
    for (var i = 0; i < length; i++) {
      if (nodes[i] === 1) {
        return nodes[i]
      }
    }
    return null
  }
```
#### 创建节点

##### 1.document.write() 不常用

##### 2.innerHTML

当用innerhtml创建很多个元素对象时，如果频繁操作DOM会很消耗性能，最好创建好DOM字符串后一次插入页面。

因为JS的字符串不可修改，每次修改字符串也消耗内存，所以选择创建数组来保存字符串，最后再把数组处理成想要的字符串插入DOM里。可以进一步优化性能。

```js
//向box里插入ul标签，里面有n个li
var box = document.getElementById('box')
var datas = ['数据一','数据二','数据三','数据四']
var arry = []
arry.push('<ul>')
for(var i =0;i<datas.length;i++){
  arry.push('<li>'+datas[i]+'</li>')
}
arry.push('</ul>')
box.innerHTML = arry.join('')
```

注意：使用 innerhtml=''来清空子元素时，如果子元素绑定了事件，会发生内存泄露。最好是遍历子元素删除。

##### 3.createElement()

document.createElement("li") //创建一个li节点

#### 操作节点

```javascript
var li = document.createElement("li")
var text = document.createTextNode("广州")
someNode.appendChild(li);  //插入节点在最后面，如果插入的节点是在页面中的，会先被移除插入到新位置。
someNode.insertBefore(li, bj)  //要插入的节点，参照节点
someNode.replaceChild(li, bj) //要插入的节点，要替换节点
someNoderemoveChild(bj)  //移除节点
someNode.parentNode.removeChild(bj)
city.innerHTML += "<li> 广州 </li>"  
someNode.cloneNode() //接受一个布尔值是否深度复制，true会复制子节点，反之不会。
//操作子父节点时需要注意table里有个隐藏的tbody标签
```
#### 设置样式

```javascript
//设置的效果都是内联样式
box1.style.width = "300px"
box1.style.backgroundColor = "blue"
alert(box1.style.width) //带有单位px
//获取当前生效样式，
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

#### innerHTML与innerText

innerHTML获取内容的时候，如果内容中有标签，会把标签页获取到，空格也保留。

innerText获取内容时，如果内容中有标签，会把标签过滤到，把前后换行和空白都去掉。

通过innerHTML设置内容，如果内容中带标签，标签会被解析后在页面展示。

通过innerText设置内容，如果内容中带标签，在网页上会显示出来标签。

#### 元素属性

除了约定好的属性名，元素是可以自定义属性名和值的。

```js
element.getAttribute() //返回指定属性名的属性值。
element.setAttribute() //设置属性
element.removeAttribute() //移除属性
```

#### 自定义创建节点的函数

```js
 function Node(options) {
    this.className = options.className || '';
    this.id = options.id || '';
    // 节点的名称
    this.nodeName = options.nodeName || '';
    // 节点的类型
    this.nodeType = options.nodeType || 1;
    // 节点的值
    this.nodeValue = options.nodeValue || null;
    // 子节点
    this.children = options.children || [];
  }
```























