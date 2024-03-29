# props 类型
## 数组形式
```
props: ['title', 'likes', 'isPublished', 'commentIds']
```
## 对象形式
```
props: {
  title: String,
  likes: [Number,String], //多个可能的类型
  isPublished: Boolean,
  commentIds: Array,
}
```
## 声明约定
> 对于props声明的属性，在**父组件**中，属性名需要使用**中划线**写法
>
> **子组件**中props属性声明时，使用**小驼峰**或**中划线**都可以,使用的时候要写对应的小驼峰写法。

# 传递静态/动态 props
## 静态
```
<blog-post title="My journey with Vue"></blog-post>
```
但是 如果传入的是数字、布尔值、数组、对象，都必须使用**v-bind**来传递(动态的形式)，告诉 vue 这是一个js表达式，而不是一个字符串

## 动态
```
<blog-post :title="post.title"></blog-post>
```
## 传入一个对象的所有属性
如果想将一个对象的所有属性都作为 prop 传入，可以使用不带参数的 v-bind (取代 v-bind:prop-name)

例：给定对象
```
post: {
  id: 1,
  title: 'My Journey with Vue'
}
```
```
<blog-post v-bind="post"></blog-post>
```
等价于
```
<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
```


# 单向数据流

- 父级 props 的更新会流向子级中，但反过来不行
- 这样可以防止子组件意外的改变父组件的状态，导致项目的数据流向难以理解
- 父组件发生更新时，子组件中的 props 都会刷新为最新的值，你**不**应该在子组件内部改变 props

以下有两种情况试图改变一个 prop
1. prop 来传递一个初始值，子组件希望将其作为一个本地数据来使用
 - 解决方法：定义一个本地的 data 属性，并将这个prop用作其初始值
 ```
props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
 ```
2. 这个 prop 以一个原始的值传入，需要转换
- 解决方法：使用这个 prop 值定义一个计算属性
```
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```
> 注意！在js中 数组和对象是通过引用传入的，对于一个数组或对象类型的 prop ，在子组件中改变它将会影响父组件中的状态。


# prop 验证
为了定制 prop 的验证方式，可以为props 中的值提供一个带有验证需求的对象，而不是一个字符串数组。
```
props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    
    // 多个可能的类型
    propB: [String, Number],
    
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
```


# 组件案例-评论列表
```html
<div id="app">
    <com1 @func="loadComments"></com1>
    <ul>
        <li v-for="i in list">
            {{ i.name }} : {{ i.content }}
        </li>
    </ul>
</div>

<template id="tmp1" >
    <div>
        <label for="">用户名：</label>
        <input type="text" v-model="name">
        
        <label for="">发表内容：</label>
        <textarea name="" id="" cols="30" rows="10" v-model="content"></textarea>
        
        <input type="button" value="发表" @click="add">
    </div>
</template>

<script>
    var app = new Vue({
        el:"#app",
        data:{
            list:[
                {id:Date.now(),name:'李白',content:'天生我材必有用'},
                {id:Date.now(),name:'康康',content:'我爱柳楠'},
                {id:Date.now(),name:'妥妥无',content:'哈哈哈哈哈哈哈哈哈哈'}                ]
        },
        methods:{
            loadComments(){ //从本地的localStorage 中加载评论列表
                var list = JSON.parse( localStorage.getItem('com1')||'[]' )
                this.list = list
            }
        },
        beforeCreate(){
            // 注意：这里不能调用 beforeCreate 方法，因为在执行中这个钩子函数的时候，data 和 methods 都还没有被初始化好
        },
        created(){
            this.loadComments()
        },
        components:{
            com1:{
                template:'#tmp1',
                data(){
                    return {
                        name:'',
                        content:''
                    }
                },
                methods:{
                    add(){
                        var comment = { id:Date.now(), name:this.name, content:this.content }
                        // 从localStorage 中获取所有的评论
                        var list = JSON.parse( localStorage.getItem('com1') || '[]' )
                        list.unshift(comment)
                        localStorage.setItem('com1',JSON.stringify(list))
                        this.name = this.content = ''
                        this.$emit('func')
                    }
                }
            }
        }
    })
</script>
```
**分析：发表评论的业务逻辑**
1. 评论数据存哪里？ 存放到了 localStorage 中
2. 先组织出一个最新的评论数据对象
3. 想办法，把第二部中得到的评论对象，保存到 localStorage 中
    -  localStorage 中只支持存放字符串数据，要调用JSON.stringify
    -  在保存最新的评论数据前，要先从 loaclStorage 获取到之前的评论数据(string)，转换为一个数组对象，然后把最新的评论，push到这个数组
    -  如果获取到的 localStorage 中的评论字符串，为空不存在，则可以返回一个 '[]' ，让JSON。parse 去转换
    -  把最新的评论列表数组再次调用 JSON.stringify ，转为数组字符串，然后调用 loaclStorage.setItem()

