#### 构建步骤

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

1. 定义 组件。
```js
const Foo = { template: '<div>foo</div>' }
```
2. 定义路由
```javascript
const routes = [
    { path: '/foo', component: Foo },
]
```
3. 创建 router 实例，然后传 `routes` 配置
```javascript
const router = new VueRouter({
    routes // (缩写) 相当于 routes: routes
})
```
4. 创建和挂载根实例。
```javascript
// 通过 router 配置参数注入路由，从而让整个应用都有路由功能
const app = new Vue({
    router
}).$mount('#app')
```
5.在html里应用，router-link 对应的路由匹配成功，将自动为router-link元素添加 class 属性值 router-link-active，
```vue
<div id="app">
 <router-link to="/foo">Go to Foo</router-link>
  <router-view></router-view>
</div>
```
#### 动态路由
```javascript
routes: [
    // 动态路径参数 以冒号开头,参数值会被设置到 this.$route.params， 可以通过this.$route.params.id获得对应的变量值，可以实现组件页面相同，但是根据传递的id显示不同的数据。
    { path: '/user/:id', component: User }  ]
```
#嵌套路由
例1

```javascript
routes: [
 { path: '/', redirect: '/account/login' }, // 使用 redirect 实现路由重定向
{
path: '/account',
component: account,
children: [ // 通过 children 数组属性，来实现路由的嵌套
 { path: 'login', component: login }, // 注意，子路由的开头位置，不要加 / 路径符
{ path: 'register', component: register }
 ] }]
```
例2
```javascript
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
```
#### 编程式的导航

可以通过 $router 访问路由实例。

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
#### 重定向
```javascript
routes: [
    { path: '/a', redirect: '/b' }
  ]
routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]
```

#### 别名

当用户访问 /b 时，URL 会保持为 /b，但是路由匹配则为 /a，就像用户访问 /a 一样。
```js
routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
```