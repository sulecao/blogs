#### 创建组件

1.使用 Vue.extend 配合 Vue.component 方法：
```javascript
var login = Vue.extend({
      template: '<h1>登录</h1>'
    });
Vue.component('login', login);
```
2.直接使用 Vue.component 方法：

```javascript
Vue.component('register', {
      template: '<h1>注册</h1>'
    });
```
3.将模板字符串，定义到template标签中：
```vue
<template id='tmpl'>
      <div><a href="#">登录</a> 
</template>
//使用 Vue.component 来绑定模板
Vue.component('account', {
      template: '#tmpl'
    });
```
##### is属性

使用:is属性来切换不同的子组件,is绑定的变量对应的值为组件的名称
通过mode="out-in"设置组件切换时的进出效果。
```vue
<transition mode="out-in">
<component :is="comName"></component>
  </transition>
```
有些 HTML 元素，诸如 <ul>、<ol>、<table> 和 <select>，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 <li>、<tr> 和 <option>，只能出现在其它某些特定的元素内部。is 特性给了我们一个变通的办法。
```vue
<table>
  <tr is="blog-post-row"></tr>
</table>
```
#### 父组件向子组件传值

父组件在子组件元素上绑定一个属性，对应着父组件的数据

```vue
//给 prop 传入一个静态的值，
<blog-post title="My journey with Vue"></blog-post>

//给 prop 传入一个动态的值，如果传的是JavaScript 表达式而不是一个字符串。也需要绑定形式。
<div id="app">
    <div>    
      <child v-bind:message="parentMsg"></child>
    </div>
</div>
```
子组件通过porps获得父组件绑定的属性名。只可读不可改。Props使用对象类型，可以添加额外的验证功能
```vue
Vue.component('child', {
  props: ['message'],
  template: '<span>{{ message }}</span>'
})
```
不应在一个子组件内部改变 prop。
在 JavaScript 中对象和数组是通过引用传入的，所以对于一个数组或对象类型的 prop 来说，在子组件中改变这个对象或数组本身将会影响到父组件的状态。

#### 子组件向父组件传值  

在子组件上绑定一个事件，事件方法里写this.$emit('increment')来触发父组件绑定的事件方法。可以增加参数传递给父组件，因此可以实现子数据传递给父组件。

```vue
Vue.component('button-counter', {
  template: '<button v-on:click="incrementHandler">{{ counter }}</button>',
  data() {
    return {
      counter: 0
    }
  },
  methods: {
    incrementHandler() {
      this.counter += 1
      this.$emit('increment')
    }
  },
})
```
父组件绑定的事件方法incrementTotal会被调用，
```vue
<div id="app">
    <div id="counter-event-example">
      <p>{{ total }}</p>
      <button-counter v-on:increment="incrementTotal"></button-counter>//在组件上绑定事件监听
    </div>
</div>
```
.sync 修饰符：
用来简写子组件给父组件传值，不再需要父组件特意定一个方法接受子组件参数
```vue
<text-document v-bind:title.sync="doc.title"></text-document>
this.$emit('update:title', newTitle)
```

#### 插槽

缩写字符 #，v-slot:header 可以被重写为 #header
实现将父组件里分发内容给子组件

```vue
<navigation-link url="/profile">
  Your Profile
</navigation-link>
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
//当组件渲染的时候，<slot></slot> 将会被替换为“Your Profile”。
```
给插槽命名，以使用多个插槽,没有命名的为默认插槽。
```vue
<!--子组件-->
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
<!--父组件-->
<base-layout>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>
  <p>A paragraph for the main content.</p>
  <p>And another one.</p>
  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</base-layout>
```
注意 v-slot 只能添加在一个 <template> 上

父级模板里的所有内容都是在父级作用域中编译的；子模板里的所有内容都是在子作用域中编译的。可以通过bind向父组件传递数据。
```vue
<!--子组件-->
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
<!--父组件，接收对应插槽default的数据并定义一个名称。-->
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```
解构插槽 Prop
```vue
<current-user v-slot="{ user }">
  {{ user.firstName }}
</current-user>
```
#### keep-alive

```vue
<!-- 失活的组件将会被缓存！切换组件保持页面之前的状态,
注意<keep-alive> 要求被切换到的组件都有自己的名字，
不论是通过组件的 name 选项还是局部/全局注册-->
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```