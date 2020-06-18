# 使用

页面中：
- 使用 `router-link`组件来导航
    - 通过`to`属性指定链接
    - `router-link`默认渲染成`a`标签,可以通过`tag`属性自己指定标签
    - 当 <router-link> 对应的路由匹配成功，将自动设置 class 属性值 .router-link-active
- 路由出口：`router-view`组件
    - 路由匹配到的组件将渲染在这里

JS中：

### 1.Vue.use(VueRouter)
如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)
### 2.定义路由组件
```javascript
const Foo = { template: '<div>foo</div>' }
```
### 3.定义路由
- 每个路由应该映射一个组件，"component"可以是通过 Vue.extend() 创建的组件构造器或者是一个组件配置对象
```javascript
const routes = [
    {path:"/foo",component:Foo}
]
```
### 4.创建 router 实例，传 `routes` 配置

```javascript
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})
```

### 5.创建和挂载根实例。
```javascript
const app = new Vue({
  router
}).$mount('#app')
```

## 路由器
可以在组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由 
