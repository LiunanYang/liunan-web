DOM 结构中对放入的 html 元素有限制,像 `<ul>、<ol>、<table>、<select> `里只允许包含指定的元素，像` <option> `这样的元素只能出现在某些特定元素的内部；当模板标签使用在这些有限制性的元素中时，载入前组件还未解析，当前元素发现非指定元素会有排斥反应

定义组件：
```javascript
Vue.component('myTr',{
    template:'<tr>列表</tr>'
})
```
如果直接这样写：
```html
<table>
    <my-tr></my-tr>
    <my-tr></my-tr>
</table>
```
实际渲染出的DOM结构是这样的：
```html
<tr>列表</tr>
<tr>列表</tr>
<table>
</table>
```
改用 `is` 属性引入组件：
```html
<table>
    <tr is="my-tr"></tr>
    <tr is="my-tr"></tr>
</table>
```
实际渲染的DOM结构是：
```html
<table>
    <tbody>
        <tr>列表</tr>
        <tr>列表</tr>
    </tbody>
</table>
```