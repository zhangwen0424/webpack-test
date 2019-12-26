# webapck
[原文链接](https://segmentfault.com/a/1190000006178770)

## 安装
//生成package.json
npm init
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack

## webpack的使用
### webpack可以在终端中使用
在基本的使用方法如下：
```
# {extry file}出填写入口文件的路径，本文中就是上述main.js的路径，
# {destination for bundled file}处填写打包文件的存放路径
# 填写路径的时候不用添加{}
webpack {entry file} {destination for bundled file}
指定入口文件后，webpack将自动识别项目所依赖的其它文件，不过需要注意的是如果你的webpack不是全局安装的，那么当你在终端中使用此命令时，需要额外指定其在node_modules中的地址，继续上面的例子，在终端中输入如下命令

# webpack非全局安装的情况
node_modules/.bin/webpack app/main.js public/bundle.js
```

### 配置文件来使用Webpack
根目录下新建一个名为webpack.config.js,简单配置代码，目前的配置主要涉及到的内容是入口文件路径和打包后文件的存放路径。有了这个配置之后，再打包文件，只需在终端里运行webpack(非全局安装需使用node_modules/.bin/webpack)命令就可以了，这条命令会自动引用webpack.config.js文件中的配置选项
```
module.exports = {
  entry:  __dirname + "/app/main.js",//已多次提及的唯一入口文件
  output: {
    path: __dirname + "/public",//打包后的文件存放的地方
    filename: "bundle.js"//打包后输出文件的文件名
  }
}
注：“__dirname”是node.js中的一个全局变量，它指向当前执行脚本所在的目录。
```

### npm使用webpack
npm可以引导任务执行，对npm进行配置后可以在命令行中使用简单的npm start命令来替代上面略微繁琐的命令。在package.json中对scripts对象进行相关设置即可，设置方法如下。
```
{
  "name": "webpack-sample-project",
  "version": "1.0.0",
  "description": "Sample webpack project",
  "scripts": {
    "start": "webpack" // 修改的是这里，JSON文件不支持注释，引用时请清除
  },
  "author": "zhang",
  "license": "ISC",
  "devDependencies": {
    "webpack": "3.10.0"
  }
}
```
npm的start命令是一个特殊的脚本名称，其特殊性表现在，在命令行中使用npm start就可以执行其对于的命令，如果对应的此脚本名称不是start，想要在命令行中运行时，需要这样用npm run {script name}如npm run build，我们在命令行中输入npm start试试

### 生成Source Maps（使调试更容易）
在webpack的配置文件中配置source maps，需要配置devtool
devtool选项	配置结果
***source-map***	在一个单独的文件中产生一个完整且功能完全的文件。这个文件具有最好的source map，但是它会减慢打包速度；
***cheap-module-source-map***	在一个单独的文件中生成一个不带列映射的map，不带列映射提高了打包速度，但是也使得浏览器开发者工具只能对应到具体的行，不能对应到具体的列（符号），会对调试造成不便；
***eval-source-map***	使用eval打包源文件模块，在同一个文件中生成干净的完整的source map。这个选项可以在不影响构建速度的前提下生成完整的sourcemap，但是对打包后输出的JS文件的执行具有性能和安全的隐患。在开发阶段这是一个非常好的选项，在生产阶段则一定不要启用这个选项；
***cheap-module-eval-source-map***	这是在打包文件时最快的生成source map的方法，生成的Source Map 会和打包后的JavaScript文件同行显示，没有列映射，和eval-source-map选项具有相似的缺点；



### 使用webpack构建本地服务器(热更新)
想不想让你的浏览器监听你的代码的修改，并自动刷新显示修改后的结果，其实Webpack提供一个可选的本地开发服务器，这个本地服务器基于node.js构建，可以实现你想要的这些功能，不过它是一个单独的组件，在webpack中进行配置之前需要单独安装它作为项目依赖

npm install --save-dev webpack-dev-serve
将配置加入webpack.config.js
```
devServer: {
  contentBase: "./public",//本地服务器所加载的页面所在的目录
  historyApiFallback: true,//不跳转
  inline: true//实时刷新
} 
```
在package.json中的scripts对象中添加如下命令，用以开启本地服务器：
```
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open"
  },
```
在终端中输入npm run server即可在本地的8080端口查看结果

注 由于webpack3.*/webpack2.*已经内置可处理JSON文件，这里我们无需再添加webpack1.*需要的json-loader。
