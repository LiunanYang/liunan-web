

## Node（后端）中的MVC 与 前端中的MVVM之间的区别
- MVC是后端的分层开发概念
- MVVM是前端视图层的分层开发概念，主要关注于视图层分离，也就是说：MVVM把前端的视图层分成了三部分 -> Model，View，VM ViewModel。
    - VM 是 MVVM 思想的核心，因为 VM 是 M 和 V 之间的调度者

## Vue.js基本代码 和 MVVM之间的对应关系
```
<!-- 将来new的Vue实例会控制这个元素中的所有内容 -->
<!-- Vue 实例所控制的元素区域，就是 MVVM 中的 V-->
<div id="app">
    <p>{{msg}}</p>
</div>

<script>
    // 当我们导入包之后，在浏览器的内存中，就多了一个Vue构造函数
    // 注意：我们new出来的这个vm对象，就是MVVM 中的VM 调度者
    var vm = new Vue({
        // 表示当前我们 new 的这个 Vue 实例，要控制页面上的哪个区域
        el:'#app',
        // data 属性中，存放的是 el 中要用到的数据
        // 这里的data 就是 MVVM 中的M
        data:{
            msg:'欢迎学习Vue'
        }
    })
</script>
```
> 前端Vue之类的框架，不提倡我们去手动操作DOM元素了。