# mock

- 前后端分离。
- 不需要修改既有代码，就可以拦截 Ajax 请求，返回模拟的响应数据。
- 数据类型丰富。
- 增加单元测试的真实性，通过随机数据，模拟各种场景。

# 使用
- 安装
- 创建mock.js 
- main.js中引入上面创建的mock文档
- 文件中调用mock.js 中模拟的数据接口，response就是mock.js中用Mock.mock("url",data）中设置的data了

# 数据模板的定义规范