<!-- TOC -->

- [使用默认过渡类名实现动画](#使用默认过渡类名实现动画)
- [自定义动画过渡类名](#自定义动画过渡类名)
- [使用第三方类实现动画](#使用第三方类实现动画)
- [钩子函数实现半场动画](#钩子函数实现半场动画)
- [使用transition-group元素实现列表动画](#使用transition-group元素实现列表动画)

<!-- /TOC -->

# 使用默认过渡类名实现动画

[案例：点击按钮，让h3显示，再点击，让h3隐藏]
- 使用 Vue 官方提供的 transition 元素，把需要被动画控制的元素，包裹起来
- 定义样式
    - v-enter: 进入之前，元素的起始状态，此时还没开始进入 */
    - v-leave-to: 是动画离开之后，离开的终止状态，此时元素动画已经结束
    - v-enter-active: 入场动画时间段
    - v-leave-active: 出场动画时间段
```HTML
<style>
    .v-enter,
    .v-leave-to{
        opacity:0;
    }
    .v-enter-active,
    .v-leave-active{
        transition:all 0.8s ease;
    }
</style>

<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <transition>
        <h3 v-if="flag">这是 h3</h3>
    </transition>
</div>

<script>
    var app = new Vue({
        el:"#app",
        data:{
            flag:false
        }
    })
</script>
```
# 自定义动画过渡类名
```html
<style>
    .my-enter,
    .my-leave-to {
        opacity:0;
        transform: translateY(70px);
    }

    .my-enter-active,
    .my-leave-active{
        transition:all 0.8s ease;
    }
</style>

<div id="app">
    <input type="button" value="toggle" @click="flag=!flag">
    <transition name="my">
        <h6 v-if="flag">这是 h6</h6>
    </transition>
</div>

<script>
    var app = new Vue({
        el:"#app",
        data:{
            flag:false
        }
    })
</script>
```

# 使用第三方类实现动画
- 引入 animated.css ,不需要自己定义样式
```html
<transition enter-active-class="animated bounceIn" leave-active-class="animated bounceOut">
    <h3 v-if="flag">这是 h3</h3>
</transition>

```
或者，将 animated 类写在里面
```html
<transition enter-active-class="bounceIn" leave-active-class="bounceOut" >
    <h3 v-if="flag" class="animated">这是 h3</h3>
</transition>
```
**还可以使用 :duration 设置入场和离场时候的动画时常**
1. 统一设置
```html
<transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="200">
    <h3 v-if="flag" class="animated">这是 h3</h3>
</transition>
```
2. 分开设置(使用对象的方法)
```html
<transition enter-active-class="animated bounceIn" leave-active-class="animated bounceOut" :duration="{enter:200,leave:400}">
    <h3 v-if="flag">这是 h3</h3>
</transition>
```

# 钩子函数实现半场动画
```html
<style>
    .ball{
        width: 20px;
        height: 20px;
        border-radius: 50%;
        background-color: red;
    }
</style>

<div id="app">
    <input type="button" value="快到碗里来" @click="flag=!flag">
    <transition 
        @before-enter="beforeEnter"
        @enter="enter"
        @after-enter="afterEnter">
        <div class="ball" v-show="flag"></div>
    </transition>
</div>

<script>
    var app = new Vue({
        el:"#app",
        data:{
            flag:false
        },
        methods:{
            beforeEnter(el){
                // beforeEnter 表示动画入场之前，此时，动画尚未开始，可以在 beforeEnter 中设置元素开始动画之前的起始样式
                el.style.transform = "translate(0,0)" 
            },
            enter(el,done){
                // 这句话没有实际的作用，如果不写，出不来动画效果
                // 可以认为 el.offsetWidth 会强制动画刷新
                el.offsetWidth
                // enter 表示动画开始之后的样式，这里可以设置小球完成动画之后的结束状态
                el.style.transform = "translate(150px,300px)"
                el.style.transition = "all 1s ease"

                // 当动画完成之后想让它消失，就调用 done()
                // 这里的 done，也就是 afterEnter 这个函数，也就是说：done 是 afterEnter 函数的引用
                done()
            },
            afterEnter(el){
                // 表示动画完成之后，会调用
                this.flag=!this.flag
            }
        }
    })
</script>
```
> **动画钩子函数的第一个参数 el**，表示要执行动画的那个DOM元素，是个**原生的 JS DOM 对象**
> 
> 可以认为 el 是通过 document.getElementById("") 方式获取到的原生 JS 对象
            
> 当只使用 JavaScript过渡的时候，在 **enter** 和 **leave** 中 ，回调函数 **done** 是必须的。否则，他们会被同步调用，过渡会立即完成。


# 使用transition-group元素实现列表动画
- 在实现列表过渡的时候，如果需要过渡的元素，是通过 v-for 循环渲染出来的,不能使用 transition 包裹，需要使用 transitionGroup 
- 如果要为v-for循环创建的元素设置动画，必须为每个元素 设置 ：key属性 

```html
<style>
    li{
        border: 1px dashed #999;
        margin: 5px;
        line-height: 35px;
        padding-left: 20px;
        font-size: 12px;
    }

    li:hover{
        background-color: hotpink;
        transition:all 0.8s ease;
    }

    .v-enter,
    .v-leave-to{
        opacity: 0;
        transform:translateY(80px);
    }
    .v-enter-active,
    .v-leave-active{
        transition:all 1s ease;
    }

    /* 设置删除列表时的动画 */
    /* 下面的 v-move 和 v-leave-active 配合使用，能够实现列表后续的元素，渐渐的飘上来 */
    .v-move{
        transition:all 0.8s ease;
    }
    .v-leave-active{
        position:absolute;
    }
</style>

<div id="app">
    <div>
        <label for="">Id:
            <input type="text" v-model="id">
        </label>

        <label for="">Name:
            <input type="text" v-model="name">
        </label>

        <input type="button" value="添加" @click="add">
    </div>

    <ul>
        <!-- appear 实现页面入场时候的效果 -->
        <transition-group appear>
            <li v-for="(item,i) in list" :key="item.id" @click="del(i)">
                {{ item.id }} --- {{ item.name }}
            </li>
        </transition-group>
    </ul>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var app = new Vue({
        el:"#app",
        data:{
            list:[
                {id:1,name:'赵高'},
                {id:2,name:'秦桧'},
                {id:3,name:'严嵩'},
                {id:4,name:'魏忠贤'}
            ]
        },
        methods:{
            add(){
                this.list.push({id:this.id,name:this.name})
                this.id=this.name=''
            },
            del(i){
                this.list.splice(i,1)
            }
        }
    })
</script>
```
此时 li 的外层是 span
```html
<ul>
    <span>
        <li>
            1---赵高
        </li>
        ···
    </span>
</ul>
```
解决：删除ul标签，在 transition-group 中加入 **tag="ul"**
> 通过为 transition-group 元素，设置 tag 属性，指定transition-group渲染为指定的元素，如果不指定，则默认渲染为 span 标签 

```html
<transition-group appear tag="ul">
    <li v-for="(item,i) in list" :key="item.id" @click="del(i)">
        {{ item.id }} --- {{ item.name }}
    </li>
</transition-group>

```
