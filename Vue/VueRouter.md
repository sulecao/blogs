#### 构建步骤

```js
//router/index.js
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
import foo from './../components/foo'
import bar from './../components/bar'
const routes = [
    {
        path: '/foo',
        component: foo
    },
    {
        path: '/bar',
        component: bar
    }
]
const router = new VueRouter({
    routes
})
export default router
```
```js
//main.js
import Vue from 'vue'
import App from './App.vue'
import router from './router/index'
new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```
```vue
//app.vue
<template>
  <div id="app">
    <router-link to='/foo'>foo</router-link>
    <router-link to='/bar'>bar</router-link>
    <router-view></router-view>
  </div>
</template>
```
#### 动态路由
```javascript
//router/index.js
const routes = [
    // 动态路径参数 以冒号开头,参数值会被设置到 this.$route.params， 可以通过this.$route.params.id获得对应的变量值，可以实现组件页面相同，但是根据传递的id显示不同的数据。
    { path: '/user/:id', component: User }  ]
```
```vue
<template>
  <div id="app">
    <router-link :to='/user/+id'>User</router-link>
    <router-view></router-view>
  </div>
</template>
```
```vue
//User.vue
//通过$route.params.id来获得对应的id，以展示对应的内容
<template>
  <div>
      {{$route.params.id}}
  </div>
</template>
```



#### 嵌套路由

例1

```javascript
//router/index.js
const routes = [
    { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
    {
        path: '/account',
        component: account,
        children: [ // 通过 children 数组属性，来实现路由的嵌套
            { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符
            { path: 'register', component: register }
        ]
    }]
```
#### 编程式的导航

可以通过 $router 访问路由实例。以实现用js来控制路由的跳转

1.this.$router.push：这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，则回到之前的 URL。其实点击 <router-link :to="..."> 等同于调用 router.push(...)。

2.this.$router.replace：它不会向 history 添加新记录，而是跟它的方法名一样 —— 替换掉当前的 history 记录。

3.this.$router.go(n)： 在 history 记录中向前或者后退多少步

#### 命名路由

在 routes 配置中给某个路由设置名称。

```javascript
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})
```
要链接到一个命名路由
可以给 router-link 的 to 属性传一个对象：

```vue
<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>
```
也可以调用 router.push()：
```js
router.push({ name: 'user', params: { userId: 123 }})
```
这两种方式都会把路由导航到 /user/123 路径。

#### 命名视图

同时展现多个视图时使用
router-view 没有设置名字，那么默认为 default

```vue
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```
#### 
#### 别名

当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
```js
routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
```

随着ajax的出现，有了前后端分离的开发模式，后端负责数据处理和提供接口，前端通过接口获取数据展示在页面上，前端路由实现了改变url但是页面不进行整体的刷新。



history和hash两种模式

location.hash = 'aaa'

histor.pushState({},'','home')

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

```js
const router = new VueRouter({
  mode: 'history',//使用history模式
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持

# 懒加载

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了。

```js
const Foo = () => import('./Foo.vue')
```

```js
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo }
  ]
})
```

## 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

# 导航守卫

## 全局前置守卫

```js
const router = new VueRouter({ ... })
router.beforeEach((to, from, next) => {
  // ...
})
```

## 全局解析守卫

> 2.5.0 新增

在 2.5.0+ 你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，区别是在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后**，解析守卫就被调用。

## 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

```js
router.afterEach((to, from) => {
  // ...
})
```

## 路由独享的守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

## 组件内的守卫

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // just use `this`
  this.name = to.params.name
  next()
}
```

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from , next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

#### keep-alive

```vue
<keep-alive exclude="user">
  <router-view></router-view>
</keep-alive>
```

activated与deactived