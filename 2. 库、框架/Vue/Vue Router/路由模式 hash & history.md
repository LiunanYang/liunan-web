## 1.hash
- URL 中 # 后面的内容（location.hash）
- URL 中 hash 值 只是客户端的一个状态，这部分不会被发送到服务器上，不需要服务器做配置
- hash 值的改变，都会在浏览器中增加一个记录。我们可以通过浏览器的回退和前进控制 hash 的切换
- 可以用 hashchange 事件监听hash的改变,当 hash 改变为某个预设值时，通过代码触发对应的DOM改变
```
<body onhashchange="fn()"> //fn 是hash变化后执行的函数
```
**触发 hash 改变**
1. 通过 a 标签，设置 href 属性
```
<a href="#info">信息</a>
```
2. 直接使用 js 对 location.hash 赋值
```
location.hash = "#info"
```
## 2.history
- history 模式改变 url 会导致浏览器向服务器发起请求，如果匹配不到资源，服务器应该始终返回同一个 html 页面

## api
### **切换**
```js
history.go(-2);//后退两次
history.go(2);//前进两次
history.back(); //后退
hsitory.forward(); //前进
```
### **修改历史状态**
- history.pushState() ：增加一个历史记录
- history.replaceState() ：替换当前历史记录
- 这两个 API 可以在不刷新的情况下，操作浏览器的历史记录。

[详解pushState() 和 replaceState()](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API)

## 两种方式的对比
### **url显示**
- hash 模式会有 # 号，路径不直观
- history 模式没有 # 号，路径直观
### **服务器设置**
- hash 不需要对服务器做改动
- history 需要对服务器进行相应的设置
### ****
-
-
### ****
-
-
