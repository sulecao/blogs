#### 词法作用域
词法作用域是在写代码或者说定义时确定的，而动态作用域是在运行时确定的。

##### JavaScript 中的作用域就是词法作用域 ，但是 this 机制某种程度上很像动态作用域。

```js
  function foo() {
    console.log(a); // 2
  }
  function bar() {
    var a = 3;
    foo();
  }
  var a = 2;
  bar();
```
##### 词法作用域让 foo() 中的 a引用到了全局作用域中的 a，因此会输出 2。

而动态作用域条件下，当 foo() 无法找到 a 的变量引用时，会顺着调用栈在调用 foo() 的地 方查找 a，而不是在嵌套的词法作用域链中向上查找。由于 foo() 是在 bar() 中调用的， 引擎会检查 bar() 的作用域，并在其中找到值为 3 的变量 a。

#### 调用栈和调用位置

```js
function baz() { 
// 当前调用栈是：baz 
// 因此，当前调用位置是全局作用域 
console.log( "baz" ); 
bar(); // <-- bar 的调用位置 
} 
function bar() { 
// 当前调用栈是 baz -> bar 
// 因此，当前调用位置在 baz 中 
console.log( "bar" ); 
foo(); // <-- foo 的调用位置 
} 
function foo() { 
// 当前调用栈是 baz -> bar -> foo 
// 因此，当前调用位置在 bar 中 
console.log( "foo" ); 
}
baz(); // <-- baz 的调用位置
```

#### this

this 是在运行时进行绑定的，并不是在编写时绑定，它的上下文取决于函数调用时的各种条件。this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。 
本质上任何函数在执行时都是通过某个对象调用的,如果没有直接指定就是window。

#### this的绑定规则

#### 默认绑定 

```js
function foo() { 
console.log( this.a ); 
} 
var a = 2; 
foo(); // 2
//foo() 是直接使用不带任何修饰的函数引用进行调用的，因此只能使用默认绑定，this指向全局对象window。 
```

```js
 // 用严格模式（strict mode），那么全局对象将无法使用默认绑定，因此 this 会绑定 到 undefined：
function foo() {
  "use strict";
  console.log(this.a);
}
var a = 2;
foo(); // TypeError: this is undefined
```

#### 隐式绑定

```js
  function foo() {
    console.log(this.a);
  }
  var obj = {
    a: 2,
    foo: foo
  };
  obj.foo(); // 2
//函数foo严格来说不属于 obj 对象。 然而，调用位置会使用 obj 上下文来引用函数
```

```js
 //对象属性引用链中只有最顶层或者说最后一层会影响调用位置。
  function foo() {
    console.log(this.a);
  }
  var obj2 = {
    a: 42,
    foo: foo
  };
  var obj1 = {
    a: 2,
    obj2: obj2
  };
  obj1.obj2.foo(); // 42
```

一个最常见的 this 绑定问题就是被隐式绑定的函数会丢失绑定对象，此时它会应用默认绑定

```js
  function foo() {
    console.log(this.a);
  }
  var obj = {
    a: 2,
    foo: foo
  };
  var bar = obj.foo; // 函数别名！
  var a = "oops, global"; // a 是全局对象的属性 bar(); // "oops, global"
//虽然 bar 是 obj.foo 的一个引用，但是实际上，它引用的是 foo 函数本身，因此此时的 bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。
```
传入回调函数时也会丢失
```js
function foo() { 
console.log( this.a ); 
} 
function doFoo(fn) { 
// fn 其实引用的是 foo 
fn(); // <-- 调用位置！ 
} 
var obj = { 
a: 2, 
foo: foo 
}; 
var a = "oops, global"; // a 是全局对象的属性 
doFoo( obj.foo ); // "oops, global" 
//函数的参数传递是一种隐式赋值，所以也会丢失绑定对象。 

setTimeout( obj.foo, 100 ); // "oops, global" 
//JavaScript 环境中内置的 setTimeout() 函数实现和下面的伪代码类似： 
function setTimeout(fn,delay) { 
// 等待 delay 毫秒 
fn(); // <-- 调用位置！ 
}
```



#### 显式绑定

用call和apply对this绑定对象，它们区别是参数形式
function.apply(Obj,[argArray])
function.call(Obj, arg1, arg2, ...);

bind 和 call/apply 的区别，一个函数被 call/apply 的时候，会直接调用，但是 bind 会创建一个新函数。当这个新函数被调用时，bind() 的第一个参数将作为它运行时的 this，新函数的参数将会和原函数的参数合并成为原函数的参数。

```js
function foo() { 
console.log( this.a ); 
} 
var obj = { 
a:2 
};

foo.call( obj ); // 2
```

如果你把 null 或者 undefined 作为 this 的绑定对象传入 call、apply 或者 bind，这些值在调用时会被忽略，实际应用的是默认绑定规则：



#### new绑定

```js
function foo(a) { 
this.a = a; 
} 
var bar = new foo(2); 
console.log( bar.a ); // 2 
//使用 new 来调用 foo(..) 时，我们会构造一个新对象并把它绑定到 foo(..) 调用中的 this 上。
```
#### 优先级

new绑定>显示绑定>隐式绑定>默认绑定

```js
//要在 new 中使用硬绑定函数，主要目的是预先设置函数的一些参数，这样在使用 new 进行初始化时就可以只传入其余的参数。
function foo(p1,p2) {
this.val = p1 + p2; 
}
var bar = foo.bind( null, "p1" );
var baz = new bar( "p2" ); baz.val; // p1p2
```



#### 箭头函数

ES6 中的箭头函数引入了一个叫作 this 词法的行为： 

箭头函数最常用于回调函数中，例如事件处理器或者定时器：

```js
  var a = 3;
  var obj = { a: 2 };
  function foo() {
    setTimeout(() => {
      console.log(this.a);
    }, 100); // 这里的 this 在词法上继承自 foo() 
  }
   foo.call(obj); // 2

  function poo() {
    setTimeout(function() {
      console.log(this.a);
    }, 100); // 这里丢失了绑定，默认绑定到window上 
  }
  poo.call(obj); // 3
```

