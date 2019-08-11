#### Set结构

它类似于数组，但是成员的值都是唯一的，向 Set 加入值的时候，不会发生类型转换，所以5和"5"是两个不同的值。(需注意NaN在这儿是被认为重复的，两个对象总是不等的，即使都是空对象)

##### 创建方法

```js
const s = new Set(); 
```
##### 基础方法

```js
s.add(value)：//添加某个值，返回 Set 结构本身。
s.delete(value)：//删除某个值，返回一个布尔值，表示删除是否成功。
s.has(value)：//返回一个布尔值，表示该值是否为Set的成员。
s.clear()：//清除所有成员，没有返回值。
```
##### 遍历方法

set里键名和键值被认为一样的

```js
s.keys()：//返回键名的遍历器
s.values()：//返回键值的遍历器
s.entries()：//返回键值对的遍历器
//可以通过上面s按个方法获得遍历器再for of循环，但实际可以直接循环set数组获得每个值
for (let x of s) {
  console.log(x);
}
s.forEach()：//使用回调函数遍历每个成员
```
#### Map

传统上只能用字符串当作键。Object 结构提供了“字符串—值”的对应，但Map 结构提供了“值—值”的对应，键也可以各种类型的值不再只能是字符串。

```js
const m= new Map();
m.set(hello, 'Hello ES6!') 
m.get(hello)
m.has(hello)  
m.delete(hello)
map.clear()
```
#### WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。
WeakSet 与 Set 有两个区别。
1.WeakSet 的成员只能是对象，而不能是其他类型的值。
2.WeakSet 中的对象都是弱引用，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。

#### WeakMap

与Map结构类似，也是用于生成键值对的集合。
WeakMap与Map有两个区别。
1.WeakMap只接受对象作为键名（null除外），不接受其他类型的值作为键名。
2.WeakMap的键名所指向的对象都是弱引用，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。