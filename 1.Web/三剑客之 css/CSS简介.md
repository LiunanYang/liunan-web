# 引入、定义样式
## link
- link 是 HTML 文件中的标签，在 <head> 标签中引入 CSS 文件。
    - rel
    - href
    - media
- 通过**link标签**引入：可以利用浏览器的缓存，加快用户访问速度，提高用户体验。

将css样式写到外部样式表中->完全使结构与表现分离，可以使样式表在不同的页面中使用。最大限度的使样式表可以进行复用。所以在开发中，我们最推荐的是使用方式就是外部的css文件。
## @import
@import 是 CSS 中的一个@规则，必须先于所有其他类型的规则（@charset 规则除外），结尾需要加分号。@import 用于从其他样式表导入样式规则。
- 因为必须要用 CSS 引擎来解析，所以只能出现在 CSS 文件中或 HTML文件的 < style> 标签中。
- @import url media-queries ;
## 行内样式
## style
    - type 规定样式表的MIME类型，目前只有 text\css
    - media

# 规范
[CSS规范](http://nec.netease.com/standard/css-optimize.html)

# css的注释
作用和HTML的注释类似，只不过它必须编写在style标签中，或者是css文件中。







