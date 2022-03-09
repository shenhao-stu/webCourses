# Vue3

## 前端工程化与webpack

### webpack的基本使用

#### 1.什么是webpack

概念：webpack 是前端项目工程化的具体解决方案。

主要功能：它提供了友好的前端模块化开发支持，以及代码压缩混淆、处理浏览器端 JavaScript 的兼容性、性能优化等强大的功能。

好处：让程序员把工作的重心放到具体功能的实现上，提高了前端开发效率和项目的可维护性。

注意：目前企业级的前端项目开发中，绝大多数的项目都是基于 webpack 进行打包构建的。

#### 2.创建列表隔行变色项目
① 新建项目空白目录，并运行 `npm init –y` 命令，初始化包管理配置文件 package.json
② 新建 src 源代码目录
③ 新建 src -> index.html 首页和 src -> index.js 脚本文件
④ 初始化首页基本的结构
⑤ 运行 npm install jquery –S 命令，安装 jQuery
⑥ 通过 ES6 模块化的方式导入 jQuery，实现列表隔行变色效果

#### 3.在项目中安装 webpack

在终端运行如下的命令，安装 webpack 相关的两个包：

```bash
npm install webpack@5.5.1 webpack-cli@4.2.0 -D
```

#### 4.在项目中配置 webpack

① 在项目根目录中，创建名为 webpack.config.js 的 webpack 配置文件，并初始化如下的基本配置：

```javascript
module.exports = {
  mode: 'development' // mode 用来指定构建模式。可选值有 development 和 production
}
```

② 在 package.json 的 scripts 节点下，新增 dev 脚本如下：

```javascript
"scripts": {
  "dev": "webpack" // script 节点下的脚本，可以通过 npm run 执行。例如 npm run dev
}
```

③ 在终端中运行 `npm run dev` 命令，启动 webpack 进行项目的打包构建

##### 4.1 mode的可选值

mode 节点的可选值有两个，分别是：

① `development` 

- ==开发环境==
- 不会对打包生成的文件进行代码压缩和性能优化
- 打包速度快，适合在开发阶段使用

② `production` 

- ==生产环境==
- 会对打包生成的文件进行代码压缩和性能优化
- 打包速度很慢，仅适合在项目发布阶段使用

##### 4.2 webpack.config.js 文件的作用

webpack.config.js 是 webpack 的配置文件。webpack 在真正开始打包构建之前，会先读取这个配置文件，从而基于给定的配置，对项目进行打包。

注意：由于 webpack 是基于 node.js 开发出来的打包工具，因此在它的配置文件中，支持使用 node.js 相关的语法和模块进行 webpack 的个性化配置。

##### 4.3 webpack 中的默认约定

在 webpack 中有如下的默认约定： 

① 默认的打包入口文件为 src -> index.js

② 默认的输出文件路径为 dist -> main.js

注意：可以在 webpack.config.js 中修改打包的默认约定

##### 4.4 自定义打包的入口与出口

在 webpack.config.js 配置文件中，通过 entry 节点指定打包的入口。通过 output 节点指定打包的出口。

示例代码如下：

```javascript
const path = require('path')

module.exports = {
  mode: 'development', // development  production
  // 指定打包的入口
  entry: path.join(__dirname, './src/index.js'),
  // 指定打包的出口
  output: {
    // 表示输出文件的存放路径
    path: path.join(__dirname, './dist'),
    // 表示输出文件的名称
    filename: 'js/bundle.js',
  }
}
```

### webpack 中的插件

#### 1. webpack 插件的作用

通过安装和配置第三方的插件，可以拓展 webpack 的能力，从而让 webpack 用起来更方便。最常用的

webpack 插件有如下两个：

① `webpack-dev-server` 

- 类似于 node.js 阶段用到的 nodemon 工具
- ==每当修改了源代码，webpack 会自动进行项目的打包和构建==

② `html-webpack-plugin`

- webpack 中的 HTML 插件（类似于一个模板引擎插件）
- 可以通过此插件自定制 index.html 页面的内容

#### 2. webpack-dev-server

`webpack-dev-server` 可以让 `webpack` 监听项目源代码的变化，从而进行自动打包构建。

##### 2.1 安装 webpack-dev-server

运行如下的命令，即可在项目中安装此插件：

```bash
npm install webpack-dev-serve@3.11.0 -D
```

##### 2.2 配置 webpack-dev-server

① 修改 package.json -> scripts 中的 dev 命令如下：

```javascript
"scripts": {
  "dev": "webpack serve" // script 节点下的脚本，可以通过 npm run 执行。例如 npm run dev
}
```

② 再次运行 `npm run dev` 命令，重新进行项目的打包

③ 在浏览器中访问 http://localhost:8080 地址，查看自动打包效果

> **注意**：webpack-dev-server 会启动一个实时打包的 http 服务器

##### 2.3 打包生成的文件哪儿去了？

① 不配置 webpack-dev-server 的情况下，webpack 打包生成的文件，会存放到实际的物理磁盘上

- 严格遵守开发者在 webpack.config.js 中指定配置
- 根据 output 节点指定路径进行存放

② 配置了 webpack-dev-server 之后，打包生成的文件存放到了内存中

- 不再根据 output 节点指定的路径，存放到实际的物理磁盘上

- 提高了实时打包输出的性能，因为内存比物理磁盘速度快很多

##### 2.4 生成到内存中的文件该如何访问？

webpack-dev-server 生成到内存中的文件，默认放到了项目的根目录中，而且是虚拟的、不可见的。

- http://localhost:8080/bundle.js

- 可以直接用 / 表示项目根目录，后面跟上要访问的文件名称，即可访问内存中的文件

- 例如 /bundle.js 就表示要访问 webpack-dev-server 生成到内存中的 bundle.js 文件

#### 3.html-webpack-plugin

html-webpack-plugin 是 webpack 中的 HTML 插件，可以通过此插件自定制 index.html 页面的内容。

需求：通过 html-webpack-plugin 插件，将 src 目录下的 index.html 首页，复制到项目根目录中一份！

##### 3.1 安装 html-webpack-plugin

运行如下的命令，即可在项目中安装此插件：

```bash
npm install html-webpack-plugin@4.5.0 -D
```

##### 3.2 配置 html-webpack-plugin

```javascript
// 1. 导入插件，得到构造函数
const HtmlPlugin = require('html-webpack-plugin')

// 2. 创建插件的实例对象
const htmlPlugin = new HtmlPlugin({
  template: './src/index.html', // 指定原文件的存放路径
  filename: './index.html', // 指定生成的文件的存放路径
})

module.exports = {
  mode: 'development',
  plugins: [htmlPlugin], // 3. 通过 plugins 节点，使 htmlPlugin 插件生效
}
```

##### 3.3 解惑 html-webpack-plugin

① 通过 HTML 插件复制到项目根目录中的 index.html 页面，也被放到了内存中

② HTML 插件在生成的 index.html 页面的底部，自动注入了打包的 bundle.js 文件

#### 4. devServer 节点

在 webpack.config.js 配置文件中，可以通过 devServer 节点对 webpack-dev-server 插件进行更多的配置，

示例代码如下：

```javascript
devServer: {
  open: true,         // 初步打包完成后，自动打开浏览器
  host: '127.0.0.1',  // 实时打包所使用的主机地址
  port: 80,           // 实时打包所使用的端口号
},
```

> **注意**：凡是修改了 webpack.config.js 配置文件，或修改了 package.json 配置文件，必须重启实时打包的服务器，否则最新的配置文件无法生效！

### webpack 中的 loader

#### 1.loader 概述

在实际开发过程中，webpack 默认只能打包处理以 .js 后缀名结尾的模块。其他非 .js 后缀名结尾的模块，webpack 默认处理不了，需要调用 loader 加载器才可以正常打包，否则会报错！

loader 加载器的作用：协助 webpack 打包处理特定的文件模块。比如：

- css-loader 可以打包处理 .css 相关的文件

- less-loader 可以打包处理 .less 相关的文件

- babel-loader 可以打包处理 webpack 无法处理的高级 JS 语法

#### 2. loader 的调用过程

<img src="README.assets/webpack.png" style="zoom:50%;" />

#### 3.打包处理 css 文件

① 运行 `npm i style-loader@2.0.0 css-loader@5.0.1 -D` 命令，安装处理 css 文件的 loader

② 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

```javascript
module: { // 所有第三方文件模块的匹配规则
    rules: [ // 文件后缀名的匹配规则
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }
    ]
}
```

其中，test 表示匹配的文件类型（.css$表示以.css结尾）， use 表示对应要调用的 loader

> **注意**：
>
> - use 数组中指定的 loader 顺序是固定的
> - 多个 loader 的调用顺序是：从后往前调用

#### 4.打包处理 less 文件

① 运行 `npm i less-loader@7.1.0 less@3.12.2 -D` 命令

② 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

```javascript
module: { // 所有第三方文件模块的匹配规则
    rules: [ // 文件后缀名的匹配规则
      { test: /\.less$/, use: ['style-loader', 'css-loader', 'less-loader'] }
    ]
}
```

#### 5.打包处理样式表中与 url 路径相关的文件

① 运行 `npm i url-loader@4.1.1 file-loader@6.2.0 -D` 命令

② 在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

```javascript
module: { // 所有第三方文件模块的匹配规则
    rules: [ // 文件后缀名的匹配规则
      { test: /\.jpg|png|gif$/, use: 'url-loader?limit=22228' }
    ]
}
```

其中 ? 之后的是 loader 的参数项：

- limit 用来指定图片的大小，单位是字节（byte）
- 只有 ≤ limit 大小的图片，才会被转为 base64 格式的图片

##### 5.1 loader 的另一种配置方式

带参数项的 loader 还可以通过对象的方式进行配置：

```javascript
module: { // 所有第三方文件模块的匹配规则
    rules: [ // 文件后缀名的匹配规则
      {
        test: /\.jpg|png|gif$/, // 匹配图片文字
        use: {
          loader: 'url-loader', // 通过 loader 属性指定要调用的 loader
          options: {            // 通过 options 属性指定参数项
            limit: 22228,
            outputPath: 'image'
          }
        }
      }
    ]
}
```

#### 6.打包处理 js 文件中的高级语法

webpack 只能打包处理一部分高级的 JavaScript 语法。对于那些 webpack 无法处理的高级 js 语法，需要借 助于 babel-loader 进行打包处理。例如 webpack 无法处理下面的 JavaScript 代码：

```javascript
class Person {
  // 通过 static 关键字，为 Person 类定义了一个静态属性 info
  // webpack 无法打包处理“静态属性”这个高级语法
  static info = 'person info'
}

console.log(Persoo.info)
```

##### 6.1 安装 babel-loader 相关的包

运行如下的命令安装对应的依赖包：

```bash
npm i babel-loader@8.2.1 @babel/core@7.12.3 @babel/plugin-proposal-class-properties@7.12.1 -D
```

##### 6.2 配置 babel-loader

在 webpack.config.js 的 module -> rules 数组中，添加 loader 规则如下：

```javascript
{
  test: /\.js$/,
  // exclude 为排除项，
  // 表示 babel-loader 只需要处理开发者编写的 js 文件，不需要处理 node_modules 下的 js 文件
  exclude: /node_modules/,
  use: {
    loader: 'babel-loader',
    options: { // 参数项
      // 声明一个 babel 插件，此插件用来转化 class 中的高级语法
      plugins: ['@babel/plugin-proposal-class-properties']
    }
  }
}
```

### 打包发布

#### 1.为什么要打包发布

项目开发完成之后，使用 webpack 对项目进行打包发布的主要原因有以下两点：

① 开发环境下，打包生成的文件存放于内存中，无法获取到最终打包生成的文件

② 开发环境下，打包生成的文件不会进行代码压缩和性能优化

**为了让项目能够在生产环境中高性能的运行，因此需要对项目进行打包发布。**

#### 2.配置 webpack 的打包发布

在 package.json 文件的 scripts 节点下，新增 build 命令如下：

```javascript
"scripts": {
  "dev": "webpack serve", // 开发环境中，运行 dev 命令
  "build": "webpack --mode production" // 项目发布时，运行 build 命令
}
```

--model 是一个参数项，用来指定 webpack 的运行模式。production 代表生产环境，会对打包生成的文件 进行代码压缩和性能优化。

> **注意**：通过 --model 指定的参数项，会**覆盖** webpack.config.js 中的 model 选项。

#### 3.把 JavaScript 文件统一生成到 js 目录中

在 webpack.config.js 配置文件的 output 节点中，进行如下的配置：

<img src="README.assets/js.png" alt="js" style="zoom:50%;" />

```javascript
output: {
  path: path.join(_dirname,'dist'),
  // 明确告诉 webpack 把生成的 bundle.js 文件存放到 dist 目录下的 js 子目录中
  filename: 'js/bundle.js',
}
```

#### 4. 把图片文件统一生成到 image 目录中

修改 webpack.config.js 中的 url-loader 配置项，新增 outputPath 选项即可指定图片文件的输出路径：

<img src="README.assets/img.png" alt="img" style="zoom:50%;" />

```javascript
module: { // 所有第三方文件模块的匹配规则
    rules: [ // 文件后缀名的匹配规则
      {
        test: /\.jpg|png|gif$/, // 匹配图片文字
        use: {
          loader: 'url-loader', // 通过 loader 属性指定要调用的 loader
          options: {            // 通过 options 属性指定参数项
            limit: 22228,
            // 明确指定把打包生成的图片文件，存储到 dist 目录下的 image 文件夹中
            outputPath: 'image'
          }
        }
      }
    ]
}
```

#### 5.自动清理 dist 目录下的旧文件

为了在每次打包发布时自动清理掉 dist 目录中的旧文件，可以安装并配置 clean-webpack-plugin 插件：

<img src="README.assets/clean-webpack.png" alt="clean-webpack" style="zoom:50%;" />

```javascript
// 1.安装清理 dist 目录的 webpack 插件
npm install clean-webpack-plugin@3.0.0 -D

// 2.按需导入插件、得到插件的构造函数之后，创建插件额度实例对象
const { CleanWebpackPlugin }= require('clean-webpack-plugin')
const cleanPlugin = new CleanWebpackPlugin()

// 3.把创建的 cleanPlugin 插件实例对象，挂载到 plugins 节点中
plugins: [htmlPlugin, cleanPlugin], // 挂载插件
```

#### 6.企业级项目的打包发布

企业级的项目在进行打包发布时，远比刚才的方式要复杂的多，主要的发布流程如下： 

- 生成打包报告，根据报告分析具体的优化方案
- Tree-Shaking
- 为第三方库启用 CDN 加载
- 配置组件的按需加载
- 开启路由懒加载
- 自定制首页内容

在后面的 vue 项目课程中，会专门讲解如何进行企业级项目的打包发布。
