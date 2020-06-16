# webpack-dev-server 

实现自动打包编译的功能

1. 运行 npm i webpack-dev-server -D 把这个工具安装到项目的本地开发依赖
2. 安装完毕后，这个工具的用法和webpack 命令的用法，完全一致
3. 由于我们是在项目中，本地安装的 webpack-dev-serevr ,所以，无法把它当作脚本命令，在 powershell 终端中直接运行；（只有那些安装到全局 -g 的工具，才能在终端中正常执行）
4. 注意：webpack-dev-server 这个工具，如果想要正常运行，要求，在本地项目中，必须安装 webpack
5. webpack-dev-server 帮我们打包生成的 bundle.js 文件，并没有存放到实际的物理磁盘上；而是，直接托管到了电脑的内存中，所以，我们在项目根目录中，根本找不到这个打包好的 bundle.js
6. 我们可以认为，webpack-dev-server 把打包好的文件，以一种虚拟的形式，托管到了咱们项目的根目录中，虽然我们看不到它，但是可以以为，和 dist、src、node_modules 平级，有一个看不见的文件，叫做bundle.js