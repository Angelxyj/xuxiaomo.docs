> 《React课程体系V16版本》课程资料——许小墨

# 一、关于React

英文官网：https://reactjs.org/   ===》   https://react.dev/

中文官网：https://zh-hans.reactjs.org/  ===》https://zh-hans.react.dev/

React 起源于 Facebook 的内部项目，因为该公司对市场上所有 JavaScript MVC 框架，都不满意，就决定自己写一套，用来架设Instagram 的网站。做出来以后，发现这套东西很好用，就在2013年5月开源了。

react在发展过程中，一直跟随原生js的脚步，特别是从v16.0版本开始（用到了class来创建组件）

https://es6.ruanyifeng.com/

2015年推出了使用react来编写移动端的app ---- react-native https://www.react-native.cn/ v0.71

重要版本发版时间

| 序号 | 版本号 | 发版时间       | 重要更新                   |
| ---- | ------ | -------------- | -------------------------- |
| 1    | 16     | 2017年9月26    | 引入es6的类组件            |
| 2    | 16.3   | 2018年4月3日   | 生命周期更新               |
| 3    | 16.4   | 2018年5月23日  | 生命周期更新               |
| 4    | 16.8   | 2019年2月6日   | 引入 react hooks           |
| 5    | 17.0   | 2020年10月20日 | 过渡版本                   |
| 6    | 18.0   | 2022年3月29日  | 写法改变，严格模式发生改变 |

AMD CMD commonjs es6模块化

# 二、脚手架

英文官网：https://create-react-app.dev/

中文官网：https://create-react-app.bootcss.com/

yarn npm  cnpm pnpm

> 配置cnpm
>
> ```bash
>  npm install -g cnpm --registry=https://registry.npmmirror.com
> ```
>
> 以后就可以使用 cnpm 代替 npm

> 补充：react的脚手架并不是只有 `create-react-app`，还有`dva-cli`,`umi`等
>
> vite

```sh
$ npm init vite@latest
# 或者
$ yarn create vite
# 或者
$ pnpm create vite
```

> `vue-cli`以及`create-react-app`底层都是基于`nodejs`的 `webpack`  

## 2.1 create-react-app脚手架的使用

> ```bash
> npm install --global smart-npm --registry=https://registry.npm.taobao.org/
> ```
>
> 第一次通过create-react-app 或者 通过webstorm等编译器创建创建react项目时，因为要安装很多相关的包，而这些包的安装基本都是在内部通过npm进行安装。 由于npm在访问有些国外的资源会非常卡，有的时候即使开了VPN也一样很卡，因此这个时候创建过程可能会异常缓慢，这里其实我们可以使用国内的数据源。
>  	如果 window 用户安装最新版本不成功的话，可以试试安装 smart-npm@1
>
> ```bash
> npm install --global smart-npm@1 --registry=https://registry.npm.taobao.org/
> ```

> Create React App 让你仅通过一行命令，即可构建现代化的 Web 应用。
>
> 本文档之后称之为cra

创建项目的方式:

> 需要保证电脑安装node版本在14以上，系统在win7以上

```sh
# 方式1:使用npx
$ npx create-react-app react-basic
# 等价于
$ npm i create-react-app -g #  npm 使用cnpm 代替 cnpm i create-react-app -g
$ create-react-app react-basic
```

如果需要使用ts开发项目，创建项目时可以通过`--template typescript`指定模版

```sh
$ npx create-react-app myapp --template typescript
```

如果出现如下内容，即代表项目创建成功

```sh
Success! Created react-model-app at E:\workspace\bj2304\code\react-course\react-model-app
    Removes this tool and copies build dependencies, configuration files
    and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

  cd react-model-app
  npm start
```



## 2.2 项目目录解析

项目创建完毕生成目录结构如下：

```sh
react-basic
├── README.md
├── node_modules
├── package.json
├── .gitignore
├── public
│   ├── favicon.ico
│   ├── index.html
│   ├── logo192.png
│   ├── logo512.png
│   ├── manifest.json
│   └── robots.txt
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    ├── index.js
    ├── logo.svg
    ├── reportWebVitals.js // 做性能测试
    └── setupTests.js // 测试
```

`src/reportWebVitals.js`

```js
const reportWebVitals = onPerfEntry => {
  if (onPerfEntry && onPerfEntry instanceof Function) {
    import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
      getCLS(onPerfEntry); // 衡量视觉稳定性。为了提供一个好的用户体验，CLS应该小于0.1
      getFID(onPerfEntry); // 衡量可交互性。为了提供一个好的用户体验，FID应该在100毫秒内。
      getFCP(onPerfEntry); // 首次内容绘制
      getLCP(onPerfEntry); // 衡量加载性能。为了提供一个好的用户体验，LCP应该在2.5秒内
      getTTFB(onPerfEntry); // 到第一个字节的时间
    });
  }
};
export default reportWebVitals;
```

> react官方文档已经给了我们性能提升的方案：https://zh-hans.legacy.reactjs.org/docs/optimizing-performance.html

打开`package.json`,发现可运行命令如下：

```json
 "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
```

`start`指令用于启动开发者服务器

`build`指令用于打包项目

`test`指令用于测试

`eject`指令用于抽离配置文件

> `cra`脚手架基于`webpack`，默认`webpack`的配置在 `node_modules `下的` react-scripts` 内部,但是一般r情况下，传输代码时，不会上传 `node_modules`，那么在必要情况下就必须得抽离配置文件。

如果项目创建不成功，复制别人的项目（不复制node_modules）

## 2.3 抽离配置文件

通过`npm run eject`或者`cnpm run eject` 或者`yarn eject`指令抽离配置文件

> 抽离配置文件过程中注意事项
>
> * 1.确保项目的git仓库是最新的
> * 2.如果不需要对于webpack进行配置，那么不需要抽离配置文件
> * create-react-app v2 默认支持 ts 以及sass 以及css的模块化，如果使用 sass作为css预处理器，那么不需要抽离配置文件

```sh

Copying files into E:\workspace\bj2304\code\react-course\react-basic
  Adding \config\env.js to the project
  Adding \config\getHttpsConfig.js to the project
  Adding \config\modules.js to the project
  Adding \config\paths.js to the project
  Adding \config\webpack.config.js to the project
  Adding \config\webpackDevServer.config.js to the project
  Adding \config\jest\babelTransform.js to the project
  Adding \config\jest\cssTransform.js to the project
  Adding \config\jest\fileTransform.js to the project
  Adding \scripts\build.js to the project
  Adding \scripts\start.js to the project
  Adding \scripts\test.js to the project
  Adding \config\webpack\persistentCache\createEnvironmentHash.js to the project

Updating the dependencies
  Removing react-scripts from dependencies
  Adding @babel/core to dependencies
  Adding @pmmmwh/react-refresh-webpack-plugin to dependencies
  Adding @svgr/webpack to dependencies
  Adding babel-jest to dependencies
  Adding babel-loader to dependencies
  Adding babel-plugin-named-asset-import to dependencies
  Adding babel-preset-react-app to dependencies
  Adding bfj to dependencies
  Adding browserslist to dependencies
  Adding camelcase to dependencies
  Adding case-sensitive-paths-webpack-plugin to dependencies
  Adding css-loader to dependencies
  Adding css-minimizer-webpack-plugin to dependencies
  Adding dotenv to dependencies
  Adding dotenv-expand to dependencies
  Adding eslint to dependencies
  Adding eslint-config-react-app to dependencies
  Adding eslint-webpack-plugin to dependencies
  Adding file-loader to dependencies
  Adding fs-extra to dependencies
  Adding html-webpack-plugin to dependencies
  Adding identity-obj-proxy to dependencies
  Adding jest to dependencies
  Adding jest-resolve to dependencies
  Adding jest-watch-typeahead to dependencies
  Adding mini-css-extract-plugin to dependencies
  Adding postcss to dependencies
  Adding postcss-flexbugs-fixes to dependencies
  Adding postcss-loader to dependencies
  Adding postcss-normalize to dependencies
  Adding postcss-preset-env to dependencies
  Adding prompts to dependencies
  Adding react-app-polyfill to dependencies
  Adding react-dev-utils to dependencies
  Adding react-refresh to dependencies
  Adding resolve to dependencies
  Adding resolve-url-loader to dependencies
  Adding sass-loader to dependencies
  Adding semver to dependencies
  Adding source-map-loader to dependencies
  Adding style-loader to dependencies
  Adding tailwindcss to dependencies
  Adding terser-webpack-plugin to dependencies
  Adding webpack to dependencies
  Adding webpack-dev-server to dependencies
  Adding webpack-manifest-plugin to dependencies
  Adding workbox-webpack-plugin to dependencies

Updating the scripts
  Replacing "react-scripts start" with "node scripts/start.js"
  Replacing "react-scripts build" with "node scripts/build.js"
  Replacing "react-scripts test" with "node scripts/test.js"

Configuring package.json
  Adding Jest configuration
  Adding Babel preset

Running npm install...

up to date in 13s
Ejected successfully!

warning: LF will be replaced by CRLF in code/react-course/react-basic/config/env.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/getHttpsConfig.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/jest/babelTransform.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/jest/cssTransform.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/jest/fileTransform.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/modules.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/paths.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/webpack.config.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/webpack/persistentCache/createEnvironmentHash.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/config/webpackDevServer.config.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/scripts/build.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/scripts/start.js.
The file will have its original line endings in your working directory
warning: LF will be replaced by CRLF in code/react-course/react-basic/scripts/test.js.
The file will have its original line endings in your working directory
Staged ejected files for commit.

Please consider sharing why you ejected in this survey:
  http://goo.gl/forms/Bi6CZjk1EqsdelXk1
```

```sh
$ npm start
$ npm build
```

## 2.4 webpack二次封装

### 2.4.1 集成css预处理器

* **集成less预处理器**

```sh
$ npm i less less-loader -D
```

* **集成sass预处理器**

  虽然已经默认支持了sass，但是得需要安装以下模块

```sh
$ npm i node-sass -D # cnpm i node-sass -D
```

* **集成stylus预处理器**

```
$ cnpm i stylus stylus-loader -D
```

具体配置如下：

`React-basic/config/webpack.config.js`

```js
// style files regexes 可以搜索此关键字快速定位
const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;
const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;
const lessRegex = /\.less$/; // +++++++
const lessModuleRegex = /\.module\.less$/;// +++++++
const stylusRegex = /\.(styl|stylus)$/;// +++++++
const stylusModuleRegex = /\.module\.(styl|stylus)$/;// +++++++

// "postcss" loader applies autoprefixer to our CSS.可以搜索此关键字快速定位
// "css" loader resolves paths in CSS and adds assets as dependencies.
// "style" loader turns CSS into JS modules that inject <style> tags.
// In production, we use MiniCSSExtractPlugin to extract that CSS
// to a file, but in development "style" loader enables hot editing
// of CSS.
// By default we support CSS Modules with the extension .module.css
{
  test: cssRegex,
    exclude: cssModuleRegex,
      use: getStyleLoaders({
        importLoaders: 1,
        sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
        modules: {
          mode: 'icss',
        },
      }),
        // Don't consider CSS imports dead code even if the
        // containing package claims to have no side effects.
        // Remove this when webpack adds a warning or an error for this.
        // See https://github.com/webpack/webpack/issues/6571
        sideEffects: true,
},
  // Adds support for CSS Modules (https://github.com/css-modules/css-modules)
  // using the extension .module.css
  {
    test: cssModuleRegex,
      use: getStyleLoaders({
        importLoaders: 1,
        sourceMap: isEnvProduction
        ? shouldUseSourceMap
        : isEnvDevelopment,
        modules: {
          mode: 'local',
          getLocalIdent: getCSSModuleLocalIdent,
        },
      }),
  },
    // Opt-in support for SASS (using .scss or .sass extensions).
    // By default we support SASS Modules with the
    // extensions .module.scss or .module.sass
    {
      test: sassRegex,
        exclude: sassModuleRegex,
          use: getStyleLoaders(
            {
              importLoaders: 3,
              sourceMap: isEnvProduction
              ? shouldUseSourceMap
              : isEnvDevelopment,
              modules: {
                mode: 'icss',
              },
            },
            'sass-loader'
          ),
            // Don't consider CSS imports dead code even if the
            // containing package claims to have no side effects.
            // Remove this when webpack adds a warning or an error for this.
            // See https://github.com/webpack/webpack/issues/6571
            sideEffects: true,
    },
      // Adds support for CSS Modules, but using SASS
      // using the extension .module.scss or .module.sass
      {
        test: sassModuleRegex,
          use: getStyleLoaders(
            {
              importLoaders: 3,
              sourceMap: isEnvProduction
              ? shouldUseSourceMap
              : isEnvDevelopment,
              modules: {
                mode: 'local',
                getLocalIdent: getCSSModuleLocalIdent,
              },
            },
            'sass-loader'
          ),
      },
        {
          test: lessRegex,
            exclude: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
                  modules: {
                    mode: 'icss',
                  },
                },
                'less-loader'
              ),
                // Don't consider CSS imports dead code even if the
                // containing package claims to have no side effects.
                // Remove this when webpack adds a warning or an error for this.
                // See https://github.com/webpack/webpack/issues/6571
                sideEffects: true,
        },
          // Adds support for CSS Modules, but using SASS
          // using the extension .module.scss or .module.sass
          {
            test: lessModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
                  modules: {
                    mode: 'local',
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
                'less-loader'
              ),
          },
     {
          test: stylusRegex,
            exclude: stylusModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
                  modules: {
                    mode: 'icss',
                  },
                },
                'stylus-loader'
              ),
                // Don't consider CSS imports dead code even if the
                // containing package claims to have no side effects.
                // Remove this when webpack adds a warning or an error for this.
                // See https://github.com/webpack/webpack/issues/6571
                sideEffects: true,
        },
          // Adds support for CSS Modules, but using SASS
          // using the extension .module.scss or .module.sass
          {
            test: stylusModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 3,
                  sourceMap: isEnvProduction
                  ? shouldUseSourceMap
                  : isEnvDevelopment,
                  modules: {
                    mode: 'local',
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
                'stylus-loader'
              ),
          },
```

### 2.4.2 配置@解析别名

> vue项目中可以使用@代替src目录，那么react中抽离配置文件之后也可以实现此功能

`react-basic/config/webpack.config.js`

```js
alias: {
  '@': path.resolve('src'), // +++++++++++++
    // Support React Native Web 搜索此关键词快速定位
    // https://www.smashingmagazine.com/2016/08/a-glimpse-into-the-future-with-react-native-for-web/
    'react-native': 'react-native-web',
      // Allows for better profiling with ReactDevTools
      ...(isEnvProductionProfile && {
      'react-dom$': 'react-dom/profiling',
      'scheduler/tracing': 'scheduler/tracing-profiling',
    }),
      ...(modules.webpackAliases || {}),
},
```

`src/index.js` 测试

```js
import React from 'react';
import ReactDOM from 'react-dom/client';
import '@/index.css'; // ++++++
import App from '@/App';// ++++++
import reportWebVitals from '@/reportWebVitals';// ++++++

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```

如果控制台报错如下，说明@别名没有配置成功

```js
Failed to compile.

Module not found: Error: Can't resolve '@/index.css' in '/Users/wudaxun/Desktop/workspace/code/bk2207A/code/react-course/react-basic/src'
ERROR in ./src/index.js 6:0-21
Module not found: Error: Can't resolve '@/index.css' in '/Users/wudaxun/Desktop/workspace/code/bk2207A/code/react-course/react-basic/src'

ERROR in ./src/index.js 7:0-24
Module not found: Error: Can't resolve '@/App' in '/Users/wudaxun/Desktop/workspace/code/bk2207A/code/react-course/react-basic/src'

ERROR in ./src/index.js 8:0-48
Module not found: Error: Can't resolve '@/reportWebVitals' in '/Users/wudaxun/Desktop/workspace/code/bk2207A/code/react-course/react-basic/src'

webpack compiled with 3 errors
```

如果没有错误说明配置是成功的。

> 如果不抽离配置文件，但是也需要配置别名@
>
> ```sh
> $ cnpm i @craco/craco -D
> ```
>
> 项目根目录中创建 craco 的配置文件：`craco.config.js`
>
> ```js
> const path = require('path')
> module.exports = {
>     webpack: {
>        alias: {
>          '@': path.resolve(__dirname, 'src')
>        }
>     }
> }
> ```
>
> 修改`package.json`中运行命令
>
> ```json
> "scripts": {
>    	"start": "craco start",
>    	"build": "craco build"
>    	"test": "craco test"
> }
> ```
>
> 重启服务器即可生效

## 2.5 setupProxy代理

> 即使不抽离配置文件，也是在此处配置代理

如果整个项目只有一个服务器且有跨域问题，可以直接在`package.json`中做如下配置：

```
"proxy": "http://121.89.205.189:3000/api",
```

然后在项目中可以如下访问：

```
// src/index.js
fetch('/pro/list').then(res => res.json()).then(res => {
  console.log(res.data)
})
```

**了解**：那么如果有多个服务器 并且也需要解决跨域问题

首先，`http-proxy-middleware`使用 npm 或 Yarn 安装：

```sh
$ cnpm install http-proxy-middleware -S
$ # or
$ yarn add http-proxy-middleware -S
```

接下来，创建`src/setupProxy.js`并在其中放置以下内容：

```js
const { createProxyMiddleware } = require('http-proxy-middleware');

module.exports = function(app) {
  // ...
};
```

您现在可以根据需要注册代理了！这是使用上述内容的示例`http-proxy-middleware`：

```js
const { createProxyMiddleware } = require('http-proxy-middleware'); // 此处不使用import语法

module.exports = function (app) {
  // http://121.89.205.189:3001/api/pro/list ==> /myapi/pro/list
  app.use('/myapi', createProxyMiddleware({
      target: 'http://121.89.205.189:3001/api',
      changeOrigin: true,
      pathRewrite: {
        '^/myapi': ''
      }
    })
  );
  
}
```

> **注意：**您不需要在任何地方导入此文件。当您启动开发服务器时，它会自动注册。

> **注意：**此文件仅支持 Node 的 JavaScript 语法。确保只使用受支持的语言功能（即不支持 Flow、ES 模块等）。

> **注意：**将路径传递给代理函数允许您在路径上使用通配符和/或模式匹配，这比快速路由匹配更灵活。

# 三、JSX

设想如下变量声明：

```js
const element = <h1>Hello, world!</h1>;
```

这个有趣的标签语法既不是字符串也不是 HTML。

它被称为 JSX，是一个 JavaScript 的语法扩展。

JSX 可以生成 React “元素”。

> React [不强制要求](https://zh-hans.reactjs.org/docs/react-without-jsx.html)使用 JSX，但是大多数人发现，在 JavaScript 代码中将 JSX 和 UI 放在一起时，会在视觉上有辅助作用。它还可以使 React 显示更多有用的错误和警告消息。

`src`文件夹下只保留`index.js`

## 3.1 jsx语法详解

在下面的例子中，我们声明了一个名为 `name` 的变量，然后在 JSX 中使用它，并将它包裹在大括号中

`src/index.js`

```js
// src/index.js

// // react V18版本
// import React from 'react'
// import ReactDOM from 'react-dom/client'
// // jsx语法 - javascript xml
// const name = <h1>hello, react course!</h1>
// const root = ReactDOM.createRoot(document.getElementById('root'))
// root.render(name)


// react V18版本以前
import React from 'react'
import ReactDOM from 'react-dom'
// jsx语法 - javascript xml
const name = <h1>hello, react course!!!</h1>

ReactDOM.render(
  name,
  document.getElementById('root')
)

```

> 在 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)。例如，`2+2`，`user.firstName` 或 `formatName(user)` 都是有效的 JavaScript 表达式。

在下面的示例中，我们将调用 JavaScript 函数 `formatName(user)` 的结果，并将结果嵌入到 `<div>` 元素中。

`src/index.js`

```js
// src/index.js

// // react V18版本
// import React from 'react'
// import ReactDOM from 'react-dom/client'
// // jsx语法 - javascript xml
// const name = <h1>hello, react course!</h1>
// const root = ReactDOM.createRoot(document.getElementById('root'))
// root.render(name)


// react V18版本以前
import React from 'react'
import ReactDOM from 'react-dom'

function greet () {
  return '您好'
}
// jsx语法 - javascript xml
// 不要在jsx代码中使用循环控制语句
const name = <h1>
  hello, react course!!! - { 2 + 2 } - { 5 > 3 ? '真' : '假' } - { greet() } 
  </h1>

ReactDOM.render(
  name,
  document.getElementById('root')
)

```

**jsx也是一个表达式**

在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX

`src/index.js`

```js
// src/index.js

// // react V18版本
// import React from 'react'
// import ReactDOM from 'react-dom/client'
// // jsx语法 - javascript xml
// const name = <h1>hello, react course!</h1>
// const root = ReactDOM.createRoot(document.getElementById('root'))
// root.render(name)


// react V18版本以前
import React from 'react'
import ReactDOM from 'react-dom'

function greet () {
  return '您好'
}
// jsx语法 - javascript xml
// 不要在jsx代码中使用循环控制语句
let name = null

if ( 12 > 11) {
  name = <h1>
    hello, react course!!!--11
    </h1>
} else {
  name = <h1>
    hello, react course!!! --22
    </h1>
}

ReactDOM.render(
  name,
  document.getElementById('root')
)

```

> 因为 JSX 语法上更接近 JavaScript 而不是 HTML，所以 React DOM 使用 `camelCase`（小驼峰命名）来定义属性的名称，而不使用 HTML 属性名称的命名约定。
>
> 例如，JSX 里的 `class` 变成了 [`className`](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)，而 `tabindex` 则变为 [`tabIndex`](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/tabIndex)。

Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

> 上述代码都没见到使用过 React 模块，但是显示却用了，为什么？

## 3.2 React.createElement

先看一个代码

`src/index.js`

```js
// src/index.js
import React from 'react'

import ReactDOM from 'react-dom/client'

// jsx代码中写多行代码时，建议使用()包裹
// 标签样式 class样式，将其改为 className；因为class在js中是一个关键字
// jsx代码会导致 React 模块被调用，因为底层调用了 React.createElement，等同于vue中的渲染函数 h
const app = (
  <div className='container'>
    <div className='box'>
      <header className='header'>header</header>
      <div className='content'>content</div>
    </div>
    <footer className='footer'>footer</footer>
  </div>
)


const root = ReactDOM.createRoot(document.querySelector('#root'))

root.render(app)
```

`React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```js
// 注意：这是简化过的结构
const element = {
  type: 'div',
  props: {
    className: 'box',
    children: 'hello react'
  }
};
```



这些对象被称为 “React 元素”。它们描述了你希望在屏幕上看到的内容。React 通过读取这些对象，然后使用它们来构建 DOM 以及保持随时更新。

```jsx
// src/index.js
import React from 'react'

import ReactDOM from 'react-dom/client'

// jsx代码中写多行代码时，建议使用()包裹
// 标签样式 class样式，将其改为 className；因为class在js中是一个关键字
// jsx代码会导致 React 模块被调用，因为底层调用了 React.createElement，等同于vue中的渲染函数 h
// const app = (
//   <div className='container'>
//     <div className='box'>
//       <header className='header'>header</header>
//       <div className='content'>content</div>
//     </div>
//     <footer className='footer'>footer</footer>
//   </div>
// )

// 添加唯一标识 key ， 渲染函数内部是数组，其实就是并且标签时
const app = React.createElement('div', { className: 'container'}, [
  React.createElement('div', { className: 'box', key: 0 }, [
    React.createElement('header', { className: 'header', key: 2 }, 'my header'),
    React.createElement('div', { className: 'content', key: 3 }, 'my content')
  ]),
  React.createElement('footer', { className: 'footer', key: 1 }, 'my footer')
])


const root = ReactDOM.createRoot(document.querySelector('#root'))

root.render(app)
```



# 四、组件定义

组件允许你将 UI 拆分为独立可复用的代码片段，并对每个片段进行独立构思。

组件，从概念上类似于 JavaScript 函数。它接受任意的入参（即 “props”），并返回用于描述页面展示内容的 React 元素。

## 4.1 类组件

ES6的加入让JavaScript直接支持使用class来定义一个类，react的创建类组件的方式就是使用的类的继承，`ES6 class`是一种使用React组件的写法，它使用了ES6标准语法来构建

```jsx
// src/index.js
import React from 'react'

import ReactDOM from 'react-dom/client'

// 定义一个类组件
// 组件的首字母必须大写
class App extends React.Component {
  // es5 的继承机制  Vs es6的继承机制
  // ES6 规定，子类必须在constructor()方法中调用super()，否则就会报错。
  // 这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，
  // 得到与父类同样的实例属性和方法，然后再对其进行加工，添加子类自己的实例属性和方法。
  // 如果不调用super()方法，子类就得不到自己的this对象。

  // ES6 的继承机制，与 ES5 完全不同。
  // ES5 的继承机制，是先创造一个独立的子类的实例对象，
  // 然后再将父类的方法添加到这个对象上面，即“实例在前，继承在后”。
  // ES6 的继承机制，则是先将父类的属性和方法，加到一个空的对象上面，
  // 然后再将该对象作为子类的实例，即“继承在前，实例在后”。
  // 这就是为什么 ES6 的继承必须先调用super()方法，
  // 因为这一步会生成一个继承父类的this对象，没有这一步就无法继承父类。
  constructor (props) {
    super(props) // 拿到父类的实例的属性和方法
    this.state = { // 子类自己的定义的初始化的状态
      count: 1000
    }
  }

  // render函数是react的类组件中必不可少的一个必须实现的函数
  // render 函数可以返回一段jsx代码，当作当前组件的结构
  render () {
    return (
      <h1>hello react class component - { this.state.count }</h1>
    )
  }
}

const root = ReactDOM.createRoot(document.getElementById('root'))

// 标签形式调用组件
//  Warning: Functions are not valid as a React child. 
//  This may happen if you return a Component instead of <Component /> from render. 
//  Or maybe you meant to call this function rather than return it.
// root.render(App)
root.render(<App />)

```

## 4.2 函数组件

定义组件最简单的方式就是编写 JavaScript 函数

```jsx
// src/index.js
import React from 'react'

import ReactDOM from 'react-dom/client'

// 定义一个函数式组件
// function App () {
//   return (
//     <h1>hello react function component</h1>
//   )
// }

// const App = function () {
//   return (
//     <h1>hello react function component!</h1>
//   )
// }


// const App = () => {
//   return (
//     <h1>hello react function component!!</h1>
//   )
// }

// 箭头函数 可以自带返回值，这种写法表示 箭头函数返回了一段jsx代码
// 一般在组件内不需要写别的业务逻辑时使用
const App = () => (
  <h1>hello react function component!!</h1>
)

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

## 4.3 两组组件的区别

* 组件的定义方式不同。
* 生命周期不同：类组件有，函数式组件没有。
* 副作用操作执行不同：class组件通过生命周期函数，函数组件用hook的useEffect。
* state的定义、读取、修改方式不同：函数组件用hook的useState。
* this： class组件有，函数式组件没有。
* 实例： class组件有，函数式组件没有。
* ref使用不同：类组件可以获取子组件实例，函数式组件不可以，因为函数式组件没有实例。

> 官方推荐使用函数式组件,以上不同点虽然现在不明白是啥意思，没有关系，会随着大家的学习印象加深。

# 五、Props

## 5.1 Props详解

`props`是正常是外部传入的，组件内部也可以通过一些方式来初始化的设置，属性不能被组件自己更改，但是你可以通过父组件主动重新渲染的方式来传入新的 `props`

React 非常灵活，但它也有一个严格的规则：

**所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

> 纯函数：输入一定，输出一定确定

总的来说，在使用一个组件的时候，可以把参数放在标签的属性当中，所有的属性都会作为组件 `props` 对象的键值。

通过箭头函数创建的组件，需要通过函数的默认参数来接收`props`

通过类创建的组件，需要通过 `this.props`来接收

组件可以在其输出中引用其他组件。

```
<van-swipe>
	<van-swipe-item></van-swipe-item>
</van-swipe>
```

这就可以让我们用同一组件来抽象出任意层次的细节。

按钮，表单，对话框，甚至整个屏幕的内容：在 React 应用程序中，这些通常都会以组件的形式表示。

## 5.2 父子组件通信

### 5.2.1 构建一个父子组件

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './01_props/01Parent_Child'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/01Parent_Child.jsx`

```js
// src/01_props/01Parent_Child.jsx
import React from 'react'

const Child2 = () => {
  return (
    <div>子组件2</div>
  )
}

class Child1 extends React.Component {
  render () {
    return (
      <div>子组件1</div>
    )
  }
}

class Parent extends React.Component {
  render () {
    return (
      <div>
        <h1>父子组件</h1>
        <div>父组件</div>
        <hr />
        <Child1 />
        <Child2 />
      </div>
    )
  }
}

export default Parent

```

>回顾vue父子传值：
>
>父组件调用子组件的地方，添加自定义的属性，属性的值就i是需要传递给子组件的数据
>
>如果属性的值是变量，boolean类型，number类型，对象，数组，null，undefined，正则，则需要使用绑定属性
>
>在子组件定义的地方，添加props选项，props选项有2中写法
>
>第一种写法为数组，数组的元素为自定义的属性名，即可在模板中通过自定义的属性名获取父组件 的值
>
>第二种写法为对象，该对象又有两种写法
>
>​	第一种写法，key值为自定义的属性名，value值为数据类型，即可在模板中通过自定义的属性名获取父组件 的值
>
>​	第二种写法，key值为自定义的属性名，value值为对象，对象下的
>
>​		key如果为type，表示数据类型
>
>​		key如果为default，表示的自定义属性的默认值，如果默认值是数组或者对象，写为函数返回即可
>
>​		key如果是required，表示该自定义的属性是不是必须传递的
>
>​		key 如果是validator，表示需要自定义一个函数验证该自定义属性
>
>项目中一般都以对象形式说明数据类型为主
>

### 5.2.2 父组件给子组件传值

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
import App from './01_props/02Parent_Child_value'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/02Parent_Child_value.jsx`

```js
// src/01_props/02Parent_Child_value.jsx
import React from 'react'
// 如果子组件是函数式组件，则可以通过 函数的默认参数 props.自定义的属性名 渲染页面
const Child2 = (props) => {
  return (
    <div>
      子组件2
      - { props.str } 
      - { props.num } 
      - { props.obj.a } 
      - { props.arr[0] }
      - { props.flag.toString() }
    </div>
  )
}
// 如果子组件是类组件，则可以通过 this.props.自定义的属性名 渲染页面
class Child1 extends React.Component {
  render () {
    return (
      <div>
        子组件1
        - { this.props.str } 
        - { this.props.num } 
        - { this.props.obj.a } 
        - { this.props.arr[0] }
        - { this.props.flag.toString() }
      </div>
    )
  }
}

class Parent extends React.Component {
  render () {
    return (
      <div>
        <h1>父子组件</h1>
        <div>父组件</div>
        <hr />
        {/* 
          在父组件调用子组件的地方，添加自定义的属性，如果属性的值是变量，boolean类型，number类型，
          对象，数组，null， undefined，正则，需要使用 {} 包裹
        */}
        <Child1 
          str="child1在此" 
          obj={ { a:1, b:2 } } 
          arr={ ['a', 'b', 'c', 'd'] } 
          num={ 1000 } 
          flag={true}
        />
        <Child2 
          str="child2在此" 
          obj={ { a:11, b:22 } } 
          arr={ ['aa', 'bb', 'cc', 'dd'] } 
          num={ 2000 }  
          flag={false}
        />
      </div>
    )
  }
}

export default Parent

```

### 5.2.3 父组件给子组件传值设置默认值

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
import App from './01_props/03Parent_Child_default'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/03_Parent_Chil_default.jsx`

```jsx
// src/01_props/03Parent_Child_default.jsx
import React from 'react'
// 1.在父组件调用子组件的地方，添加自定义的属性，如果属性的值是变量，boolean类型，number类型，
// 对象，数组，null， undefined，正则，需要使用 {} 包裹
// 2.1 如果子组件是类组件，则可以通过 this.props.自定义的属性名 渲染页面
// 2.2 如果子组件是函数式组件，则可以通过 函数的默认参数 props.自定义的属性名 渲染页面
// 3.1 如果需要给组件传递的值设置默认值的话，可以在定义完组件后 通过 组件.defaultProps = {} 设置
// 3.2 依据es6的类的静态属性也可以设置默认值, 通过 static defaultProps = {} 设置

const Child2 = (props) => {
  return (
    <div>
      子组件2
      - { props.str } 
      - { props.num } 
      - { props.obj.a } 
      - { props.arr[0] }
      - { props.flag.toString() }
    </div>
  )
}
Child2.defaultProps = {
  num: 10000
}

class Child1 extends React.Component {
  static defaultProps = {
    num: 30000
  }
  render () {
    return (
      <div>
        子组件1
        - { this.props.str } 
        - { this.props.num } 
        - { this.props.obj.a } 
        - { this.props.arr[0] }
        - { this.props.flag.toString() }
      </div>
    )
  }
}
// Child1.defaultProps = {
//   num: 20000
// }
class Parent extends React.Component {
  render () {
    return (
      <div>
        <h1>父子组件</h1>
        <div>父组件</div>
        <hr />
        {/* 
          在父组件调用子组件的地方，添加自定义的属性，如果属性的值是变量，boolean类型，number类型，
          对象，数组，null， undefined，正则，需要使用 {} 包裹
        */}
        <Child1 
          str="child1在此" 
          obj={ { a:1, b:2 } } 
          arr={ ['a', 'b', 'c', 'd'] } 
          flag={true}
        />
        <Child2 
          str="child2在此" 
          obj={ { a:11, b:22 } } 
          arr={ ['aa', 'bb', 'cc', 'dd'] } 
          flag={false}
        />
      </div>
    )
  }
}

export default Parent

```

### 5.2.4 使用prop-types属性验证

> 自 React v15.5 起，`React.PropTypes` 已移入另一个包中。请使用 [`prop-types` 库](https://www.npmjs.com/package/prop-types) 代替。
>
> ```sh
> $ cnpm i prop-types -D
> ```
>
> 
>
> ```jsx
> import PropTypes from 'prop-types';
> 
> MyComponent.propTypes = {
> // 你可以将属性声明为 JS 原生类型，默认情况下
> // 这些属性都是可选的。
> optionalArray: PropTypes.array,
> optionalBool: PropTypes.bool,
> optionalFunc: PropTypes.func,
> optionalNumber: PropTypes.number,
> optionalObject: PropTypes.object,
> optionalString: PropTypes.string,
> optionalSymbol: PropTypes.symbol,
> 
> // 任何可被渲染的元素（包括数字、字符串、元素或数组）
> // (或 Fragment) 也包含这些类型。
> optionalNode: PropTypes.node,
> 
> // 一个 React 元素。
> optionalElement: PropTypes.element,
> 
> // 一个 React 元素类型（即，MyComponent）。
> optionalElementType: PropTypes.elementType,
> 
> // 你也可以声明 prop 为类的实例，这里使用
> // JS 的 instanceof 操作符。
> optionalMessage: PropTypes.instanceOf(Message),
> 
> // 你可以让你的 prop 只能是特定的值，指定它为
> // 枚举类型。
> optionalEnum: PropTypes.oneOf(['News', 'Photos']),
> 
> // 一个对象可以是几种类型中的任意一个类型
> optionalUnion: PropTypes.oneOfType([
>  PropTypes.string,
>  PropTypes.number,
>  PropTypes.instanceOf(Message)
> ]),
> 
> // 可以指定一个数组由某一类型的元素组成
> optionalArrayOf: PropTypes.arrayOf(PropTypes.number),
> 
> // 可以指定一个对象由某一类型的值组成
> optionalObjectOf: PropTypes.objectOf(PropTypes.number),
> 
> // 可以指定一个对象由特定的类型值组成
> optionalObjectWithShape: PropTypes.shape({
>  color: PropTypes.string,
>  fontSize: PropTypes.number
> }),
> 
> // An object with warnings on extra properties
> optionalObjectWithStrictShape: PropTypes.exact({
>  name: PropTypes.string,
>  quantity: PropTypes.number
> }),
> 
> // 你可以在任何 PropTypes 属性后面加上 `isRequired` ，确保
> // 这个 prop 没有被提供时，会打印警告信息。
> requiredFunc: PropTypes.func.isRequired,
> 
> // 任意类型的必需数据
> requiredAny: PropTypes.any.isRequired,
> 
> // 你可以指定一个自定义验证器。它在验证失败时应返回一个 Error 对象。
> // 请不要使用 `console.warn` 或抛出异常，因为这在 `oneOfType` 中不会起作用。
> customProp: function(props, propName, componentName) {
>  if (!/matchme/.test(props[propName])) {
>    return new Error(
>      'Invalid prop `' + propName + '` supplied to' +
>      ' `' + componentName + '`. Validation failed.'
>    );
>  }
> },
> 
> // 你也可以提供一个自定义的 `arrayOf` 或 `objectOf` 验证器。
> // 它应该在验证失败时返回一个 Error 对象。
> // 验证器将验证数组或对象中的每个值。验证器的前两个参数
> // 第一个是数组或对象本身
> // 第二个是他们当前的键。
> customArrayProp: PropTypes.arrayOf(function(propValue, key, componentName, location, propFullName) {
>  if (!/matchme/.test(propValue[key])) {
>    return new Error(
>      'Invalid prop `' + propFullName + '` supplied to' +
>      ' `' + componentName + '`. Validation failed.'
>    );
>  }
> })
> };
> 
> 
> ```
>
> 

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
import App from './01_props/04Parent_Child_type'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/04_Parent_Chil_type.jsx`

```js
// src/01_props/04Parent_Child_type.jsx
import React from 'react'
import PropTypes from 'prop-types'
// 1.在父组件调用子组件的地方，添加自定义的属性，如果属性的值是变量，boolean类型，number类型，
// 对象，数组，null， undefined，正则，需要使用 {} 包裹
// 2.1 如果子组件是类组件，则可以通过 this.props.自定义的属性名 渲染页面
// 2.2 如果子组件是函数式组件，则可以通过 函数的默认参数 props.自定义的属性名 渲染页面
// 3.1 如果需要给组件传递的值设置默认值的话，可以在定义完组件后 通过 组件.defaultProps = {} 设置
// 3.2 依据es6的类的静态属性也可以设置默认值, 通过 static defaultProps = {} 设置
// 4.1 如果需要校验数据类型，可以通过prop-types模块，定义完组件后，通过 组件.propTypes = { key: PropTypes.数据类型 } 设置
// 4.2 如果需要设定某一项数据是必须传递的，可以通过  组件.propTypes = { key: PropTypes.数据类型.isRequired } 设置
// 4.3 如果某一项数据既可以是 string类型，也可以是number类型，可以通过  组件.propTypes = { key: PropTypes.oneOfType([PropTypes.string, PropTypes.number]).isRequired } 设置
// 4.4 如果需要对某一项数据做自定义的验证规则，可以把属性写为一个函数，函数内部必须返回报错的信息
const Child2 = (props) => {
  return (
    <div>
      子组件2
      - { props.str } 
      - { props.num } 
      - { props.obj.a } 
      - { props.arr[0] }
      - { props.flag.toString() }
    </div>
  )
}
Child2.defaultProps = {
  num: 10000
}
Child2.propTypes = {
  str: PropTypes.string.isRequired,
  // num: PropTypes.number,
  num: PropTypes.oneOfType([ PropTypes.number, PropTypes.string ]).isRequired,
  obj: PropTypes.object

}

class Child1 extends React.Component {
  static defaultProps = {
    num: 30000
  }
  render () {
    return (
      <div>
        子组件1
        - { this.props.str } 
        - { this.props.num } 
        - { this.props.obj.a } 
        - { this.props.arr[0] }
        - { this.props.flag.toString() }
      </div>
    )
  }
}
// Child1.defaultProps = {
//   num: 20000
// }

Child1.propTypes = {
  num (props, propName, componentName ) {
    if (props[propName] < 100000) {
      return new Error(componentName + '传递的值太小')
    }
  }
}
class Parent extends React.Component {
  render () {
    return (
      <div>
        <h1>父子组件</h1>
        <div>父组件</div>
        <hr />
        {/* 
          在父组件调用子组件的地方，添加自定义的属性，如果属性的值是变量，boolean类型，number类型，
          对象，数组，null， undefined，正则，需要使用 {} 包裹
        */}
        <Child1 
          str="child1在此" 
          obj={ { a:1, b:2 } } 
          arr={ ['a', 'b', 'c', 'd'] } 
          flag={true}
        />
        <Child2 
          str="child2在此" 
          obj={ { a:11, b:22 } } 
          arr={ ['aa', 'bb', 'cc', 'dd'] } 
          flag={false}
        />
      </div>
    )
  }
}

export default Parent

```

## 5.3 props.children

我们知道使用组件的时候，可以嵌套。要在自定义组件中使用嵌套结构，就需要使用 `props.children` 。

等同于 vue中的 slot 插槽

> 内容分发 == 插槽
>
> 自定义组件内部的内容显示还是不显示（看组件定义时有没有slot标签），在哪里显示（定义组件模板时slot的位置），如何显示（具名插槽），这就是内容分发所干的活

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
import App from './01_props/05App_props_children'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/05App_props_children.jsx`

```jsx
// src/01_props/05App_props_children.jsx
import React, { Component } from 'react';

function Child2 (props) {
  return (
    <div>
    { props.children }
  </div>
  )
}

class Child1 extends Component {
  render() {
    return (
      <div>
        {/* 等同于 vue 中的 <slot></slot> */}
        { this.props.children }
      </div>
    );
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Child1 >
          child1
        </Child1>
        <Child2 >
          child2
        </Child2>
      </div>
    );
  }
}

export default App;
```

如果需要给组件添加多个元素，并且显示在多个位置，可以如下设置：

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
import App from './01_props/06_App_multiple_props_children'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/06_App_multiple_props_children.jsx`

```jsx
// src/01_props/06_App_multiple_props_children.jsx
import React, { Component } from 'react';

function Header (props) {
  console.log(props)
  return (
    <ul>
      {/*  {/* <slot name="left"></slot> */} */}
      <li>左侧：{ props.children[0] }</li>
      <li>中间：{ props.children[1] }</li>
      <li>右侧：{ props.children[2] }</li>
    </ul>
  )
}

class App extends Component {
  render() {
    return (
      <div>
        <Header>
         {/* <template #left>首页左侧</template> */}
          <div>首页左侧</div>
          <div>首页中间</div>
          <div>首页右侧</div>
        </Header>
        <Header>
          <div>分类左侧</div>
          <div>分类中间</div>
          <div>分类右侧</div>
        </Header>
      </div>
    );
  }
}

export default App;
```

> 实现类似vue的具名插槽，需要通过 props.children 的下标去访问

## 5.4 render props特性

>  使用 Render Props 来解决横切关注点（Cross-Cutting Concerns）

> 什么情况下需要使用状态管理器
>
> * 多个视图需要依赖于同一个状态
> * 来自不同视图的行为需要变更同一个状态

组件是 React 代码复用的主要单元，但如何将一个组件封装的状态或行为 共享给其他需要相同状态的组件 并不总是显而易见。

以下组件跟踪 Web 应用程序中的鼠标位置:

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
import App from './01_props/07App_mouse_tracker'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/01_props/07App_mouse_tracker.jsx`

> 还没有学习状态state以及事件处理，这里先用

```jsx
// src/01_props/07App_mouse_tracker.jsx
import React, { Component } from 'react';

class App extends Component {

  constructor (props) {
    super(props)
    this.state = { // 组件初始化数据，等同于 vue中的data选项
      x: 0, y: 0
    }
  }

  render() {
    return (
      <div
        style={ { width: '100vw', height: '100vh', backgroundColor: '#f66'} }
        onMouseMove={ (event) => {
          console.log(event.clientX, event.clientY)
          this.setState({
            x: event.clientX, 
            y: event.clientY
          })
        } }
      >
        鼠标的位置为：(x: { this.state.x }, y: { this.state.y })
      </div>
    );
  }
}

export default App;
```

> 当光标在屏幕上移动时，组件在 `<div>` 中显示其坐标。
>
> 现在的问题是：我们如何在另一个组件中复用这个行为？换个说法，若另一个组件需要知道鼠标位置，我们能否封装这一行为，以便轻松地与其他组件共享它？

**render prop 是一个用于告知组件需要渲染什么内容的函数 prop。**

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// 引入react组件时，后缀名可以不写
// import App from './01_props/01_Parent_Child' // 父子组件
// import App from './01_props/02_Parent_Child_value' // 父组件给子组件传值
// import App from './01_props/03_Parent_Child_default' // 父组件给子组件传值,子组件设置默认值
// import App from './01_props/04_Parent-Child_type' // 父组件给子组件传值,子组件设置默认值,验证数据类型
// import App from './01_props/05_App_props_children' // 类插槽
// import App from './01_props/06_App_mutiple_props_children' // 类具名插槽 多个插槽 
// import App from './01_props/07_App_mouse_tracker' // 鼠标跟随 
import App from './01_props/08_App_render_props' // 渲染属性 - 其他组件共享状态

const root = ReactDOM.createRoot(document.querySelector('#root'))

root.render(<App />)

```

`src/01_props/08App_render_props.jsx`

```jsx
// src/01_props/08App_render_props.jsx
import React, { Component } from 'react';
// 渲染属性共享组件的状态
// 在调用需要共享的组件（Mouse）上，添加一个render的自定义属性，该属性是一个自定义函数
// 在自定义函数的内部返回需要共享给的那个组件（Cat）
// 在需要共享的组件（Mouse）内部，通过 this.props.render() 或者 props.render() 即可调用，参数即为需要共享的状态
// 那么在定义自定义render属性的函数内部，就会接收到 参数，通过返回的组件（Cat）传递该参数即可
function Dog (props) {
  return (
    <div>DOG的位置为：(x: { props.points.x }, y: { props.points.y })</div>
  )
}
function Cat (props) {
  return (
    <div>CAT的位置为：(x: { props.points.x }, y: { props.points.y })</div>
  )
}
function Tiger (props) {
  return (
    <div>Tiger的位置为：(x: { props.points.x }, y: { props.points.y })</div>
  )
}

class Mouse extends Component {
  constructor (props) {
    super(props)
    this.state = { // 组件初始化数据，等同于 vue中的data选项
      x: 0, y: 0
    }
  }

  render() {
    return (
      <div
        style={ { width: '100vw', height: '30vh', backgroundColor: '#f66'} }
        onMouseMove={ (event) => {
          console.log(event.clientX, event.clientY)
          this.setState({
            x: event.clientX, 
            y: event.clientY
          })
        } }
      >
        { 
          this.props.render({
            x: this.state.x,
            y: this.state.y
          })
        }
      </div>
    );
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Mouse render = { (points) => { // 完成子组件给父祖件传值
          return <Dog points={ points }/>
        } }/>
        <Mouse render = { (points) => {
          return <Cat points={points}/>
        } }/>
        <Mouse render = { (points) => {
          return <Tiger points={points}/>
        } }/>
      </div>
    );
  }
}

export default App;
```

> 此案例实际上完成了react中子组件给父组件传值

# 六、State

`state` 是 `class`组件的内置对象，用于class组件内部数据更新

`state`就是组件描述某种显示情况的数据，由组件自己设置和更改，也就是说由组件自己维护，使用`state`的目的就是为了在不同的状态下使组件的显示不同(自己管理)

## 6.1 state及其特点

State 与 props 类似，但是 state 是私有的，并且完全受控于当前组件

不要直接修改state

state更新可能是异步的：出于性能考虑，React 可能会把多个 `setState()` 调用合并成一个调用。

state更新会被合并：当你调用 `setState()` 的时候，React 会把你提供的对象合并到当前的 state

## 6.2 state的定义和使用

目前react中的状态有两种使用方式：

### 6.2.1 es6的类 - 构造函数

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

import App from './02_state/01App_state_es6'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/02_state/01App_state_es6.jsx`

```jsx
// src/02_state/01App_state_es6.jsx
import React, { Component } from 'react';

// 使用构造函数即可
class App extends Component {
  constructor (props) {
    super(props)
    this.state = {
      count: 100,
      time: new Date()
    }
  }
  render() {
    return (
      <div>
        { this.state.count } - { this.state.time.toLocaleDateString() + ' ' + this.state.time.toLocaleTimeString() }
      </div>
    );
  }
}

export default App;
```

### 6.2.2 es7的类 - 属性初始化器

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
import App from './02_state/02App_state_es7'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/02_state/02App_state_es7.jsx`

```jsx
// src/02_state/02App_state_es7.jsx
import React, { Component } from 'react';

// es7 - 属性初始化器
class App extends Component {
  // constructor (props) {
  //   super(props)
  //   this.state = {
  //     count: 1,
  //     time: new Date()
  //   }
  // }
  state = { // es7 - 属性初始化器
    count: 100000,
    time: new Date()
  }
  render() {
    return (
      <div>
        { this.state.count } - { this.state.time.toLocaleDateString() + ' ' + this.state.time.toLocaleTimeString() }
      </div>
    );
  }
}

export default App;
```

## 6.3 如何正确的修改state

`setState()` 将对组件 state 的更改排入队列，并通知 React 需要使用更新后的 state 重新渲染此组件及其子组件。这是用于更新用户界面以响应事件处理器和处理服务器数据的主要方式.

将 `setState()` 视为*请求*而不是立即更新组件的命令。为了更好的感知性能，React 会延迟调用它，然后通过一次传递更新多个组件。

`setState()` 并不总是立即更新组件。它会批量推迟更新。这使得在调用 `setState()` 后立即读取 `this.state` 成为了隐患。为了消除隐患，请使用 `componentDidUpdate` 或者 `setState` 的回调函数（`setState(updater, callback)`），这两种方式都可以保证在应用更新后触发。

记住修改状态的三大原则：

* 不要直接修改 State

```js
state = { a: 10 }
this.state.a = 100 // ❌
```

* state 的更新可能是异步的

```js
state = { a: 10 }
this.setState({a: this.state.a + 1 })
this.setState({a: this.state.a + 1 })
this.setState({a: this.state.a + 1 })
console.log(this.state.a) // 10
```

* state 的更新会被合并

## 6.4 this.setState()方法及其特点

`setState()` 会对一个组件的 `state` 对象安排一次更新。当 state 改变了，该组件就会重新渲染。

`setState()`可以添加两个参数,

`setState()` 的第二个参数为可选的回调函数，它将在 `setState` 完成合并并重新渲染组件后执行

### 6.4.1 传递函数

参数一为带有形式参数的 `updater` 函数：

```
this.setState((state, props) => stateChange[, callback] )
```

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
import App from './02_state/03App_setState_function'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/02_state/03App_setState_function.jsx`

```jsx
// src/02_state/03App_setState_function.jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    count: 10
  }
  render() {
    return (
      <div>
        <button onClick={ () => {
          // prevState 只要调用过 setState, 那么prevState就是最新的值
          this.setState((prevState) => {
            return {
              count: prevState.count + 1
            }
          }, () => {
            console.log('count1', this.state.count) // 13
          })
          this.setState((prevState) => {
            return {
              count: prevState.count + 1
            }
          }, () => {
            console.log('count1', this.state.count) // 13
          })
          this.setState((prevState) => {
            return {
              count: prevState.count + 1
            }
          }, () => {
            console.log('count1', this.state.count) // 13
          })

          console.log('count', this.state.count) // 10
        } }>加1</button> 
        { this.state.count }
      </div>
    );
  }
}

export default App;
```

> updater 函数中接收的 `state` 和 `props` 都保证为最新。updater 的返回值会与 `state` 进行浅合并。

### 6.4.2 传递对象

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
import App from './02_state/04App_setState_object'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(<App />)

```

`src/02_state/04App_setState_object.jsx`

```jsx
// src/02_state/04App_setState_object.jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    count: 10
  }
  render() {
    return (
      <div>
        <button onClick={ () => {
          this.setState({
            count: this.state.count + 1
          }, () => {
            console.log('count1', this.state.count)
          })
          this.setState({
            count: this.state.count + 1
          }, () => {
            console.log('count2', this.state.count)
          })
          this.setState({
            count: this.state.count + 1
          }, () => {
            console.log('count3', this.state.count)
          })
          console.log('count', this.state.count)
        } }>加1</button> 
        { this.state.count }
      </div>
    );
  }
}

export default App;
```

> 这种形式的 `setState()` 是异步的，并且在同一周期内会对多个 `setState` 进行批处理,相当于
>
> ```
> 
> Object.assign(
>    prevState,
>    {count: this.state.count + 1},
>    {count: this.state.count + 1},
>    ...
> )
> ```
>
> 后调用的 `setState()` 将覆盖同一周期内先调用 `setState` 的值，因此商品数仅增加一次。**如果后续状态取决于当前状态,建议使用 updater 函数的形式代替**（前面案例已经实现）。或者在第二个参数中再继续操作。

> 思考题：
>
> 1.[何时以及为什么 `setState()` 会批量执行？](https://stackoverflow.com/a/48610973/458193)
>
> 2.[为什么不直接更新 `this.state`？](https://github.com/facebook/react/issues/11527#issuecomment-360199710)



# 七、生命周期

组件的生命周期可分成三个状态：

- Mounting(挂载,初始化)：已插入真实 DOM
- Updating(更新,运行时)：正在被重新渲染
- Unmounting(卸载,销毁)：已移出真实 DOM

生命周期图谱可以参考链接：https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/

> 类组件如何实现类似vue的计算属性： https://zh-hans.reactjs.org/blog/2018/06/07/you-probably-dont-need-derived-state.html#what-about-memoization
>
> ```sh
> $ cnpm i memoize-one -S
> ```
>
> `src/index.js`
>
> ```jsx
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> import App from './02_state/05App_computed'
> 
> // import './style.stylus'
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(<App />)
> 
> ```
>
> `src/02_state/05App_computed.jsx`
>
> ```jsx
> // src/02_state/05App_computed.jsx
> import React, { Component } from 'react';
> 
> class Search extends Component {
>   state = {
>     word: ''
>   }
>   render () {
>     const filterList = this.props.list.filter(item => {
>       return item.includes(this.state.word)
>     })
>     return (
>       <div>
>         <input value={ this.state.word } onInput={(event) => {
>           console.log(event)
>           this.setState({
>             word: event.target.value
>           })
>         }}/> { this.state.word }
> 
>         {/* 遍历数据 map */}
>         {
>           filterList.map((item, index) => {
>             return <p key = {index}> { item }</p>
>           })
>         }
>       </div>
>     )
>   }
> }
> 
> class App extends Component {
>   state = {
>     list: [
>       "a",
>       "ab",
>       "abc",
>       "b",
>       "bc",
>       "d"
>     ]
>   }
>   render() {
>     return (
>       <div>
>         <Search list={ this.state.list } />
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> ```jsx
> // src/02_state/05App_computed.jsx
> import React, { Component } from 'react';
> import computed from 'memoize-one'
> class Search extends Component {
>   state = {
>     word: '',
>     count: 10
>   }
>   // fiter = (list, word) => { // 改变 count的 值，代码也会执行
>   //   console.log('666')
>   //   return list.filter(item => {
>   //     return item.includes(word)
>   //   })
>   // }
>   fiter = computed((list, word) => { // 只有 list 或者 word 发生改变，代码才会执行
>     console.log('666')
>     return list.filter(item => {
>       return item.includes(word)
>     })
>   })
>   render () {
>     const filterList = this.fiter(this.props.list, this.state.word)
>     return (
>       <div>
>         <button onClick={() =>{
>            this.setState({ count: this.state.count + 1 })
>         }}>加1</button>{ this.state.count}
>         <input value={ this.state.word } onInput={(event) => {
>           console.log(event)
>           this.setState({
>             word: event.target.value
>           })
>         }}/> { this.state.word }
> 
>         {/* 遍历数据 map */}
>         {
>           filterList.map((item, index) => {
>             return <p key ={ index }> { item }</p>
>           })
>         }
>       </div>
>     )
>   }
> }
> 
> class App extends Component {
>   state = {
>     list: [
>       "a",
>       "ab",
>       "abc",
>       "b",
>       "bc",
>       "d"
>     ]
>   }
>   render() {
>     return (
>       <div>
>         <Search list={ this.state.list } />
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> 函数式组件可以使用 useMemo 实现计算属性
>
> 类组件可以通过 componentDidUpdate 实现 watch 监听功能
>
> 函数式组件可以通过 useEffect 第二个参数实现 类似于watch 监听功能

## 7.1 三个阶段

### 7.1.1 挂载时阶段-装载时阶段

当组件实例被创建并插入 DOM 中时，其生命周期调用顺序如下：

- `constructor()`: 在 React 组件挂载之前，会调用它的构造函数。

  > 如果不需要对类组件添加初始化数据以及绑定事件，那么就不需要写 `constructor`

- `static getDerivedStateFromProps()`: 在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。

- `render()`: render() 方法是 class 组件中唯一必须实现的方法。

- `componentDidMount()`: 在组件挂载后（插入 DOM 树中）立即调用。

render() 方法是 class 组件中唯一必须实现的方法，其他方法可以根据自己的需要来实现。

### 7.1.2 更新时阶段 - 运行时阶段

每当组件的 state 或 props 发生变化时，组件就会更新。

当组件的 props 或 state 发生变化时会触发更新。组件更新的生命周期调用顺序如下：

- `static getDerivedStateFromProps()`: 在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。根据 shouldComponentUpdate() 的返回值，判断 React 组件的输出是否受当前 state 或 props 更改的影响。
- `shouldComponentUpdate()`:当 props 或 state 发生变化时，shouldComponentUpdate() 会在渲染执行之前被调用。
- `render()`: render() 方法是 class 组件中唯一必须实现的方法。
- `getSnapshotBeforeUpdate()`: 在最近一次渲染输出（提交到 DOM 节点）之前调用。
- `componentDidUpdate()`: 在更新后会被立即调用,如果你需要**执行副作用**（例如，数据提取或动画）以响应 props 中的更改。

render() 方法是 class 组件中唯一必须实现的方法，其他方法可以根据自己的需要来实现。

### 7.1.3 卸载时阶段 - 销毁阶段

当组件从 DOM 中移除时会调用如下方法：

- `componentWillUnmount()`: 在组件卸载及销毁之前直接调用。

### 7.1.4 Error boundaries

* static getDerivedStateFromError
* componentDidCatch

[Error boundaries](https://zh-hans.reactjs.org/docs/error-boundaries.html) 是 React 组件，它会在其子组件树中的任何位置捕获 JavaScript 错误，并记录这些错误，展示降级 UI 而不是崩溃的组件树。Error boundaries 组件会捕获在渲染期间，在生命周期方法以及其整个树的构造函数中发生的错误。

> 项目中需要使用的最多的生命周期的钩子函数为 `render`, `componentDidMount`,`componentDidUpdate`,`componentWillUnmount`

> 详细介绍范例：https://zhuanlan.zhihu.com/p/392532496

```jsx
// src/ErrorBoundary.jsx
import React from 'react'
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 更新 state 使下一次渲染能够显示降级后的 UI
    return { hasError: true };
  }

  // try catch 
  componentDidCatch(error, errorInfo) {
    // 你同样可以将错误日志上报给服务器
    // logErrorToMyService(error, errorInfo);
    console.log(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 你可以自定义降级后的 UI 并渲染
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}

export default ErrorBoundary
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
import App from './02_state/06App_lifeCycle'
import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'



const root = ReactDOM.createRoot(document.getElementById('root'))
// ++++++++++<App root={ root }/>
root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/02_state/06App_lifeCycle.jsx`

```jsx
// src/02_state/06App_lifeCycle.jsx
import React, { Component } from 'react';

// 挂载阶段 - 初始化阶段
//   constructor 初始化状态以及事件绑定
//   static getDerivedStateFromProps 让组件在 props 变化时更新 state
//   render
//   componentDidMount  等同于vue中的mounted

// 更新阶段 - 运行时阶段
//   static getDerivedStateFromProps
//   shouldComponentUpdate react组件提升性能的关键 返回值false 表示不更新
//   render
//   getSnapshotBeforeUpdate
//   componentDidUpdate  除了类似vue的updated,还可以实现watch的功效，条件请求数据

// 卸载阶段 - 销毁阶段
//  componentWillUnmount


class App extends Component {
  // constructor (props) { // Useless constructor
  //   super(props)
  // }
  state = { count: 100 }
  // static getDerivedStateFromProps (props, state) { // 一般不使用
  //   // getDerivedStateFromProps 会在调用 render 方法之前调用，并且在初始挂载及后续更新时都会被调用。
  //   // 它应返回一个对象来更新 state，如果返回 null 则不更新任何内容
  //   // state 的值在任何时候都取决于 props
  // }

  componentDidMount () {
    // 等同于 vue中的 mounted
    // 数据请求，实例化操作，DOM操作，定时器，计时器，订阅数据变化
    // 修改状态
    this.setState({ count: this.state.count + 100 })
  }

  shouldComponentUpdate (nextProps, nextState) {
    // 可以作为react组件的性能优化的手段，但是也要慎用
    return true
    // return false
  }
  // getSnapshotBeforeUpdate(prevProps, prevState) {
  //   // 在最近一次渲染输出（提交到 DOM 节点）之前调用。
  //   // 它使得组件能在发生更改之前从 DOM 中捕获一些信息（例如，滚动位置）。
  //   // 此生命周期方法的任何返回值将作为参数传递给 componentDidUpdate()。
  // }

  componentDidUpdate(prevProps, prevState, snapshot) {
    // 参照vue中 updated, 也可以实现类似vue中的watch的功能
    // 实例化操作，DOM操作, 特定条件可以请求数据以及修改数据
    // if (this.props.userID !== prevProps.userID) { // 监听数据的变化
    //   this.fetchData(this.props.userID);
    // }
  }

  componentWillUnmount () {
    // 参照vue  beforeDestory beforeUnmount
    // 清理对象，取消订阅，消除定时器计时器
    // 当count的值等于210的时候销毁组件
  }
  render() {
    // 挂载阶段 依据初始化数据渲染数据
    // 更新阶段 当该组件的状态或者属性发生改变时触发此函数，也就输数据的改变引起视图的二次渲染
    return (
      <div>
        <p>{ this.state.count }</p>
        <button onClick={ () => {
          if (this.state.count === 210) {
            console.log('销毁组件')
            // 销毁组件
            this.props.root.unmount()
          } else {
            this.setState({ count: this.state.count + 1 } )
          }
        } }>加</button>
      </div>
    );
  }
}

export default App;
```

> ? shouldComponentUpdate 作为组件提升性能的一个关键，但是不要轻易使用，可以考虑使用 PureComponent
>
> ```js
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> // import App from './02_state/05App_computed'
> // import App from './02_state/06App_lifeCycle'
> import App from './02_state/07App_PureComponent'
> import ErrorBoundary from './ErrorBoundary'
> 
> // import './style.stylus'
> 
> 
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(
>   <ErrorBoundary>
>     <App root={ root }/>
>   </ErrorBoundary>
> )
> 
> ```
>
> ```jsx
> import React, { Component, PureComponent } from 'react';
> class Child1 extends Component {
>   shouldComponentUpdate () {
>     const random = Math.random()
>     console.log(random)
>     if (random > 0.5) {
>       return true
>     } else {
>       return false
>     }
>   }
>   render() {
>     console.log('child1')
>     return (
>       <div>
>         child1
>       </div>
>     );
>   }
> }
> /**
>  * shouldComponentUpdate方法仅作为性能优化的方式而存在。
>  * 不要企图依靠此方法来“阻止”渲染，因为这可能会产生 bug。
>  * 你应该考虑使用内置的 PureComponent 组件，
>  * 而不是手动编写 shouldComponentUpdate()。
>  * PureComponent 会对 props 和 state 进行浅层比较，并减少了跳过必要更新的可能性。
>  */
> class Child2 extends PureComponent {
>   render() {
>     console.log('child2')
>     return (
>       <div>
>         child2
>       </div>
>     );
>   }
> }
> class App extends Component {
>   state = {
>     count: 10
>   }
>   render() {
>     return (
>       <div>
>         <button onClick={ () => this.setState({ count: this.state.count + 1}) }>加1</button>{this.state.count}
>         <Child1 />
>         <Child2 />
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> 
>
> 

## 7.2 两个时期

将应用的渲染过程分为`mount`阶段（应用首次渲染）和`update`阶段（应用状态更新），无论在`mount`阶段还是`update`阶段，都会经历两个子阶段，一个是`render`阶段，一个是`commit`阶段。

**mount时**：
在`render`阶段会根据jsx对象构建新的`workInProgressFiber`树，然后将相应的`fiber`节点标记为`Placement`，表示这个`fiber`节点需要被插入到`dom`树中，然后会这些带有副作用的`fiber`节点加入一条叫做`Effect List`的链表中。
在`commit`阶段会遍历`render`阶段形成的`Effect List`，执行链表上相应`fiber`节点的副作用，比如`Placement`插入，或者执行`Passive`（useEffect的副作用）。将这些副作用应用到真实节点上
**update时**：
在`render`阶段会根据最新状态的jsx对象对比`current Fiber`，再构建新的`workInProgressFiber`树，这个对比的过程就是`diff算法`，`diff算法`又分成单节点的对比和多节点的对比，对比的过程中同样会经历收集副作用的过程，也就是将对比出来的差异标记出来，加入`Effect List`中，这些对比出来的副作用例如：`Placement`（插入）、`Update`(更新)、`Deletion`（删除）等。
在`commit`阶段同样会遍历`Effect List`，将这些fiber节点上的副作用应用到真实节点上。

>  参考链接: https://blog.csdn.net/bemystery/article/details/121897223

## 7.3 入门理解React Fiber架构

在 React 16 之前，`VirtualDOM` 的更新采用的是`Stack`架构实现的，也就是循环递归方式。不过，这种对比方式有明显的缺陷，就是一旦任务开始进行就无法中断，如果遇到应用中组件数量比较庞大，那么`VirtualDOM` 的层级就会比较深，带来的结果就是主线程被长期占用，进而阻塞渲染、造成卡顿现象。

为了避免出现卡顿等问题，我们必须保障在执行更新操作时计算时不能超过16ms，如果超过16ms，就需要先暂停，让给浏览器进行渲染，后续再继续执行更新计算。而`Fiber`架构就是为了支持“可中断渲染”而创建的。

在`React`中，`Fiber`使用了一种新的数据结构`fiber tree`，它可以把虚拟`dom tree`转换成一个链表，然后再执行遍历操作，而链表在执行遍历操作时是支持断点重启的，示意图如下。
![image.png](https://segmentfault.com/img/bVc1w1a)

官方介绍中，`Fiber` 被理解为是一种数据结构，但是我们也可以将它理解为是一个执行单元。

`Fiber` 可以理解为一个执行单元，每次执行完一个执行单元，`React Fiber`就会检查还剩多少时间，如果没有时间则将控制权让出去，然后由浏览器执行渲染操作。`React Fiber` 与浏览器的交互流程如下图。
![image.png](https://segmentfault.com/img/bVc1w1b)

可以看到，`React` 首先向浏览器请求调度，浏览器在执行完一帧后如果还有空闲时间，会去判断是否存在待执行任务，不存在就直接将控制权交给浏览器；如果存在就会执行对应的任务，执行完一个新的任务单元之后会继续判断是否还有时间，有时间且有待执行任务则会继续执行下一个任务，否则将控制权交给浏览器执行渲染，这个流程是循环进行的。

所以，我们可以将`Fiber` 理解为一个执行单元，并且这个执行单元必须是一次完成的，不能出现暂停。并且，这个小的执行单元在执行完后计算之后，可以移交控制权给浏览器去响应用户，从而提升了渲染的效率。

在官方的文档中，`Fiber` 被解释为是一种数据结构，即链表结构。在链表结构中，每个` Virtual DOM` 都可以表示为一个 `fiber`，如下图所示。
![image.png](https://segmentfault.com/img/bVc1w1c)
通常，一个 `fiber`包括了` child`（第一个子节点）、`sibling`（兄弟节点）、`return`（父节点)等属性，`React Fiber` 机制的实现，就是依赖于上面的数据结构。

通过介绍，我们知道`Fiber`使用的是链表结构，准确的说是**单链表树结构**。为了方便理解 `Fiber` 的遍历过程，下面我们就看下`Fiber`链表结构。

![image.png](https://segmentfault.com/img/bVc1w1d)

在上面的例子中，每一个单元都包含了`payload`（数据）和`nextUpdate`（指向下一个单元的指针）两个元素

> 参考链接：https://segmentfault.com/a/1190000042271919

# 八、事件绑定

React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同：

- React 事件的命名采用小驼峰式（camelCase），而不是纯小写。

- 使用 JSX 语法时你需要传入一个函数作为事件处理函数，而不是一个字符串。\

  ```
  * 保证是一个函数
  * 如果需要使用this关键词，保证this指向
  ```

## 8.1 ES5语法绑定事件

### 8.1.1 无参数的绑定

#### 8.1.1.1 方法一

* 定义函数

```
handleClick(e) { // e - 事件对象
  e.preventDefault();
  // doSomething ...
}
```

* constructor 中绑定函数执行上下文

```
this.handleClick = this.handleClick.bind(this);
```

* jsx中调用

```
<button onClick={this.hanleClick} />
```

#### 8.1.1.1 方法二

* 定义函数

```
handleClick(e) { // e - 事件对象
  e.preventDefault();
  // doSomething ...
}
```

* jsx 中调用

```
<button onClick={this.hanleClick.bind(this)} />
```

### 8.1.2 有参数的绑定

* 定义函数

```
handleClick(param1, param2, e) {
  e.preventDefault();
  // do something ...
}
```

> 注意此时无论多少个参数， e 一定放在最后

* jsx 中调用

```
<button onClick={this.hanleClick.bind(this, 'x', 'xx')} />
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

import App from './03_event/01App_event_es5'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/03_event/01App_event_es5.jsx`

```jsx
// src/03_event/01App_event_es5.jsx
import React, { Component } from 'react';

class App extends Component {

  constructor (props) {
    super(props)
    this.noParamsHandlerFun1a = this.noParamsHandlerFun1.bind(this)
  }

  noParamsHandlerFun1 (event) {
    console.log('无参数绑定方法一', this, event)
    // event: SyntheticBaseEvent 
  }
  noParamsHandlerFun2 (event) {
    console.log('无参数绑定方法二', this, event)
    // event: SyntheticBaseEvent 
  }

  // 如果需要传递参数并且需要事件对象，那么事件对象为函数的最后一个参数
  hasParamsHandlerFun (a, b, event) {
    console.log('有参数绑定方法', this, a, b, event)
  }

  render() {
    return (
      <div>
        {/* 不推荐使用： 使用了构造函数 只是实现了this指向的改变 */}
        <button onClick={ this.noParamsHandlerFun1a }>无参数绑定方法一</button>
        {/* (function () {}).bind(this) */}
        <button onClick={ this.noParamsHandlerFun2.bind(this) }>无参数绑定方法二</button>
        <button onClick={ this.hasParamsHandlerFun.bind(this, '1', '2') }>有参数绑定方法</button>
      </div>
    );
  }
}

export default App;
```

## 8.2 ES6语法绑定事件

### 8.2.1 无参数绑定

#### 8.2.1.1 方法一

* 定义函数

```
handleClick = (e) => {
  e.preventDefault();
  // do something ...
}
```

* jsx中调用

```
<button onClick={this.hanleClick} />
```

> 比起 es 5 中的无参数函数的绑定调用， es 6 不需要使用 bind 函数；

#### 8.2.1.2 方法二

jsx中定义箭头函数

```
<button onClick={ () => {}} />
```

### 8.2.2 有参数绑定

#### 8.2.2.1 方法一

* 定义函数

```
handleClick = (param1, e) => {
  e.preventDefault();
  // do something ...
}
```

* jsx调用

```
<button onClick={this.hanleClick.bind(this, 'x')} />
```

> 有参数时，在绑定时依然要使用 bind；
> 并且参数一定要传，不然仍然存在 this 指向错误问题；

#### 8.2.2.2 方法二

* 定义函数

```
handleClick = (param1, e) => {
  // do something ...
}
```

* jsx调用

```
<button onClick={() => this.handleClick('c')} />
// 如果需要对 event 对象进行处理的话，需要写成下面的格式
<button onClick={(e) => this.handleClick('c', e)} />
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
import App from './03_event/02App_event_es6'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/03_event/02App_event_es6.jsx`

```jsx
// src/03_event/02App_event_es6.jsx
import React, { Component } from 'react';

class App extends Component {

  noParamsHandlerFun1 = (event) => {
    console.log('无参数绑定方法一', this, event)
    // event: SyntheticBaseEvent 
  }

  hasParamsHandlerFun1 (a, event) {
    console.log('有参数绑定方法一', this, event, a)
  }

  hasParamsHandlerFun2 = (a, event) => {
    console.log('有参数绑定方法二', this, event, a)
  }

  hasParamsHandlerFun3 (a) {
    console.log('有参数绑定方法三', this, a)
  }
  hasParamsHandlerFun4 = (a) => {
    console.log('有参数绑定方法四', this, a)
  }
  render() {
    return (
      <div>
        
        <button onClick={ this.noParamsHandlerFun1 }>无参数绑定方法一</button>
        <button onClick={ (event) => {
          console.log('无参数绑定方法二', this, event)
        } }>无参数绑定方法二</button>
        <button onClick={ this.hasParamsHandlerFun1.bind(this, '111') }>有参数绑定方法一</button>
        <button onClick={ this.hasParamsHandlerFun2.bind(this, '222') }>有参数绑定方法二</button>
        <button onClick={ (event) => {
          this.hasParamsHandlerFun3('6666')
        }}>有参数绑定方法三</button>
        {/* <button onClick={ (event) => {
          this.hasParamsHandlerFun4('8888')
        }}>有参数绑定方法四</button> */}
        <button onClick={ (event) => this.hasParamsHandlerFun4('8888') }>有参数绑定方法四</button>
      </div>
    );
  }
}

export default App;
```



## 8.3 合成事件的特点

> 以上案例react事件对象打印为 SyntheticBaseEvent，原生js事件打印为 PointerEvent

### 8.3.1 事件机制

* `react`自身实现了一套事件机制，包括事件的注册、事件的存储、事件的合成及执行等。
* `react` 的所有事件并没有绑定到具体的`dom`节点上而是绑定在了`document` 上，然后由统一的事件处理程序来派发执行。
* 通过这种处理，减少了事件注册的次数，另外`react`还在事件合成过程中，对不同浏览器的事件进行了封装处理，抹平浏览器之间的事件差异。

### 8.3.2 对合成事件的理解

（1）对原生事件的封装

> `react`会根据原生事件类型来使用不同的合成事件对象，比如: 聚焦合成事件对象`SyntheticFoucsEvent`（合成事件对象：`SyntheticEvent`是`react`合成事件的基类，定义了合成事件的基础公共属性和方法。合成事件对象就是在该基类上创建的，原生js的 事件对象为 `PointerEvent`）

（2）不同浏览器事件兼容的处理

>  在对事件进行合成时，`react`针对不同的浏览器，也进行了事件的兼容处理

### 8.3.3 事件机制的流程

#### 1、事件注册

> 在组件挂载阶段，根据组件内声明的事件类型-`onclick`，`onchange` 等，给 `document` 上添加事件 -`addEventListener`，并指定统一的事件处理程序 `dispatchEvent`。

#### 2、事件存储

> 完成事件注册后，将` react dom` ，事件类型，事件处理函数` fn `放入数组存储，组件挂载完成后，经过遍历把事件处理函数存储到 `listenerBank`(一个对象)中，缓存起来，为了在触发事件的时候可以查找到对应的事件处理方法去执行。

> 开始事件的存储，在` react` 里所有事件的触发都是通过 `dispatchEvent`方法统一进行派发的，而不是在注册的时候直接注册声明的回调，来看下如何存储的 。
> `react` 把所有的事件和事件类型以及`react` 组件进行关联，把这个关系保存在了一个` map`里，也就是一个对象里（键值对），然后在事件触发的时候去根据当前的 组件id和 事件类型查找到对应的 事件fn

#### 3、事件执行

> 1、进入统一的事件分发函数(`dispatchEvent`)
> 2、结合原生事件找到当前节点对应的`ReactDOMComponent`对象
> 3、开始 事件的合成
>
> > * 根据当前事件类型生成指定的合成对象
> > * 封装原生事件和冒泡机制
> > * 在 `listenerBank`事件池中查找事件回调并合成到 `event`(合成事件结束)
>
> 4.处理合成事件内的回调事件（事件触发完成 end）

### 8.3.4 合成事件、原生事件之间的冒泡执行关系

结论：

* 原生事件阻止冒泡肯定会阻止合成事件的触发。

* 合成事件的阻止冒泡不会影响原生事件。

原因：

* 浏览器事件的执行需要经过三个阶段，`捕获阶段-目标元素阶段-冒泡阶段`。

> 节点上的原生事件的执行是在目标阶段，然而合成事件的执行是在冒泡阶段，所以原生事件会先合成事件执行，然后再往父节点冒泡，所以原生事件阻止冒泡会阻止合成事件的触发，而合成事件的阻止冒泡不会影响原生事件。

> 理解react事件中出现的undefined：https://www.inte.net/news/275774.html

# 九、条件渲染

在 React 中，你可以创建不同的组件来封装各种你需要的行为。然后还可以根据应用的状态变化只渲染其中的一部分。

React 中的条件渲染和 JavaScript 中的一致，使用 JavaScript 操作符 if 或条件运算符来创建表示当前状态的元素，然后让 React 根据它们来更新 UI。

## 9.1 &&

你可以通过用花括号包裹代码在 JSX 中嵌入任何表达式 ，也包括 JavaScript 的逻辑与 &&，它可以方便地条件渲染一个元素。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

import App from './04_condition/01App_condition_yu'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/04_condition/01App_condition_yu.jsx`

```jsx
// src/04_condition/01App_condition_yu.jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    arr: [
      "第1封邮件",
      "第2封邮件",
      "第3封邮件",
      "第4封邮件",
      "第5封邮件"
    ]
  }

  readMail = (index) => {
    const list = this.state.arr
    list.splice(index, 1)
    this.setState({
      arr: list
    })
  }
  render() {
    return (
      <div>
        {
          // react中 使用 map 遍历渲染数组
          this.state.arr && this.state.arr.map((item, index) => {
            return (
              <div key={ index }>
                { item }
                <button onClick={ this.readMail.bind(this, index)}>阅读</button>
              </div>)
          })
        }
      </div>
    );
  }
}

export default App;
```

## 9.2 三元运算符

条件渲染的另一种方法是使用 JavaScript 的条件运算符:

```
condition ? true : false。
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
import App from './04_condition/02App_condition_san'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/04_condition/02_App_condition_san.jsx`

```jsx
// src/04_condition/02App_condition_san.jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    flag: false
  }

  changeFlag = (event) => {
    // console.log(event.target.checked)
    this.setState({
      flag: event.target.checked
    })
  }
  // 不可以在jsx中使用流程控制语句
  // 但是可以在流程控制语句中使用 jsx 代码
  render() {
    // return (
    //   <div>
    //     <input type="checkbox" onChange={ this.changeFlag } /> { this.state.flag + '' }

    //     { 
    //       this.state.flag ? (<div>
    //         11111111
    //       </div>) : (<div>
    //         22222222
    //       </div>)
    //     }
    //   </div>
    // );

    if (this.state.flag) {
      return (
        <div>
          <input type="checkbox" onChange={ this.changeFlag } /> { this.state.flag + '' }
          <div>11111</div>
        </div>
      )
    } else {
      return (
        <div>
          <input type="checkbox" onChange={ this.changeFlag } /> { this.state.flag + '' }
          <div>22222</div>
        </div>
      )
    }
  }
}

export default App;
```

## 9.3 动态className

Vue中有很方便的[动态绑定](https://so.csdn.net/so/search?q=动态绑定&spm=1001.2101.3001.7020)class属性的方式，v-bind:class,那么react怎么实现这样的效果呢？

> `<button class="btn btn-success btn-sm"></button>`
>
> `<button class="btn btn-danger btn-sm"></button>`
>
> `<button class="btn btn-warning btn-sm"></button>`

`{ this.state.type === 'success' ? 'btn btn-success btn-sm' : 'btn btn-sm'}`

通过classnames这个插件可以实现

```sh
$ cnpm i classnames -S
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
import App from './04_condition/03App_condition_classnames'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/04_condition/03App_condition_classname.jsx`

```jsx
// src/04_condition/02App_condition_san.jsx
import React, { Component } from 'react';
import classnames from 'classnames'
class App extends Component {
  state = {
    type: 'success'
  }
  render() {
    return (
      <div>
        <div>
          <button className='btn btn-success btn-sm'>success</button>
          <button className='btn btn-danger btn-sm'>danger</button>
          <button className='btn btn-warning btn-sm'>warning</button>
        </div>
        <div>
          <button className={ this.state.type === 'success' ? 'btn btn-success btn-sm' : 'btn btn-sm'}>success</button>
          <button className={ classnames({ btn: true, 'btn-danger': this.state.type === 'danger' }) }>danger</button>
          <button className='btn btn-warning btn-sm'>warning</button>
        </div>
      </div>
    )
  }
}

export default App;
```

> 补充：
>
> * **css-in-js**
>
> ```
> $ cnpm i styled-components -S
> ```
>
> `src/index.js`
>
> ```jsx
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> // import App from './02_state/05App_computed'
> // import App from './02_state/06App_lifeCycle'
> // import App from './02_state/07App_PureComponent'
> 
> // import App from './03_event/01App_event_es5'
> // import App from './03_event/02App_event_es6'
> 
> // import App from './04_condition/01App_condition_yu'
> // import App from './04_condition/02App_condition_san'
> // import App from './04_condition/03App_condition_classnames'
> import App from './04_condition/04App_condition_cssinjs'
> 
> import ErrorBoundary from './ErrorBoundary'
> 
> // import './style.stylus'
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(
>   <ErrorBoundary>
>     <App root={ root }/>
>   </ErrorBoundary>
> )
> 
> ```
>
> `src/04_condition/04App_condition_cssinjs.jsx`
>
> ```jsx
> // src/04_condition/04App_condition_cssinjs.jsx
> import React, { Component } from 'react';
> import styled from 'styled-components'
> 
> // css in js
> const Button = styled.button`
>   padding: 30px;
>   font-size: 20px;
> `
> 
> // 一般不推荐使用 css-in-js 特别是针对初学者
> class App extends Component {
>   render() {
>     return (
>       <div>
>         <Button />
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> * **模块化css**
>
> > 可以解决类似于 vue中 scoped 
>
> `src/index.js`
>
> ```jsx
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> // import App from './02_state/05App_computed'
> // import App from './02_state/06App_lifeCycle'
> // import App from './02_state/07App_PureComponent'
> 
> // import App from './03_event/01App_event_es5'
> // import App from './03_event/02App_event_es6'
> 
> // import App from './04_condition/01App_condition_yu'
> // import App from './04_condition/02App_condition_san'
> // import App from './04_condition/03App_condition_classnames'
> // import App from './04_condition/04App_condition_cssinjs'
> // import App from './04_condition/05App_style'
> import App from './04_condition/06App_module_css'
> 
> // import App from './05_list/01App_map'
> // import App from './05_list/02App_mutiple_map'
> 
> // import App from './06_form/01App_form copy'
> 
> 
> import ErrorBoundary from './ErrorBoundary'
> 
> // import './style.stylus'
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(
>   <ErrorBoundary>
>     <App root={ root }/>
>   </ErrorBoundary>
> )
> 
> ```
>
> `src/04_condition/06App_module_css.jsx`
>
> ```jsx
> // src/04_condition/06App_module_css.jsx
> import React, { Component } from 'react';
> import './style.css'
> // class App extends Component {
> //   render() {
> //     return (
> //       <div className='contianer'>
>         
> //       </div>
> //     );
> //   }
> // }
> 
> import style from './test.module.css'
> class App extends Component {
>   render() {
>     return (
>       <div className={ style.container }>
>         
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> `src/04_condition/test.module.css`
>
> ```css
> /* src/04_condition/test.module.css */
> .container {
>   width: 600px;
>   height: 600px;
>   background-color: #0f0;
> }
> ```
>
> 

## 9.4 动态style

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
import App from './04_condition/05App_style'


import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/04_condition/05App_style.jsx`

```jsx
// src/04_condition/05App_style.jsx

import React, { Component } from 'react';

class App extends Component {
  state = {
    color: '#333',
    size: 12
  }
  changeColor = (event) => {
    this.setState( { color: event.target.value })
  }
  changeSize = (event) => {
    this.setState( { size: event.target.value })
  }
  render() {
    // <></> 空标签
    // React.Fragment 空标签
    return (
      <React.Fragment>
        <input type="color" onChange={ this.changeColor }/>
        <input type="number" onChange={ this.changeSize } value = { this.state.size }/>
        <p style={ { fontSize: this.state.size + 'px', color: this.state.color } }>
        随着你的应用不断变大，更有意识的去关注应用状态如何组织，以及数据如何在组件之间流动会对你很有帮助。冗余或重复的状态往往是缺陷的根源。
        在本节中，你将学习如何组织好状态，如何保持状态更新逻辑的可维护性，以及如何跨组件共享状态。
        </p>
      </React.Fragment>
    );
  }
}

export default App;
```



# 十、列表渲染

* map()方法、key

使用 map() 方法遍历数组

> 组件接收数组参数，每个列表元素分配一个 key，不然会出现警告 **a key should be provided for list items**，意思就是需要包含 key：

Keys 可以在 DOM 中的某些元素被增加或删除的时候帮助 React 识别哪些元素发生了变化。因此你应当给数组中的每一个元素赋予一个确定的标识。

一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据的` id `作为元素的 key

当元素没有确定的 id 时，你可以使用他的序列号索引` index` 作为` key`

> 如果列表可以重新排序，我们不建议使用索引来进行排序，因为这会导致渲染变得很慢。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'

import App from './05_list/01App_map'


import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/05_list/01_App_map.jsx`

```jsx
// src/05_list/01App_map.jsx
import React, { Component } from 'react';

class App extends Component {
  state = {
    arr: ['a', 'b', 'c', 'd']
  }
  topAdd = () => {
    const list = this.state.arr
    list.unshift('e')
    this.setState({
      arr: list
    })
  }
  afterAdd = () => {
    const list = this.state.arr
    list.push('f')
    this.setState({
      arr: list
    })
  }
  deleteItem = () => {
    const list = this.state.arr
    list.splice(1, 1)
    this.setState({
      arr: list
    })
  }
  // render() {
  //   return (
  //     <div>
  //       <button onClick={ this.topAdd }>数组前加入e</button>
  //       <button onClick={ this.afterAdd }>数组后加入f</button>
  //       <button onClick={ this.deleteItem }>删除第2个数据</button>
  //       <ul>
  //         {
  //           // this.state.arr.forEach((item, index) => {
  //           //   return <li key = { index }>{ item }</li>
  //           // })
  //           this.state.arr.map((item, index) => {
  //             // return <li key = { index }>{ item }</li>
  //             return <li key = { item }>{ item }</li>
  //           })
  //         }
  //       </ul>
  //     </div>
  //   );
  // }

  render() {
    // 不要在jsx中使用 for 循环系列
    // 但是可以在for循环系列中 组合 jsx 代码 --- 数组的特性
    const testArr = [<h1 key={ 1 }>第一章节</h1>, <h1 key={ 2 }>第二章节</h1>]
    const listArr = []
    // for in 索引  for of 元素  forEach
    // for (let i in this.state.arr) {
    //   listArr.push(<li key = { i }>{ this.state.arr[i] }</li>)
    // }
    // for (let item of this.state.arr) {
    //   listArr.push(<li key = { item }>{ item }</li>)
    // }
    this.state.arr.forEach((item, index) => {
      listArr.push(<li key = { index }>{ item }</li>)
    }) 
    return (
      <div>
        {
          testArr
        }
        <ul>
          { listArr }
        </ul>
      </div>
    );
  }
}

export default App;
```

> 接口 `http://121.89.205.189:3000/api/city/sortCity`
>
> 实现多层遍历
>
> `src/index.js`
>
> ```js
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> // import App from './02_state/05App_computed'
> // import App from './02_state/06App_lifeCycle'
> // import App from './02_state/07App_PureComponent'
> 
> // import App from './03_event/01App_event_es5'
> // import App from './03_event/02App_event_es6'
> 
> // import App from './04_condition/01App_condition_yu'
> // import App from './04_condition/02App_condition_san'
> // import App from './04_condition/03App_condition_classnames'
> // import App from './04_condition/04App_condition_cssinjs'
> // import App from './04_condition/05App_style'
> 
> // import App from './05_list/01App_map'
> import App from './05_list/02App_mutiple_map'
> 
> 
> import ErrorBoundary from './ErrorBoundary'
> 
> // import './style.stylus'
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(
>   <ErrorBoundary>
>     <App root={ root }/>
>   </ErrorBoundary>
> )
> 
> ```
>
> `src/05_list/02_App_mutiple_map.jsx`
>
> ```jsx
> // src/05_list/02App_mutiple_map.jsx
> import React, { Component } from 'react';
> 
> class App extends Component {
>   state = {
>     list: []
>   }
> 
>   componentDidMount () {
>     fetch('http://121.89.205.189:3000/api/city/sortCity')
>       .then(res => res.json())
>       .then(res => {
>         console.log(JSON.parse(res.data))
>         this.setState({
>           list: JSON.parse(res.data)
>         })
>       })
>     // fetch('http://121.89.205.189:3000/api/pro/list')
>     //   .then(res => res.json())
>     //   .then(res => {
>     //     console.log(res.data)
>     //   })
>     // fetch('http://121.89.205.189:3000/api/banner/list')
>     //   .then(res => res.json())
>     //   .then(res => {
>     //     console.log(res.data)
>     //   })
>   }
> 
> 
>   render() {
>     return (
>       <div>
>         <ul>
>           {
>             this.state.list.map((item) => (
>               <li key = { item.letter }>
>                 <h4>{ item.letter }</h4>
>                 {
>                   item.data.map(itm => {
>                     return <p key = { itm.cityId }>{ itm.name }</p>
>                   })
>                 } 
>               </li>
>             ))
>           }
>         </ul>
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```

# 十一、表单绑定

在 React 里，HTML 表单元素的工作方式和其他的 DOM 元素有些不同，这是因为表单元素通常会保持一些内部的 state。例如这个纯 HTML 表单只接受一个名称：

```html
<form>
  <label>
    名字:
    <input type="text" name="name" />
  </label>
  <input type="submit" value="提交" />
</form>
```

> 此表单具有默认的 HTML 表单行为，即在用户提交表单后浏览到新页面。如果你在 React 中执行相同的代码，它依然有效。但大多数情况下，使用 JavaScript 函数可以很方便的处理表单的提交， 同时还可以访问用户填写的表单数据。实现这种效果的标准方式是使用“受控组件”。
>
> 表单元素的value值受 state的控制
>
> 

## 11.1 各种表单的绑定与取值

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

import App from './06_form/01App_form'


import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/06_form/01App_form.jsx`

```JSX
// src/06_form/01App_form.jsx
import React, { Component } from 'react';
import computed from 'memoize-one'

class App extends Component {
  state = {
    userName: '',
    password: '',
    lesson: 3,
    sex: '男',
    hobby: ['🎱'],
    note: '',
    flag: false
  }

  // changeUserName = (event) => {
  //   console.log(event.target.name) // userName
  //   this.setState({
  //     userName: event.target.value
  //   })
  // }

  // changePassword = (event) => {
  //   console.log(event.target.name) // password
  //   this.setState({
  //     password: event.target.value
  //   })
  // }

  changeValue = (event) => {
    console.log(event.target.name)
    // console.log(typeof event.target.value)
    this.setState({ // [event.target.name] 代表变量 ===== 对象的key值为变量 ，可以写为 [key]: value
      // btn.style.color = "red"  ===>  btn.style['color'] = 'red'
      [event.target.name]: event.target.value
    })
  }

  filter = computed((lesson) => {
    console.log('computed')
    return this.state.lesson * 1 === 1 ? '一阶段' : (this.state.lesson * 1 === 2 ? '二阶段' : '三阶段')
  })


  changeHobby = (event) => {
    console.log(event.target.value, event.target.checked)
    if (event.target.checked) { // 之前数组没有，现在有，效果上选中
      const list = this.state.hobby
      list.push(event.target.value)
      console.log('list', list)
      this.setState({
        hobby: list
      })
    } else {
      const index = this.state.hobby.findIndex(item => item === event.target.value)
      const list = this.state.hobby
      list.splice(index, 1)
      console.log('list', list)
      this.setState({
        hobby: list
      })
    }
  }

  changeFlag = (event) => {
    this.setState({
      flag: event.target.checked
    })
  }
  render() {
    const lessonStr = this.filter(this.state.lesson)
    return (
      <div>
        {/* 受控组件：表单的value的属性受state的控制 */}
        {/* <div>
          <input type="text" name='userName' placeholder='用户名' value = { this.state.userName } onInput={ this.changeUserName } /> { this.state.userName }
        </div>
        <div>
          <input type="password" name='password' placeholder='密码' value = { this.state.password } onInput={ this.changePassword } /> { this.state.password }
        </div> */}
        <div>
          <input type="text" name='userName' placeholder='用户名' value = { this.state.userName } onInput={ this.changeValue } /> { this.state.userName }
        </div>
        <div>
          <input type="password" name='password' placeholder='密码' value = { this.state.password } onInput={ this.changeValue } /> { this.state.password }
        </div>
        <div>
          <select name="lesson" value={this.state.lesson} onChange={ this.changeValue }>
            <option value={1}>一阶段</option>
            <option value={2}>二阶段</option>
            <option value={3}>三阶段</option>
          </select> 
          {/* -- { this.state.lesson * 1 === 1 ? '一阶段' : (this.state.lesson * 1 === 2 ? '二阶段' : '三阶段') } */}
          { lessonStr }
        </div>
        <div>
          <input type="radio" name='sex' onChange={ this.changeValue } checked = { this.state.sex === "男" } value="男" />男
          <input type="radio" name='sex' onChange={ this.changeValue } checked = { this.state.sex === "女" } value="女" />女 ---- { this.state.sex }
        </div>
        <div>
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🏀") } name="hobby" value="🏀" />🏀
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("⚽") } name="hobby" value="⚽" />⚽
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🎱") } name="hobby" value="🎱" />🎱
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🏓") } name="hobby" value="🏓" />🏓
          --- { 
            this.state.hobby.map((item, index) => (<span key={index}>{item}</span>))
          }
        </div>
        <div>
          <textarea name="note" placeholder='备注' value={ this.state.note } onInput={ this.changeValue } ></textarea> { this.state.note }
        </div>
        <input type="checkbox" checked = { this.state.flag } onChange={ this.changeFlag } />同意******协议 -- { this.state.flag + ''}
        <button disabled = { this.state.flag === false }>注册</button>
      </div>
    );
  }
}

export default App;
```

```jsx
// src/06_form/01App_form.jsx
import React, { Component } from 'react';
import computed from 'memoize-one'

class App extends Component {
  state = {
    userName: '',
    password: '',
    lesson: 3,
    sex: '男',
    hobby: ['🎱'],
    note: '',
    flag: false,
    wan: ['🏀','⚽','🎱', '🏓', '🏸', '⚾']
  }

  // changeUserName = (event) => {
  //   console.log(event.target.name) // userName
  //   this.setState({
  //     userName: event.target.value
  //   })
  // }

  // changePassword = (event) => {
  //   console.log(event.target.name) // password
  //   this.setState({
  //     password: event.target.value
  //   })
  // }

  changeValue = (event) => {
    console.log(event.target.name)
    // console.log(typeof event.target.value)
    this.setState({ // [event.target.name] 代表变量 ===== 对象的key值为变量 ，可以写为 [key]: value
      // btn.style.color = "red"  ===>  btn.style['color'] = 'red'
      [event.target.name]: event.target.value
    })
  }

  filter = computed((lesson) => {
    console.log('computed')
    return this.state.lesson * 1 === 1 ? '一阶段' : (this.state.lesson * 1 === 2 ? '二阶段' : '三阶段')
  })


  changeHobby = (event) => {
    console.log(event.target.value, event.target.checked)
    if (event.target.checked) { // 之前数组没有，现在有，效果上选中
      const list = this.state.hobby
      list.push(event.target.value)
      console.log('list', list)
      this.setState({
        hobby: list
      })
    } else {
      const index = this.state.hobby.findIndex(item => item === event.target.value)
      const list = this.state.hobby
      list.splice(index, 1)
      console.log('list', list)
      this.setState({
        hobby: list
      })
    }
  }

  changeFlag = (event) => {
    this.setState({
      flag: event.target.checked
    })
  }
  render() {
    const lessonStr = this.filter(this.state.lesson)
    return (
      <div>
        {/* 受控组件：表单的value的属性受state的控制 */}
        {/* <div>
          <input type="text" name='userName' placeholder='用户名' value = { this.state.userName } onInput={ this.changeUserName } /> { this.state.userName }
        </div>
        <div>
          <input type="password" name='password' placeholder='密码' value = { this.state.password } onInput={ this.changePassword } /> { this.state.password }
        </div> */}
        <div>
          <input type="text" name='userName' placeholder='用户名' value = { this.state.userName }  onInput={ this.changeValue } /> { this.state.userName }
        </div>
        <div>
          <input type="password" name='password' placeholder='密码' value = { this.state.password } onInput={ this.changeValue } /> { this.state.password }
        </div>
        <div>
          <select name="lesson" value={this.state.lesson} onChange={ this.changeValue }>
            <option value={1}>一阶段</option>
            <option value={2}>二阶段</option>
            <option value={3}>三阶段</option>
          </select> 
          {/* -- { this.state.lesson * 1 === 1 ? '一阶段' : (this.state.lesson * 1 === 2 ? '二阶段' : '三阶段') } */}
          { lessonStr }
        </div>
        <div>
          <input type="radio" name='sex' onChange={ this.changeValue } checked = { this.state.sex === "男" } value="男" />男
          <input type="radio" name='sex' onChange={ this.changeValue } checked = { this.state.sex === "女" } value="女" />女 ---- { this.state.sex }
        </div>
        <div>
          {/* 
    
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🏀") } name="hobby" value="🏀" />🏀
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("⚽") } name="hobby" value="⚽" />⚽
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🎱") } name="hobby" value="🎱" />🎱
          <input type="checkbox" onChange={ this.changeHobby } checked = { this.state.hobby.includes("🏓") } name="hobby" value="🏓" />🏓
          
          */}
          {
            this.state.wan.map((item) => {
              return <div key = { item }><input type="checkbox"  onChange={ this.changeHobby } checked = { this.state.hobby.includes(item) } name="hobby" value={ item } />{ item }</div>
            })
          }
          --- { 
            this.state.hobby.map((item, index) => (<span key={index}>{item}</span>))
          }
        </div>
        <div>
          <textarea name="note" placeholder='备注' value={ this.state.note } onInput={ this.changeValue } ></textarea> { this.state.note }
        </div>
        <input type="checkbox" checked = { this.state.flag } onChange={ this.changeFlag } />同意******协议 -- { this.state.flag + ''}
        <button disabled = { this.state.flag === false }>注册</button>
      </div>
    );
  }
}

export default App;
```



## 11.2 受控表单以及受控组件

在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 [`setState()`](https://react.docschina.org/docs/react-component.html#setstate)来更新。

我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。

> `input`、`textarea`、`select` 受控组件： value的属性受了 `state` 的控制
>
> * 使用了受控组件，一定要写 `value` 属性以及`onChange`事件
>
> `radio`、'checkbox' 受控组件: checked 的属性受了`state`的控制
>
> 如果需要设置默认值，那么需要通过 `defaultValue` 以及`defaultChecked`设置

案例如上

# 十二、状态提升

在 React 中，将多个组件中需要共享的 state 向上移动到它们的最近共同父组件中，便可实现共享 state。这就是所谓的“状态提升”。

## 12.1 非父子组件通信

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

import App from './07_state_up/01App_parent_child_value'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/07_state_up/01App_parent_child_value.jsx`

```jsx
// src/07_state_up/01App_parent_child_value.jsx
import React, { Component } from 'react';

class Child1 extends Component {
  state = {
    count: 10
  }
  render () {
    return (
      <div>
        <button onClick={ () => this.setState({ count: this.state.count + 1 }) }>加1</button>{this.state.count}
      </div>
    )
  }
}

class Child2 extends Component {
  state = {
    count: 10
  }
  render () {
    return (
      <div>
        <button onClick={ () => this.setState({ count: this.state.count + 1 }) }>加1</button>{this.state.count}
      </div>
    )
  }
}

class App extends Component {
  render() {
    return (
      <div>
        <Child1 />
        <hr />
        <Child2 />
      </div>
    );
  }
}

export default App;
```

> 我们发现Child1和Child2都是两个独立的个体，并没有实现数据共享

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
import App from './07_state_up/02App_state_up'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/07_state_up/02App_state_up.jsx`

```jsx
// src/07_state_up/02App_state_up.jsx
import React, { Component } from 'react';

class Child1 extends Component {
  state = {
    count: 10
  }
  render () {
    return (
      <div>
        <button onClick={ this.props.onClick }>加1</button>{this.props.count}
      </div>
    )
  }
}

class Child2 extends Component {
  state = {
    count: 10
  }
  render () {
    return (
      <div>
        <button onClick={ this.props.onClick }>加1</button>{this.props.count}
      </div>
    )
  }
}

class App extends Component {
  state = {
    count: 100
  }

  add = () => {
    this.setState({
      count: this.state.count + 1
    })
  }
  render() {
    return (
      <div>
        <Child1 count = { this.state.count } onClick = { this.add } />
        <hr />
        <Child2 count = { this.state.count } onClick = { this.add } />
      </div>
    );
  }
}

export default App;
```



## 12.2 状态提升解读

实现方式是 利用最近的共同的父级组件中，用`props`的方式传过去到两个子组件，`props`中传的是一个`setState`的方法，通过子组件触发`props`传过去的方法，进而调用父级组件的`setState`的方法，改变了父级组件的`state`，调用父级组件的`add`方法，进而同时改变了两个子级组件的`数据`。

这是 两个有关连的**同级组件**的传值，因为`react`的单项数据流，所以不在两个组件中进行传值，而是提升到 最近的共同的父级组件中，改变父级的`state`,进而影响了两个子级组件的`render`。

> 注意如果两个组件是同级组件（这两个组件的父组件是同一个）才考虑状态提升共享数据

# 十三、组合vs继承

React 有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

## 13.1 理解组件化

**组件化是React的核心思想**：

* 组件化提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。

* 任何的应用都会被抽象成一颗组件树。

**组件化思想的应用**：

* 有了组件化的思想，我们在之后的开发中就要充分的利用它。

* 尽可能的将页面拆分成一个个小的、可复用的组件。

* 这样让我们的代码更加方便组织和管理，并且扩展性也更强。

  

**React的组件相对于Vue更加的灵活和多样，按照不同的方式可以分成很多类 组件**：

* 根据组件的定义方式，可以分为：函数组件(Functional Component )和类组件(Class Component)；

> vue 中有没有类组件和函数式组件？vue2中有
>
> ```vue
> <template>
> 
> </template>
> 
> <script>
> export default {
>  props:[],
>  data () {
>  	return {}
>  }
> }
> </script>
> ```
>
> 
>
> * vue中的函数式组件 - 无状态组件，所有的数据来源均来自父组件
>
> ```jsx
> <template functional>
>   <div>{{props.msg}}</div>
> </template>
> 
> ```
>
> * vue 中的类组件 - 兼容ts时
>
> ```
> <template>
>   <div>hello vue</div>
> </template>
> 
> class Home extends Vue {} // export default {}
> ```

* 根据组件内部是否有状态需要维护，可以分成：无状态组件(Stateless Component )和有状态组件(Stateful Component)；

* 根据组件的不同职责，可以分成：**展示型组件**(Presentational Component - 只做数据的展示，一般不需要写更多的业务逻辑-数据请求不出现在展示型组件-顶多发出请求的指令-具体的请求交给容器型组件)和**容器型组件**(Container Component - 负责给展示性组件提供数据以及处理展示型组件需要的具体的业务逻辑) - 状态管理器-更容易理解；

  

这些概念有很多重叠，但是他们最主要是关注数据逻辑和UI展示的分离：

* 函数组件、无状态组件、展示型组件主要关注UI的展示；
* 类组件、有状态组件、容器型组件主要关注数据逻辑；

## 13.2 使用组合而非继承实现React组件化

有些组件无法提前知晓它们子组件的具体内容，建议这些组件使用一个特殊的 `children` prop 来将他们的子组件传递到渲染结果中。

`参照5.3章节`

少数情况下，你可能需要在一个组件中预留出几个“洞”。这种情况下，我们可以不使用 `children`，而是自行约定：将所需内容传入 props，并使用相应的 prop。

## 13.3 封装Modal弹窗

> vue3中的 teleport

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

import App from './08_com/01App_modal'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/08_com/01App_modal.jsx`

```jsx
// src/08_com/01App_modal.jsx

import React, { Component } from 'react';

class Modal extends Component {
  render() {
    return (
      <div
        ref = 'test'
        style={{
          width: '100%',
          minHeight: 600,
          display: 'flex',
          justifyContent: 'center',
          alignItems: 'center',
          backgroundColor: 'rgba(0, 0, 0, 0.5)'
        }}
        onClick={ (event) => {
          console.log(event.target, this.refs.test) // 使用形如vue中的ref的特性
          if (event.target === this.refs.test) {
            this.props.onClick()
          }
        } }
      >
        <p style={{ width: 600, height: 300, backgroundColor: '#fff' }}>
          这里是模态框
          <button onClick={ (event) => {
            this.props.onClick()
          }  }>关闭</button>
        </p>
      </div>
    );
  }
}



class App extends Component {
  state = {
    flag: false
  }

  closeModal = () => {{
    this.setState({
      flag: false
    })
  }}
  render () {
    return (
      <>
        <button onClick={ () => {
          this.setState({
            flag: true
          })
        }}>打开模态框</button>
        {
          this.state.flag ? <Modal onClick = { this.closeModal }/> : null
        }
      </>
    )
  }
}
export default App;
```

> 审查元素发现 Modal 组件是渲染在原来的组件的位置的，如果想要让它渲染到不同的位置怎么办呢？

## 13.4 ReactDOM.createPortal()

普通的组件，子组件的元素将挂载到父组件的DOM节点中。

有时需要将元素渲染到DOM中的不同位置上去，这是就用到的portal的方法。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
import App from './08_com/02App_portal'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/08_com/02App_portal.jsx`

```jsx
// src/08_com/02App_portal.jsx

import React, { Component } from 'react';
import ReactDOM from 'react-dom';

class Modal extends Component {
  render() {
    return ReactDOM.createPortal(
      <div
        ref = 'test'
        style={{
          width: '100%',
          minHeight: 600,
          display: 'flex',
          justifyContent: 'center',
          alignItems: 'center',
          backgroundColor: 'rgba(0, 0, 0, 0.5)'
        }}
        onClick={ (event) => {
          // console.log(event.target, this.refs.test) // 使用形如vue中的ref的特性
          if (event.target === this.refs.test) {
            this.props.onClick()
          }
        } }
      >
        <p style={{ width: 600, height: 300, backgroundColor: '#fff' }}>
          这里是模态框
          <button onClick={ (event) => {
            this.props.onClick()
          }  }>关闭</button>
        </p>
      </div>
    , document.getElementsByTagName('body')[0]);
  }
}



class App extends Component {
  state = {
    flag: false
  }

  closeModal = () => {{
    this.setState({
      flag: false
    })
  }}
  render () {
    return (
      <>
        <button onClick={ () => {
          this.setState({
            flag: true
          })
        }}>打开模态框</button>
        {
          this.state.flag ? <Modal onClick = { this.closeModal }/> : null
        }
      </>
    )
  }
}
export default App;
```

> 一个 portal 的典型用例是当父组件有 `overflow: hidden` 或 `z-index` 样式时，但你需要子组件能够在视觉上“跳出”其容器。例如，对话框、悬浮卡以及提示框：



# 十四、上下文Context

## 14.1 理解上下文、作用及其特点

Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。

Context 设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言。

Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。请谨慎使用，因为这会使得组件的复用性变差。

## 14.2 使用React.createContext()

### 14.2.1 逐层传递数据

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

import App from './09_context/01App_next_value'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/09_context/01App_next_value.jsx`

```jsx
// src/09_context/01App_next_value.jsx

import React, { Component } from 'react';

class Third extends Component {
  render () {
    return (
      <div>
        后代组件 - { this.props.bao }
      </div>
    )
  }
}
class Second extends Component {
  render () {
    return (
      <div>
        <Third bao={this.props.bao }/>
      </div>
    )
  }
}

class First extends Component {
  render () {
    return (
      <div>
        <Second  bao={this.props.bao }/>
      </div>
    )
  }
}

class App extends Component {
  state = { bao: '传家宝' }
  render() {
    return (
      <div>
        <First bao={this.state.bao }/>
      </div>
    );
  }
}

export default App;
```

### 14.2.2 使用Context传值

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
import App from './09_context/02App_context'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/09_context/02App_context.jsx`

```jsx
// src/09_context/02App_context.jsx
import React, { Component } from 'react';

// 1.创建一个context对象
const MyContext = React.createContext()


// 3.如果后代组件是类组件，则通过 static contextType = MyContext 静态属性接收值，最后通过 this.context 获取值
//    等价写法为定义组件后， 则通过 组件.contextType = MyContext, 静态属性接收值，最后通过 this.context 获取值
class Third extends Component {
  static contextType = MyContext
  render () {
    return (
      <div>
        后代组件 - { this.context }
      </div>
    )
  }
}
class Second extends Component {
  render () {
    return (
      <div>
        { this.context }
        <Third />
      </div>
    )
  }
}
Second.contextType = MyContext

class First extends Component {
  render () {
    return (
      <div>
        <Second />
      </div>
    )
  }
}

class App extends Component {
  state = { bao: '传家宝' }
  render() {
    return (
      <div>
        {/* 2.在祖先组件向后代组件传值的地方，添加一个组件  自定义上下文对象.Provider, 通过value属性传递数据 */}
        <MyContext.Provider value = { this.state.bao }>
          <First/>
        </MyContext.Provider>
        
      </div>
    );
  }
}

export default App;

```

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
import App from './09_context/03App_context_consumer'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

```jsx
// src/09_context/03App_context_consumer.jsx
import React, { Component } from 'react';

// 1.创建一个context对象
const MyContext = React.createContext()


// 3.如果后代组件是类组件，则通过 static contextType = MyContext 静态属性接收值，最后通过 this.context 获取值
//    等价写法为定义组件后， 则通过 组件.contextType = MyContext, 静态属性接收值，最后通过 this.context 获取值
class Third extends Component {
  static contextType = MyContext
  render () {
    return (
      <div>
        后代组件 - { this.context }
      </div>
    )
  }
}
class Second extends Component {
  render () {
    return (
      <div>
        { this.context }
        <Third />
      </div>
    )
  }
}
Second.contextType = MyContext

// 4.使用 上下文对象.Consumer接收值，在其内部，实现一个函数，函数的参数即为需要传递的数据
class First extends Component {
  render () {
    return (
      <div>
        11111
        <MyContext.Consumer>
          {
            (val) => {
              return <span>{ val }</span>
            }
          }
        </MyContext.Consumer>
        <Second />
      </div>
    )
  }
}

class App extends Component {
  state = { bao: '传家宝' }
  render() {
    return (
      <div>
        {/* 2.在祖先组件向后代组件传值的地方，添加一个组件  自定义上下文对象.Provider, 通过value属性传递数据 */}
        <MyContext.Provider value = { this.state.bao }>
          <First/>
        </MyContext.Provider>
        
      </div>
    );
  }
}

export default App;

```



### 14.2.3 传递多个值

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
import App from './09_context/04App_context_multiple_value'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/09_context/03App_context_multiple_value.jsx`

```jsx
// src/09_context/04App_context_multiple_value.jsx
import React, { Component, useContext } from 'react';
const ThemeContext = React.createContext()
const LanguageContext = React.createContext()

// function Second () {
//   return (
//     <div>
//       second
//       <LanguageContext.Consumer>
//           {
//             (lanVal) => {
//               return (
//                 <div>
//                   语言为： { lanVal }
//                   <ThemeContext.Consumer>
//                     {
//                       (themeVal) => {
//                         return (
//                           <div>主题为: { themeVal }</div>
//                         )
//                       }
//                     }
//                   </ThemeContext.Consumer>
//                 </div>
//               )
//             }
//           }
//         </LanguageContext.Consumer>
//     </div>
//   );
// }

function Second () { // 函数式组件 hooks
  const theme = useContext(ThemeContext)
  const language = useContext(LanguageContext)
  return (
    <div>
      语言为111： { language }
      <div>
      主题为111: { theme }
      </div>
    </div>
  )
}

class First extends Component {
  render() {
    return (
      <div>
        first
        <Second />
        <LanguageContext.Consumer>
          {
            (lanVal) => {
              return (
                <div>
                  语言为： { lanVal }
                  <ThemeContext.Consumer>
                    {
                      (themeVal) => {
                        return (
                          <div>主题为: { themeVal }</div>
                        )
                      }
                    }
                  </ThemeContext.Consumer>
                </div>
              )
            }
          }
        </LanguageContext.Consumer>

      </div>
    );
  }
}

class App extends Component {
  state = {
    language: '中文'
  }
  changeValue = (event) => {
    this.setState({
      language: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input type="radio" name="language" value="中文" onChange={ this.changeValue }/>中文
        <input type="radio" name="language" value="英文" onChange={ this.changeValue }/>英文 - { this.state.language }
        <LanguageContext.Provider value={ this.state.language }>
          <ThemeContext.Provider value = "red">
              <First />
          </ThemeContext.Provider>
        </LanguageContext.Provider>
       
      </div>
    );
  }
}

export default App;
```

上述案例，还可以通过一个上下文对象传递多个值 - 自行测试

> vue中祖先组件传递变量给后代组件，祖先组件状态发生改变，后代组件不会立即更新
>
> ```vue
> <script>
> import { computed } from '@vue/reactivity';
> import Child from './Child.vue'
> 
> export default {
>   components: {
>     Child
>   },
>   data () {
>     return {
>       count: 10
>     }
>   },
>   provide () {
>     return {
>       count: computed(() => this.count)
>     }
>   }
> }
> </script>
> <template>
>   <button @click="count++">加1</button> {{ count  }}
>   <Child />
> </template>
> 
> <style scoped>
> </style>
> 
> ```
>
> ```vue
> <template>
>   count: {{  count  }}
> </template>
> 
> <script>
> export default {
>   inject: ['count']
> }
> </script>
> ```
>
> 

> 如果浏览器安装过 react的开发者工具,打开之后发现上述代码，都显示为 `Context.Provider` 和 `Context.Consumer`,不好区分
>
> 加入 上下文对象的 `displayName`

### 14.2.4 displayName

```
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
import App from './09_context/05App_context_multiple_value_displayName'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```



```jsx
// src/09_context/05App_context_multiple_value_displayName.jsx
import React, { Component, useContext } from 'react';
const ThemeContext = React.createContext()
const LanguageContext = React.createContext()

ThemeContext.displayName = 'ThemeContext'
LanguageContext.displayName = 'LanguageContext'

// function Second () {
//   return (
//     <div>
//       second
//       <LanguageContext.Consumer>
//           {
//             (lanVal) => {
//               return (
//                 <div>
//                   语言为： { lanVal }
//                   <ThemeContext.Consumer>
//                     {
//                       (themeVal) => {
//                         return (
//                           <div>主题为: { themeVal }</div>
//                         )
//                       }
//                     }
//                   </ThemeContext.Consumer>
//                 </div>
//               )
//             }
//           }
//         </LanguageContext.Consumer>
//     </div>
//   );
// }

function Second () { // 函数式组件 hooks
  const theme = useContext(ThemeContext)
  const language = useContext(LanguageContext)
  return (
    <div>
      语言为111： { language }
      <div>
      主题为111: { theme }
      </div>
    </div>
  )
}

class First extends Component {
  render() {
    return (
      <div>
        first
        <Second />
        <LanguageContext.Consumer>
          {
            (lanVal) => {
              return (
                <div>
                  语言为： { lanVal }
                  <ThemeContext.Consumer>
                    {
                      (themeVal) => {
                        return (
                          <div>主题为: { themeVal }</div>
                        )
                      }
                    }
                  </ThemeContext.Consumer>
                </div>
              )
            }
          }
        </LanguageContext.Consumer>

      </div>
    );
  }
}

class App extends Component {
  state = {
    language: '中文'
  }
  changeValue = (event) => {
    this.setState({
      language: event.target.value
    })
  }
  render() {
    return (
      <div>
        <input type="radio" name="language" value="中文" onChange={ this.changeValue }/>中文
        <input type="radio" name="language" value="英文" onChange={ this.changeValue }/>英文 - { this.state.language }
        <LanguageContext.Provider value={ this.state.language }>
          <ThemeContext.Provider value = "red">
              <First />
          </ThemeContext.Provider>
        </LanguageContext.Provider>
       
      </div>
    );
  }
}

export default App;
```

## 14.3 常见应用场景解读

* 共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言
* 配合`react hooks `中的`useReducer`可以实现轻量的 `redux`



# 十五、高阶组件

高阶组件（HOC）是 React 中用于复用组件逻辑的一种高级技巧。

HOC 自身不是 React API 的一部分，它是一种基于 React 的组合特性而形成的设计模式。

具体而言，**高阶组件是参数为组件，返回值为新组件的函数。**

## 15.1 理解高阶组件、作用及其特点

一个高阶组件只是一个包装了另外一个 React 组件的 函数。
这种形式通常实现为一个函数，本质上是一个类工厂。

.实现了对原有组件的`增强和优化`。

可以对原有`组件`中的state, props和逻辑执行增删改操作, 一般用于代码`重用`和组件`增强优化`

## 15.2 高阶组件语法详解

我们想要我们的组件通过自动注入一个版权信息

### 15.2.1 组件嵌套

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

import App from './10_hoc/01App_more_use'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/10_hoc/01App_more_use.jsx`

```jsx
// src/10_hoc/01App_more_use.jsx

import React, { Component } from 'react';
class Footer extends Component {
  render() {
    return (
      <div>
        Copyright © 2023 Meta Platforms, Inc.
      </div>
    );
  }
}
class Page1 extends Component {
  render() {
    return (
      <div>
        页面1
        <Footer />
      </div>
    );
  }
}
class Page2 extends Component {
  render() {
    return (
      <div>
        页面2
        <Footer />
      </div>
    );
  }
}
class App extends Component {
  render() {
    return (
      <div>
        <Page1 />

        <hr />

        <Page2 />
      </div>
    );
  }
}

export default App;
```

> 通过`Footer`组件可以复用jsx代码，但是其余的业务逻辑代码显得无能为力，可以通过高阶组件来实现

### 15.2.2 高阶组件

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
import App from './10_hoc/02App_more_use_hoc'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <App root={ root }/>
  </ErrorBoundary>
)

```

`src/10_hoc/02App_more_use_hoc.jsx`

```jsx
// src/10_hoc/02App_more_use_hoc.jsx

import React, { Component } from 'react';
// class Footer extends Component {
//   render() {
//     return (
//       <div>
//         Copyright © 2023 Meta Platforms, Inc.
//       </div>
//     );
//   }
// }
// 高阶组件其实就是一个函数，把组件作为参数，返回一个新的组件
// 也可以把需要复用的业务逻辑 写到 高阶组件内部
function withFooter (Com) {
  return class extends Component {

    componentDidMount () {
      console.log('1111')
    }

    render() {
      return (
        <div>
          <Com />
          {/* 公共部分 */}
          Copyright © 2023 Meta Platforms, Inc.
        </div>
      );
    }
  }
}

class Page1 extends Component {
  render() {
    return (
      <div>
        页面1
      </div>
    );
  }
}

const MyPage1 = withFooter(Page1)

class Page2 extends Component {
  render() {
    return (
      <div>
        页面2
      </div>
    );
  }
}
Page2 = withFooter(Page2)
class App extends Component {
  render() {
    return (
      <div>
        <MyPage1 />

        <hr />

        <Page2 />
      </div>
    );
  }
}

export default App;
```

> 先执行了 组件的生命周期，后执行了高阶组件的生命周期
>
> 但是以后再复用组件的业务时，可以选用函数式组件的自定义hooks

## 15.3 常见应用场景解读

1.需要代码重用时, react如果有多个组件都用到了`同一段逻辑`, 这时,就可以把共同的逻辑部分提取出来,利用高阶组件的形式将这段逻辑整合到每一个组件中, 从而减少代码的逻辑重复

2.需要组件`增强优化时`, 比如我们在项目中使用的组件有些不是自己写的, 而是从网上撸下来的,但是`第三方`写的组件可能比较复杂, 有时不能完全满足需求, 但第三方组件不易修改, 此时也可以用高阶组件,在不修改原始组件的前提下, 对组件添加满足实际开发需求的功能

 3.可以对原有`组件`中的state, props和逻辑执行增删改操作, 一般用于代码`重用`和组件`增强优化`

 4.也可以用来替换 `mixins` 混入

> 父组件和高阶组件有什么区别？
>
> * 首先从逻辑的执行流程上来看，高阶组件确实和父组件比较相像
> * 但是`高阶`组件强调的是`逻辑`的抽象。高阶组件是一个`函数`，函数关注的是逻辑；
> * `父组件`是一个组件，组件主要关注的是`UI/DOM`。如果逻辑是与DOM直接相关的，那么这部分逻辑适合放到父组件中实现；
> * 如果逻辑是与DOM不直接相关的，那么这部分逻辑适合使用高阶组件抽象，如数据校验、请求发送等。

> 如果高阶组件和 调用的组件拥有相同的业务逻辑，谁的优先级更高
>
> ```js
> // src/index.js
> import React from 'react'
> import ReactDOM from 'react-dom/client'
> 
> // import App from './01_props/01Parent_Child'
> // import App from './01_props/02Parent_Child_value'
> // import App from './01_props/03Parent_Child_default'
> // import App from './01_props/04Parent_Child_type'
> // import App from './01_props/05App_props_children'
> // import App from './01_props/06_App_multiple_props_children'
> // import App from './01_props/07App_mouse_tracker'
> // import App from './01_props/08App_render_props'
> 
> // import App from './02_state/01App_state_es6'
> // import App from './02_state/02App_state_es7'
> // import App from './02_state/03App_setState_function'
> // import App from './02_state/04App_setState_object'
> // import App from './02_state/05App_computed'
> // import App from './02_state/06App_lifeCycle'
> // import App from './02_state/07App_PureComponent'
> 
> // import App from './03_event/01App_event_es5'
> // import App from './03_event/02App_event_es6'
> 
> // import App from './04_condition/01App_condition_yu'
> // import App from './04_condition/02App_condition_san'
> // import App from './04_condition/03App_condition_classnames'
> // import App from './04_condition/04App_condition_cssinjs'
> // import App from './04_condition/05App_style'
> // import App from './04_condition/06App_module_css'
> 
> // import App from './05_list/01App_map'
> // import App from './05_list/02App_mutiple_map'
> 
> // import App from './06_form/01App_form copy'
> 
> // import App from './07_state_up/01App_parent_child_value'
> // import App from './07_state_up/02App_state_up'
> 
> // import App from './08_com/01App_modal'
> // import App from './08_com/02App_portal'
> 
> // import App from './09_context/01App_next_value'
> // import App from './09_context/02App_context'
> // import App from './09_context/03App_context_consumer'
> // import App from './09_context/04App_context_multiple_value'
> // import App from './09_context/05App_context_multiple_value_displayName'
> 
> // import App from './10_hoc/01App_more_use'
> // import App from './10_hoc/02App_more_use_hoc'
> import App from './10_hoc/03App_more_use_hoc'
> 
> import ErrorBoundary from './ErrorBoundary'
> 
> // import './style.stylus'
> 
> const root = ReactDOM.createRoot(document.getElementById('root'))
> 
> root.render(
>   <ErrorBoundary>
>     <App root={ root }/>
>   </ErrorBoundary>
> )
> 
> ```
>
> ```jsx
> // src/10_hoc/03App_more_use_hoc.jsx
> 
> import React, { Component } from 'react';
> // class Footer extends Component {
> //   render() {
> //     return (
> //       <div>
> //         Copyright © 2023 Meta Platforms, Inc.
> //       </div>
> //     );
> //   }
> // }
> // 高阶组件其实就是一个函数，把组件作为参数，返回一个新的组件
> // 也可以把需要复用的业务逻辑 写到 高阶组件内部
> // 高级组件中和组件内的逻辑代码，以组件为优先执行，比如生命周期，先执行组件的，后执行高阶组件的
> function withFooter (Com) {
>   return class extends Component {
>     state = { count: 100}
>     componentDidMount () {
>       console.log('1111')
>     }
> 
>     render() {
>       return (
>         <div>
>           <Com />
>           <button onClick={ () => this.setState({ count: this.state.count + 10 })}>加10</button> { this.state.count }
>           {/* 公共部分 */}
>           Copyright © 2023 Meta Platforms, Inc.
>         </div>
>       );
>     }
>   }
> }
> 
> class Page1 extends Component {
>   componentDidMount () {
>     console.log('页面1')
>   }
>   render() {
>     return (
>       <div>
>        页面1
>       </div>
>     );
>   }
> }
> 
> const MyPage1 = withFooter(Page1)
> 
> class Page2 extends Component {
>   componentDidMount () {
>     console.log('页面2')
>   }
>   render() {
>     return (
>       <div>
>         页面2
>       </div>
>     );
>   }
> }
> Page2 = withFooter(Page2)
> class App extends Component {
>   render() {
>     return (
>       <div>
>         <MyPage1 />
> 
>         <hr />
> 
>         <Page2 />
>       </div>
>     );
>   }
> }
> 
> export default App;
> ```
>
> 

# 十六、ref

Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

## 16.1 ref访问DOM

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
// import App from './10_hoc/02App_more_use_hoc'
// import App from './10_hoc/03App_more_use_hoc'

import App from './11_ref/01App_ref_dom'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/11_ref/01App_ref_dom.jsx`

```jsx
// src/11_ref/01App_ref_dom.jsx
import React, { Component } from 'react';

// 开启React的严格模式
// https://react.docschina.org/docs/strict-mode.html
// react 18 为严格模式引入了一个全新的仅用于开发环境的检查操作。
// 每当第一次安装组件时，这个新的检查将自动卸载并重新安装每个组件，并在第二次挂载时恢复之前的 state。
// 慎用严格模式

class App extends Component {
  componentDidMount () {
    console.log(document.getElementById('btn'))
    console.log(this.refs.btn)
  }
  render() {
    return (
      <div>
        <button id='btn'>按钮1</button>
        <button ref='btn'>按钮2</button>
      </div>
    );
  }
}

export default App;
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
// import App from './10_hoc/02App_more_use_hoc'
// import App from './10_hoc/03App_more_use_hoc'

// import App from './11_ref/01App_ref_dom'
import App from './11_ref/02App_ref_dom'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/11_ref/02App_ref_dom.jsx`

```jsx
// src/11_ref/02App_ref_dom.jsx
import React, { Component } from 'react';

// 开启React的严格模式
// https://react.docschina.org/docs/strict-mode.html
// react 18 为严格模式引入了一个全新的仅用于开发环境的检查操作。
// 每当第一次安装组件时，这个新的检查将自动卸载并重新安装每个组件，并在第二次挂载时恢复之前的 state。
// 慎用严格模式

class App extends Component {
  btnRef = React.createRef()
  componentDidMount () {
    console.log(document.getElementById('btn'))
    // console.log(this.refs.btn)
    console.log(this.btnRef.current)
  }
  render() {
    return (
      <div>
        <button id='btn'>按钮1</button>
        {/* <button ref='btn'>按钮2</button> */}
        <button ref={ this.btnRef }>按钮2</button>
      </div>
    );
  }
}

export default App;
```

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
// import App from './10_hoc/02App_more_use_hoc'
// import App from './10_hoc/03App_more_use_hoc'

// import App from './11_ref/01App_ref_dom'
// import App from './11_ref/02App_ref_dom'
import App from './11_ref/03App_Ref_Com'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

```jsx
// src/11_ref/03App_Ref_Com.jsx
import React, { Component } from 'react';
class Child extends Component {
  state = {
    count: 10
  }
  add = () => {
    this.setState({
      count: this.state.count + 1    })
  }
  render () {
    return (
      <div>child - { this.state.count }</div>
    )
  }
}
class App extends Component {
  childRef = React.createRef()
  componentDidMount () {
    console.log(this.childRef.current)
  }
  render() {
    return (
      <div>
        <button id='btn'>按钮1</button>
        <button onClick={ () => {
          this.childRef.current.add()
        }}>加1</button>
        <Child ref = { this.childRef } / >
      </div>
    );
  }
}

export default App;
```



> 如果在上述案例中，在`FunCom`组件中上使用ref，发现报了警告信息
>
> Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
>
> 其原因是，函数式组件使用ref，**必须使用React.forwardRef()方法二次包装**
>

## 16.2 详解ref转发

**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。**

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
// import App from './10_hoc/02App_more_use_hoc'
// import App from './10_hoc/03App_more_use_hoc'

// import App from './11_ref/01App_ref_dom'
// import App from './11_ref/02App_ref_dom'
// import App from './11_ref/03App_Ref_Com'
import App from './11_ref/04App_Ref_Com_forward'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/11_ref/03App_Ref_Com.jsx`

```jsx
// src/11_ref/03App_Ref_Com.jsx
import React, { Component } from 'react';
// forwardRef - 高阶组件
const Child  = React.forwardRef((props, ref) => {
  return (
    <div ref = { ref }>
      child
    </div>
  )
})
class App extends Component {
  childRef = React.createRef()
  componentDidMount () {
    this.childRef.current.style.color = 'red'
  }
  render() {
    return (
      <div>
        <Child ref = { this.childRef } / >
      </div>
    );
  }
}

export default App;
```

## 16.3 使用ref注意事项

- 当 `ref` 属性用于 HTML 元素时，使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
- **你不能在函数组件上使用 `ref` 属性**，因为他们没有实例。如果非要使用，实际上是转发ref（父组件中获取到了子组件的某个DOM,并且可以继续操作DOM）

- - https://react.docschina.org/docs/hooks-reference.html#usedebugvalue)

## 16.4 非受控组件

只读的表单元素，比如`<input type = "file" />`

上传图片前预览的操作

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './01_props/01Parent_Child'
// import App from './01_props/02Parent_Child_value'
// import App from './01_props/03Parent_Child_default'
// import App from './01_props/04Parent_Child_type'
// import App from './01_props/05App_props_children'
// import App from './01_props/06_App_multiple_props_children'
// import App from './01_props/07App_mouse_tracker'
// import App from './01_props/08App_render_props'

// import App from './02_state/01App_state_es6'
// import App from './02_state/02App_state_es7'
// import App from './02_state/03App_setState_function'
// import App from './02_state/04App_setState_object'
// import App from './02_state/05App_computed'
// import App from './02_state/06App_lifeCycle'
// import App from './02_state/07App_PureComponent'

// import App from './03_event/01App_event_es5'
// import App from './03_event/02App_event_es6'

// import App from './04_condition/01App_condition_yu'
// import App from './04_condition/02App_condition_san'
// import App from './04_condition/03App_condition_classnames'
// import App from './04_condition/04App_condition_cssinjs'
// import App from './04_condition/05App_style'
// import App from './04_condition/06App_module_css'

// import App from './05_list/01App_map'
// import App from './05_list/02App_mutiple_map'

// import App from './06_form/01App_form copy'

// import App from './07_state_up/01App_parent_child_value'
// import App from './07_state_up/02App_state_up'

// import App from './08_com/01App_modal'
// import App from './08_com/02App_portal'

// import App from './09_context/01App_next_value'
// import App from './09_context/02App_context'
// import App from './09_context/03App_context_consumer'
// import App from './09_context/04App_context_multiple_value'
// import App from './09_context/05App_context_multiple_value_displayName'

// import App from './10_hoc/01App_more_use'
// import App from './10_hoc/02App_more_use_hoc'
// import App from './10_hoc/03App_more_use_hoc'

// import App from './11_ref/01App_ref_dom'
// import App from './11_ref/02App_ref_dom'
// import App from './11_ref/03App_Ref_Com'
// import App from './11_ref/04App_Ref_Com_forward'
import App from './11_ref/05App_file'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

```jsx
// src/11_ref/05App_file.jsx

import React, { Component } from 'react';

// input text  DOM.value
// input radio DOM.checked
// input file  DOM.files 获取选中的文件，以数组的形式展示，每一个文件即为一个元素

class App extends Component {
  fileRef = React.createRef()
  linkRef = React.createRef()
  state = {
    url: '',
    link: ''
  }

  selectFile = () => {
    const file = this.fileRef.current.files[0]
    const that = this
    console.log(file)
    // 依据文件信息 ，读取文件信息，讲图片文件以base64的形式输出
    // 读取文件信息 - 文件阅读器 - 以base64格式读取
    const reader = new FileReader()
    reader.readAsDataURL(file) // 输出base64
    // 读取图片是一个异步操作 ----  图片是一像素一像素渲染的
    reader.onload = function () { // 需要的this指向，reader.onload 内要通过 result 读取结果信息，this指向的是reader阅读器
      console.log(this.result)
      that.setState({
        url: this.result
      })
    }
  }
  upload = () => {
    /**
     * axios.post('', {
     *  img: this.state.url,
     *  link: this.state.link
     * })
     */
  }
  render() {
    return (
      <div>
        <input type="text" ref={ this.linkRef } placeholder='点击链接'/>
        <input ref = { this.fileRef } type="file" onChange={ this.selectFile }/>
        <img src={ this.state.url } alt="" />
        <button onClick={ this.upload }>上传</button>
      </div>
    );
  }
}

export default App;
```



# 十七、hooks

## 17.1 为什么使用hooks

React 没有提供将可复用性行为“附加”到组件的途径（例如，把组件连接到 store）。如果你使用过 React 一段时间，你也许会熟悉一些解决此类问题的方案，比如 [render props](https://gitee.com/link?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Frender-props.html) 和 [高阶组件](https://gitee.com/link?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Fhigher-order-components.html)。

**Hook 使你在非 class 的情况下可以使用更多的 React 特性。** 从概念上讲，React 组件一直更像是函数。而 Hook 则拥抱了函数，同时也没有牺牲 React 的精神原则。Hook 提供了问题的解决方案，无需学习复杂的函数式或响应式编程技术。

**react 18版本以前hooks**

- [基础 Hook](https://react.docschina.org/docs/hooks-reference.html#basic-hooks)
  - [`useState`](https://react.docschina.org/docs/hooks-reference.html#usestate)
  - [`useEffect`](https://react.docschina.org/docs/hooks-reference.html#useeffect)
  - [`useContext`](https://react.docschina.org/docs/hooks-reference.html#usecontext)
- [额外的 Hook](https://react.docschina.org/docs/hooks-reference.html#additional-hooks)
  - [`useReducer`](https://react.docschina.org/docs/hooks-reference.html#usereducer)
  - [`useCallback`](https://react.docschina.org/docs/hooks-reference.html#usecallback)
  - [`useMemo`](https://react.docschina.org/docs/hooks-reference.html#usememo)
  - [`useRef`](https://react.docschina.org/docs/hooks-reference.html#useref)
  - [`useImperativeHandle`](https://react.docschina.org/docs/hooks-reference.html#useimperativehandle)
  - [`useLayoutEffect`](https://react.docschina.org/docs/hooks-reference.html#uselayouteffect)
  - `useDebugValue`

## 17.2常见的hooks

### 17.2.1 useState

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './12_hooks/01App_useState'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/12_hooks/01App_useState.jsx`

```jsx
// src/12_hooks/01App_useState.jsx

import React, { useState } from 'react';

const App = () => {
  // 设置了一个 变量 count，初始值为10，修改状态的方法叫 setCount
  const [count, setCount] = useState(10)
  /**
   * const [num, setNum] = useState(1)
   * const [arr, setArr] = useState([])
   * const [obj, setObj] = useState({})
   * const [flag, setFlag] = useState(false)
   */

  return (
    <div>
      { count }
      <button onClick={ () => {
        // setCount(count + 1)
        // setCount(count + 1)
        setCount((prevCount) => prevCount + 1)
        // setCount((prevCount) => prevCount + 1)
      }}>加1</button>
    </div>
  );
};

export default App;
```



`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
import App from './12_hooks/02App_useState'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/12_hooks/02App_useState.jsx`

```jsx
// src/12_hooks/02App_useState.jsx

import React, { useState } from 'react';

const App = () => {

  const [obj, setObj] = useState({
    w: 100,
    h: 100,
    x: 0,
    y: 0
  })

  const move = (event) => {
    // setObj({ // ❌
    //   x: event.clientX,
    //   y: event.clientY
    // })

    // setObj({ ...obj, x: event.clientX, y: event.clientY })
    setObj(Object.assign({}, obj, { x: event.clientX, y: event.clientY }))
  }

  return (
    <div style={ {width:'100vw', height: '100vh'} } onMouseMove={ move }>
      <div style={{ width: obj.w, height: obj.h, backgroundColor: '#f66'}}>
        宽为： { obj.w }, 高为: { obj.h }
      </div>
      <div>x: { obj.x }, y: { obj.y }</div>
    </div>
  );
};

export default App;
```

> 思考：如上效果虽然已经实现，但是设计合理吗

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
import App from './12_hooks/03App_useState'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    <React.StrictMode>
      <App root={ root }/>
    </React.StrictMode>
  </ErrorBoundary>
)

```

`src/12_hooks/03App_useState.jsx`

```jsx
// src/12_hooks/03App_useState.jsx

import React, { useState } from 'react';

const App = () => {

  // const box = { w: 100, h: 100 }
  const [box] = useState({ w: 100, h: 100 })
  const [poi, setPoi] = useState({ x: 0, y: 0 })


  const move = (event) => {
    setPoi({ 
      x: event.clientX,
      y: event.clientY
    })
  }

  return (
    <div style={ {width:'100vw', height: '100vh'} } onMouseMove={ move }>
      <div style={{ width: box.w, height: box.h, backgroundColor: '#0f0'}}>
        宽为： { box.w }, 高为: { box.h }
      </div>
      <div>x: { poi.x }, y: { poi.y }</div>
    </div>
  );
};

export default App;
```

> - **不要在循环，条件或嵌套函数中调用 Hook，** 确保总是在你的 React 函数的最顶层以及任何 return 之前调用他们。
>
> - **只在 React 函数中调用 Hook，**不要在普通的 JavaScript 函数中调用 Hook。你可以：
>   - 在 React 的函数组件中调用 Hook
>   - 在自定义 Hook 中调用其他 Hook
> - 如果遇到需要以对象的形式定义状态时，根据需求划分对象，因为修改状态使用的是替换

### 17.2.2 useEffect

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
import App from './12_hooks/04App_useEffect'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/04App_useEffect.jsx`

```jsx
// src/12_hooks/04App_useEffect.jsx

import React, { useState, useEffect } from 'react';

const App = (props) => {
  const [list, setList] = useState([])
  const [count, setCount] = useState(10)

  // useEffect(() => {}) // componentDidMount  componentDidUpdate
  // useEffect(() => {}, []) // componentDidMount  只执行一次
  // useEffect(() => {}, [ count ]) // 监听到count的值发生改变时，执行 - 监听器
  // useEffect(() => { return () => {} }, []) // 返回的函数相当于 componentWillUnmount
  useEffect(() => {
    fetch('http://121.89.205.189:3000/api/pro/list')
      .then(res => res.json())
      .then(res => {
        console.log(1, res.data)
        setList(res.data)
      })
    return () => {
      console.log('销毁1')
    }
  }, [])
  useEffect(() => {
    fetch('http://121.89.205.189:3000/api/pro/list')
      .then(res => res.json())
      .then(res => {
        console.log(2, res.data)
        setList(res.data)
      })
    return () => {
      console.log('销毁2')
    }
  }, [count])
  return (
    <div>
      <button onClick={ () => { setCount(count + 1) }}>加1</button>
      <button onClick={ () => { props.root.unmount() }}>销毁</button>
      <ul>
        {
          list && list.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
      </ul>
    </div>
  );
};

export default App;
```

> 1. useEffect() 是个副作用函数。
> 2. useEffect() 函数在每次组件重新渲染时，可再次被调用。
> 3. 在开发环境中，开启了 React.StrictMode 模式，组件开始时被渲染两次。https://zh-hans.legacy.reactjs.org/docs/strict-mode.html#%E7%A1%AE%E4%BF%9D%E5%8F%AF%E5%A4%8D%E7%94%A8%E7%9A%84-state-ensuring-reusable-state
> 4. useEffect() 通过返回函数来清理副作用。
> 5. useEffect() 通过传递第二个参数数组来提高渲染性能，或者说实现 watch 效果。

### 17.2.3 useRef

利用 `useRef` 就可以绕过 Capture Value 的特性。**可以认为** `ref` **在所有 Render 过程中保持着唯一引用，因此所有对** `ref` **的赋值或取值，拿到的都只有一个最终状态**，而不会在每个 Render 间存在隔离。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
import App from './12_hooks/05App_useRef'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/05App_useRef.jsx`

```jsx
// src/12_hooks/05App_useRef.jsx

import React, { Component, useRef, useEffect } from 'react';

const Fun = React.forwardRef((props, ref) => {
  return (
    <div>
      function 组件
      <input ref = { ref }/>
    </div>
  )
})

class Com extends Component {
  state = {
    com: 'class com'
  }
  render () {
    return (
      <div>
        class 组件
      </div>
    )
  }
}

const App = () => {
  const btnRef = useRef()
  const comRef = useRef()
  const funRef = useRef()

  useEffect(() => {
    // console.log(btnRef)
    btnRef.current.style.color = 'red'
    console.log(comRef.current)
    // console.log(funRef.current)
    funRef.current.value = '睡觉大王-刘创'

  }, [])

  return (
    <div>
      <button ref = { btnRef }>按钮</button>
      <Com ref = { comRef }/>
      <Fun ref = { funRef }/>
    </div>
  );
};

export default App;
```

### 17.2.4 useReducer

useReducer 践行了 Flux/Redux 思想。使用步骤：

1、创建初始值initialState

2、创建所有操作 reducer(state, action);

3、传给 userReducer，得到读和写API

4、调用写 ({type: '操作类型'})

总的来说，useReducer 是 useState 的复杂版。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
import App from './12_hooks/06App_useReducer'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/06App_useReducer.jsx`

```jsx
// src/12_hooks/06App_useReducer.jsx

import React, { useEffect, useReducer } from 'react';

// 1.定义组件的初始化状态
const intialState = {
  bannerList: [],
  proList: []
}
// 2.创建所有操作 reducer(state, action); 定义好修改状态的reducer - 纯函数
// state 初始化状态以及上一次的状态
// action 包含修改状态的标识以及传递的数据 { type: '', payload: '' }
const reducer = (state, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST': // 标识 触发 dispatch({ type: 'CHANGE_BANNER_LIST', payload: '' })
      return Object.assign({}, state, { bannerList: action.payload })
    case 'CHANGE_PRO_LIST': // 标识
      return { ...state, proList: action.payload }
    default:
      return state
  }
}

const App = () => {
  // 3.传给 userReducer，得到读和写API
  const [state, dispatch] = useReducer(reducer, intialState)

  useEffect(() => {
    fetch('http://121.89.205.189:3000/api/banner/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
    fetch('http://121.89.205.189:3000/api/pro/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_PRO_LIST',
          payload: res.data
        })
      })
  }, [])
  return (
    <div>
      <h1>banner</h1>
      {
        state.bannerList && state.bannerList.map(item => {
          return <img src={ item.img } alt="" key={item.bannerid} style={{ height: 60 }} />
        })
      }
      <h1>pro</h1>
      {
        state.proList && state.proList.map(item => {
          return (
            <div key={ item.proid }>
              <img src={ item.img1 } alt="" style={{ height: 60 }} />
              { item.proname }
            </div>
          )
        })
      }
    </div>
  );
};

export default App;
```

> 如果遇到多个组件需要共享状态时，单纯useReducer就显得无能为力

### 17.2.5 useContext

1、使用 C = createContext(initial) 创建上下文

2、使用 <C.Provider> 圈定作用域

3、在作用域内使用 useContext(C)来使用上下文

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
import App from './12_hooks/07App_useContext'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/01App_useContext.jsx`

```jsx
// src/12_hooks/01App_useContext.jsx
import React, { useContext } from 'react';
const MyContext = React.createContext()

const Child = () => {
  const val = useContext(MyContext)

  return (
    <div>child - { val }</div>
  )
}
const App = () => {
  return (
    <div>
      <MyContext.Provider value = '传家宝'>
        <Child />
      </MyContext.Provider>
    </div>
  );
};

export default App;
```

 **使用 useReducer 和 useContext 实现轻型Redux,可以让组件间共享状态**

步骤：

1、将数据集中在一个 store 对象

2、将所有操作集中在 reducer

3、创建一个 Context

4、创建对数据的读取 API

5、将第四步的内容放到第三步的 Context

6、用 Context.Provider 将 Context 提供给所有组件

7、各个组件用 useContext 获取读写API

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
import App from './12_hooks/08App_little_redux'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/08App_little_redux.jsx`

```jsx
// src/12_hooks/08App_little_redux.jsx
import React, { useContext, useEffect, useReducer } from 'react';
// 4.
const MyContext = React.createContext()

// 1.
const intialState = {
  bannerList: [],
  proList: []
}
// 2.
const reducer = (state, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return Object.assign({}, state, { bannerList: action.payload })
    case 'CHANGE_PRO_LIST': 
      return { ...state, proList: action.payload }
    default:
      return state
  }
}

function Home () {
  // 6.
  const {state, dispatch} = useContext(MyContext)

  useEffect(() => {
    fetch('http://121.89.205.189:3000/api/banner/list')
      .then(res => res.json())
      .then(res => {
        // 7.
        dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
    fetch('http://121.89.205.189:3000/api/pro/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_PRO_LIST',
          payload: res.data
        })
      })
  }, [])
  return (
    <div>
      首页
      
    </div>
  )
}

function List () {
  // 8.
  const {state} = useContext(MyContext)
  return (
    <div>
      列表
      {/* 9. */}
      {
        state.bannerList && state.bannerList.map(item => {
          return <img src={ item.img } alt="" key={item.bannerid} style={{ height: 60 }} />
        })
      }
      <h1>pro</h1>
      {
        state.proList && state.proList.map(item => {
          return (
            <div key={ item.proid }>
              <img src={ item.img1 } alt="" style={{ height: 60 }} />
              { item.proname }
            </div>
          )
        })
      }
    </div>
  )
}
const App = () => {
  // 3.
  const [state, dispatch] = useReducer(reducer, intialState)

  
  return (
    <div>
      {/* 5. */}
      <MyContext.Provider value = {{ state, dispatch }}>
        <Home />

        <hr />

        <List />
      </MyContext.Provider>
      
    </div>
  );
};

export default App;
```



> useContext + useReducer 可以实现轻型redux，但是不适合应用于多模块管理的大型项目

### 17.2.6 useMemo

把“创建”函数和依赖项数组作为参数传入 `useMemo`，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。

如果没有提供依赖项数组，`useMemo` 在每次渲染时都会计算新的值

**你可以把 `useMemo` 作为性能优化的手段，但不要把它当成语义上的保证。**

### 17.2.7 useCallback

把内联回调函数及依赖项数组作为参数传入 `useCallback`，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染（例如 `shouldComponentUpdate`）的子组件时，它将非常有用。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// 引入react组件时，后缀名可以不写
// import App from './12_hooks/01_App_hooks_useState'
// import App from './12_hooks/02_App_hooks_useState_obj'
// import App from './12_hooks/03_App_hooks_useState_state'
// import App from './12_hooks/04_App_hooks_useEffect'
// import App from './12_hooks/05_App_hooks_useRef'
// import App from './12_hooks/06_App_hooks_useReducer'
// import App from './12_hooks/07_App_hooks_useContext'
// import App from './12_hooks/08_App_hooks_redux'
import App from './12_hooks/09_App_hooks_useCallback_useMemo'

const root = ReactDOM.createRoot(document.querySelector('#root'))

root.render(<App />)

```

`src/12_hooks/09_App_hooks_useCallback_useMemo.jsx`

```jsx
// src/12_hooks/09App_useCallback_useMemo.jsx
import React, { memo, useCallback, useMemo, useState } from 'react';
// 如果子组件是函数式组件，且调用组件时没有传递任何依赖项
// 想要达到子组件不重新渲染，需要使用高阶组件 React.memo 包裹组件

const Child = memo(() => {
  console.log('child1')
  return (
    <div>child</div>
  )
})
const App = () => {
  const [arr] = useState([1, 2, 3, 4, 5, 6, 7, 8, 9])
  const [count, setCount] = useState(0)
  const [a, setA] = useState(10)

  const addA = () => {
    setA(a + 1)
  }
  // const handler = (event) => {} // 只要count的值发生改变，那么child组件就会被重新渲染 --- 每次都产生一个新的函数
  
  // const handler = useCallback(() => { 
  //   return (event) => {} 
  // }, []) // 记忆函数 （event） => {}
  const handler = useMemo((event) => {}, [])

  // 计算属性
  const len = useMemo(() => { 
    return arr.length 
  }, [arr])
  return (
    <div>
      { count }
      <button onClick={() => setCount(count + 1)}>加1</button>
      <button onClick={addA}>a加1</button> - { len }
      <Child a={a} handler = { handler }/>
    </div>
  );
};

export default App;
```

> useCallback 应用于 组件的事件，使用useCallback 包裹组件   -  记忆函数
>
> useMemo 可以是计算属性， 也应用于 组件的事件，使用useMemo  包裹组件  - 记忆组件

### 17.2.8 useImperativeHandle

`useImperativeHandle` 可以让你在使用 `ref` 时自定义暴露给父组件的实例值。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
import App from './12_hooks/11App_useImperativeHandle'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/11App_useImperativeHandle.jsx`

```jsx
// src/12_hooks/11App_useImperativeHandle.jsx
// 在父组件中获取子组件提供的数据何方法
import React, { forwardRef, useImperativeHandle, useRef } from 'react';

const MyInput = forwardRef((props, ref) => {
  const inputRef = useRef()

  // 关联了父组件传递过来的 ref 值
  // 暴露了父组件可以访问的子组件的数据何方法
  useImperativeHandle(ref, () => {
    return {
      a: '111', 
      b: 222,
      focus: () => {
        inputRef.current.focus()
      }
    }
  })

  return (
    <div>
      <input type="text" ref = { inputRef }/>
    </div>
  )
})

const App = () => {
  const myInputRef = useRef()
  const getData = () => {
    console.log(myInputRef.current.a)
    console.log(myInputRef.current.b)
    myInputRef.current.focus()
  }
  return (
    <div>
      <button onClick={ getData }>获取子组件的数据以及获取焦点</button>
      <MyInput ref = { myInputRef }/>
    </div>
  );
};

export default App;
```



上面这个例子中与直接转发 ref 不同，直接转发 ref 是将 `React.forwardRef` 中函数上的 ref 参数直接应用在了返回元素的 ref 属性上，其实父、子组件引用的是同一个 ref 的 current 对象，官方不建议使用这样的 ref 透传，而使用 `useImperativeHandle` 后，可以让父、子组件分别有自己的 ref，通过 `React.forwardRef` 将父组件的 ref 透传过来，通过 `useImperativeHandle` 方法来自定义开放给父组件的 current。

`useImperativeHandle `的第一个参数是定义 current 对象的 ref，第二个参数是一个函数，返回值是一个对象，即这个 ref 的 current 对象，这样可以像上面的案例一样，通过自定义父组件的 ref 来使用子组件 ref 的某些方法。

`useImperativeHandle` 和` React.forwardRef `必须是配合使用的，这也是为什么在开头要介绍 ref 的转发。

### 17.2.9 useLayoutEffect

useLayoutEffect 与 useEffect的区别：

- `useEffect` 是异步执行的，而`useLayoutEffect`是同步执行的。
- `useEffect` 的执行时机是浏览器完成渲染之后，而 `useLayoutEffect` 的执行时机是浏览器把内容真正渲染到界面之前

举个例子：

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
// import App from './12_hooks/11App_useImperativeHandle'
import App from './12_hooks/12App_useLayoutEffect'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12App_useLayoutEffect.jsx`

```jsx
// src/12_hooks/12App_useLayoutEffect.jsx

import React, { useEffect, useLayoutEffect, useRef } from 'react';

const App = () => {
  const inputRef = useRef()
  useEffect(() => { // DOM操作 数据请求
    console.log('useEffect') // 后打印
    inputRef.current.value = '1111'
  }, [])
  useLayoutEffect(() => { // DOM操作
    console.log('useLayoutEffect') // 先打印
    inputRef.current.value = '2222'
  })
  return (
    <div>
      <input type="text" ref={ inputRef } />
    </div>
  );
};

export default App;
```

> useLayoutEffect 做DOM操作
>
> useEffect 中 副作用执行

### 17.2.10 useDebugValue

`useDebugValue` 可用于在 React 开发者工具中显示自定义 hook 的标签。

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
// import App from './12_hooks/11App_useImperativeHandle'
// import App from './12_hooks/12App_useLayoutEffect'
import App from './12_hooks/13App_useDebugValue'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/13App_useDebugValue.jsx`

```jsx
// src/12_hooks/13App_useDebugValue.jsx
import React, { useDebugValue, useEffect, useState } from 'react';

// const App = () => {

//   const [count, setCount] = useState(0)
//   const add = () => setCount(count + 1)

//   useEffect(() => {
//     document.title = `点击了${count}次`
//   })
//   return (
//     <div>
//       <button onClick={ add }>加1</button> { count }
//     </div>
//   );
// };

const useCount = () => {
  useDebugValue('myCount')
  const [count, setCount] = useState(0)
  const add = () => setCount(count + 1)
  return {
    count, add
  }
}
const useTitle = (count) => {
  useDebugValue('myTitle')
  useEffect(() => {
    document.title = `点击了${count}次`
  })
}
const App = () => {
  const { count, add } = useCount()
  useTitle(count)
  return (
    <div>
      <button onClick={ add }>加1</button> { count }
    </div>
  );
} 

export default App;
```

接下来的hooks是属于react18版本新增的hooks

### 17.2.11 useId

`useId`是一个钩子，用于生成唯一的ID，在服务器和客户端之间是稳定的，同时避免hydration 不匹配。

>注意：
>
>`useId`不是用来生成列表中的键的。`Keys` 应该从你的数据中生成。
>
>不能用于列表渲染的key值

对于一个基本的例子，直接将id传递给需要它的元素。

对于同一组件中的多个ID，使用相同的ID附加一个后缀。

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
// import App from './12_hooks/11App_useImperativeHandle'
// import App from './12_hooks/12App_useLayoutEffect'
// import App from './12_hooks/13App_useDebugValue'
import App from './12_hooks/14App_useId'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/14App_useId.jsx`

```jsx
// src/12_hooks/14App_useId.jsx
import React, { useId, useState } from 'react';

const App = () => {
  const [list] = useState(['aa', 'bb', 'cc'])
  const useNameId = useId()
  const passwordId = useId()
  return (
    <div>
      {
        // list.map(item => { // ❌
        //   const id = useId() // 不可以在jsx代码中使用 hooks
        //   return <p key={ id }>{ item }</p>
        // })
        list.map(item => {
          return <p key={ item }>{ item }</p>
        })
      }

      <form >
        <div>
          {/* 网页中为 for属性 ==》 react  htmlFor */}
          {/* <label htmlFor="userName">用户名</label>
          <input type="text" id='userName' /> */}

          <label htmlFor={ useNameId }>用户名</label>
          <input type="text" id={ useNameId } />
        </div>
        <div>
          <label htmlFor={ passwordId }>密码</label>
          <input type="password" id={ passwordId } />
        </div>
      </form>
    </div>
  );
};

export default App;
```

> **注意：**
>
> `useId` 会生成一个包含 : token的字符串。这有助于确保令牌是唯一的，但在CSS选择器或API（如`querySelectorAll`）中不支持。
>
> `useId`支持一个`identifierPrefix`，以防止在多根应用程序中发生碰撞。要配置，请参阅 `hydrateRoot` 和 `ReactDOMServer `的选项。
>
> hooks需要在函数式组件以及自定义hook的顶级使用（返回jsx代码之前），不要在jsx代码中使用hooks

### 17.2.12 useDeferredValue  - 暂时了解

> 真实需求其实不需要实时渲染所有的数据

`useDeferredValue` 需要接收一个值, 返回这个值的副本, 副本的更新会在值更新渲染之后进行更新, 以此来避免一些不必要的重复渲染. 打个比方页面中有输入框, 输入框下的内容依赖于输入框的值, 但是输入框是一个高频操作, 如果输入10次, 可能用户只想看到最终的结果那么中途的实时渲染就显得不那么重要了, 页面元素少点还好, 一旦元素过多页面就会及其的卡顿, 渲染引擎堵得死死的, 用户就会骂娘了, 此时使用useDeferredValue是一个很好的选择

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
// import App from './12_hooks/11App_useImperativeHandle'
// import App from './12_hooks/12App_useLayoutEffect'
// import App from './12_hooks/13App_useDebugValue'
// import App from './12_hooks/14App_useId'
import App from './12_hooks/15App_useDeferredValue'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/15App_useDeferredValue.jsx` 数据量必须过大，否则看不到效果

```jsx
// src/12_hooks/15App_useDeferredValue.jsx
import React, { useDeferredValue, useEffect, useState, useMemo, memo } from 'react'
const List = memo(function List({count}) {
  const [data, setData] = useState([])

  useEffect(() => {
    const data = []
    data.length = 50000
    for (let i = 0; i < data.length; i++) {
      data.fill(i+1, i)
    }
    setData(data)

  }, [count])

  return (
    <div>
      {
        data.map((item) => {
          return (
            <p key={item}>{count}</p>
          )
        })
      }
    </div>
  )
})

export default function UseDeferredValueDemo() {
  const [inpVal, setInpVal] = useState('')
  const deferredValue = useDeferredValue(inpVal) // 备份数据
  const memoList = useMemo(() => <List count={deferredValue}></List>, [deferredValue])
  return (
    <>
      <h1>UseDeferredValue</h1>
      <input type="text" value={inpVal}  onChange={(e) => setInpVal(e.target.value)}/>
      {memoList}
    </>
  )
}
```

### 17.2.13 useTransition  - 暂时了解

`useTransition` 又叫过渡, 他的作用就是标记非紧急更新, 这些被标记非紧急更新会在紧急更新完之后进行更新, `useTransition` 使用场景在应对渲染量很大的页面，需要及时响应某些事件的情况。

举个例子，准备一个进度条, 通过滑动进度条来显示进度条的进度并且渲染相同进度数量的div, 如果我们不对渲染进行优化那无疑页面会很卡, 此时使用过渡配合useMemo来缓存页面结构, diffing算法就会对比出少量的变化进行局部修改。

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

// import App from './12_hooks/01App_useState'
// import App from './12_hooks/02App_useState'
// import App from './12_hooks/03App_useState'
// import App from './12_hooks/04App_useEffect'
// import App from './12_hooks/05App_useRef'
// import App from './12_hooks/06App_useReducer'
// import App from './12_hooks/07App_useContext'
// import App from './12_hooks/08App_little_redux'
// import App from './12_hooks/09App_useCallback_useMemo'
// import App from './12_hooks/10App_useCallback_useMemo'
// import App from './12_hooks/11App_useImperativeHandle'
// import App from './12_hooks/12App_useLayoutEffect'
// import App from './12_hooks/13App_useDebugValue'
// import App from './12_hooks/14App_useId'
// import App from './12_hooks/15App_useDeferredValue'
import App from './12_hooks/16App_useTransition'

import ErrorBoundary from './ErrorBoundary'

// import './style.stylus'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    {/* react严格模式的组件 */}
    {/* <React.StrictMode> */}
      <App root={ root }/>
    {/* </React.StrictMode> */}
  </ErrorBoundary>
)

```

`src/12_hooks/16App_useTransition`

```jsx
import React, { useTransition, useState, useMemo } from 'react'
 
export default function UseTransition() {
  const [isPending, startTransition] = useTransition()
 
  const [rangeValue, setRangeValue] = useState(1)
  const [renderData, setRenderData] = useState([1])
  const [isStartTransition, setIsStartTransition] = useState(false)
  const handleChange = (e) => {
    setRangeValue(e.target.value)
    const arr = []
    arr.length = e.target.value
    for (let i = 0; i <= arr.length; i++) {
      arr.fill(i, i + 1)
    }
    if (isStartTransition) {
      startTransition(() => {
        setRenderData(arr)
      })
    } else {
      setRenderData(arr)
    }
  }
  const jsx = useMemo(() => {
    return renderData.map((item, index) => {
      return (
        <div
          style={{
            width: 50,
            height: 50,
            backgroundColor: `#${Math.floor(Math.random() * 16777215).toString(
              16
            )}`,
            margin: 10,
            display: 'inline-block',
          }}
          key={'item'+index}
        >
          {item}
        </div>
      )
    })
  }, [renderData])
  return (
    <div>
      <div style={{ textAlign: 'center' }}>
        <label>
          <input
            type="checkbox"
            checked={isStartTransition}
            onChange={(e) => {
              setIsStartTransition(e.target.checked)
            }}
          />
          useTransition
        </label>
        <input
          type="range"
          value={rangeValue}
          min={0}
          max={10000}
          style={{ width: 120 }}
          onChange={handleChange}
        />
        <span>进度条 {rangeValue}</span>
        <hr />
      </div>
      {jsx}
    </div>
  )
}
```

```jsx
import React, { useTransition, useState, useMemo } from 'react'
 
export default function UseTransition() {
  const [isPending, startTransition] = useTransition()
 
  // 滑块默认值
  const [rangeValue, setRangeValue] = useState(1)

  const [renderData, setRenderData] = useState([1])

  const [isStartTransition, setIsStartTransition] = useState(false)
  // 滑块滑动事件
  const handleChange = (e) => {
    setRangeValue(e.target.value)
    const arr = []
    arr.length = e.target.value
    for (let i = 0; i <= arr.length; i++) {
      arr.fill(i, i + 1)
    }
    if (isStartTransition) {
      startTransition(() => {
        setRenderData(arr)
      })
    } else {
      setRenderData(arr)
    }
  }
  const jsx = useMemo(() => {
    return renderData.map((item, index) => {
      return (
        <div
          style={{
            width: 50,
            height: 50,
            backgroundColor: `#${Math.floor(Math.random() * 16777215).toString(
              16
            )}`,
            margin: 10,
            display: 'inline-block',
          }}
          key={'item'+index}
        >
          {item}
        </div>
      )
    })
  }, [renderData])
  return (
    <div>
      <div style={{ textAlign: 'center' }}>
        <label>
          <input
            type="checkbox"
            checked={isStartTransition}
            onChange={(e) => {
              setIsStartTransition(e.target.checked)
            }}
          />
          useTransition
        </label>
        <input
          type="range"
          value={rangeValue}
          min={0}
          max={10000}
          style={{ width: 120 }}
          onChange={handleChange}
        />
        <span>进度条 {rangeValue}</span>
        <hr />
      </div>
      {jsx}
    </div>
  )
}
```



### 17.2.14 useSyncExternalStore - 暂时了解

React18的beta版本将`useMutableSource`更新为了`useSyncExternalStore`，这个新的api将会对React的各种状态管理库产生非常大的影响，下面我来介绍`useSyncExternalStore`的用法和场景。

我们可以通过这个api自行设计一个redux + react-redux的数据方案：

**1、设计store**

首先我们要设计一个store，它必须有如下属性：

- currentState:当前状态
- subscribe:提供状态发生变化时的订阅能力
- getSnapshot: 获取当前状态

以及改变state的方法，这里参考redux，设计了dispatch、reducer

```jsx
const store = {
    currentState:{data:0},
    listeners:[],
    reducer(action){
        switch(action.type) {
            case 'ADD':
                return {data:store.currentState.data+1}
            default:
                return store.state
        }
    },
    subscribe(l){
        store.listeners.push(l)
    },
    getSnapshot() {
        return store.currentState
    },
    dispatch(action) {
        store.currentState = store.reducer(action)
        store.listeners.forEach(l=>l())
        return action;
    }
}
```

**2、应用 store 同步组件状态**

```jsx
import React, { useSyncExternalStore } from 'react'
import store from './store'

export default function UseSyncExternalStoreDemo() {
  const state = useSyncExternalStore(store.subscribe, () => store.getSnapshot().data);
    
    return (
      <div>
        <div>count: {state}</div>
        <div>
            <button onClick={()=>store.dispatch({type:'ADD'})}>add+</button>
        </div>
      </div>
    )
}
```



### 17.2.15 useInsertionEffect  - 了解

`useInsertionEffect` 与useEffect相同，在所有DOM变更之前同步触发。在使用 useLayoutEffect 读取布局之前，使用这个函数将样式注入到DOM中。因为这个钩子的作用域是有限的，所以这个钩子不能访问 refs，也不能调度更新。

```jsx
import React, { useInsertionEffect, useEffect, useLayoutEffect } from 'react'
 
export default function UseInsertionEffect() {

  useInsertionEffect(() => {
    console.log('useInsertionEffect') // 1
    // const style = document.createElement('style')
    // style.innerHTML = '.box { color: red }'
    // document.head.appendChild(style)
  })

  useEffect(() => {
    console.log('useEffect') // 3
  })

  useLayoutEffect(() => {
    console.log('useLayoutEffect') // 2
  })

  return (
    <div className="box">UseInsertionEffect</div>
  )
}
```

> useEffect - useLayoutEffect - useInsertionEffect

## 17.3 自定义hooks

以use开头的小驼峰式的函数

# 十八、Redux

## 18.1 理解Flux架构

在2013年，Facebook让`React`亮相的同时推出了Flux框架，`React`的初衷实际上是用来替代`jQuery`的，`Flux`实际上就可以用来替代`Backbone.js`，`Ember.js`等一系列`MVC`架构的前端JS框架。

其实`Flux`在`React`里的应用就类似于`Vue`中的`Vuex`的作用，但是在`Vue`中，`Vue`是完整的`mvvm`框架，而`Vuex`只是一个全局的插件。

`React`只是一个MVC中的V(视图层)，只管页面中的渲染，一旦有数据管理的时候，`React`本身的能力就不足以支撑复杂组件结构的项目，在传统的`MVC`中，就需要用到Model和Controller。Facebook对于当时世面上的`MVC`框架并不满意，于是就有了`Flux`, 但`Flux`并不是一个`MVC`框架，他是一种新的思想。

![image-20220812002755397](image\flux.png)

- View： 视图层
- ActionCreator（动作创造者）：视图层发出的消息（比如mouseClick）
- Dispatcher（派发器）：用来接收Actions、执行回调函数
- Store（数据层）：用来存放应用的状态，一旦发生变动，就提醒Views要更新页面

Flux的流程：

1. 组件获取到store中保存的数据挂载在自己的状态上
2. 用户产生了操作，调用actions的方法
3. actions接收到了用户的操作，进行一系列的逻辑代码、异步操作
4. 然后actions会创建出对应的action，action带有标识性的属性
5. actions调用dispatcher的dispatch方法将action传递给dispatcher
6. dispatcher接收到action并根据标识信息判断之后，调用store的更改数据的方法
7. store的方法被调用后，更改状态，并触发自己的某一个事件
8. store更改状态后事件被触发，该事件的处理程序会通知view去获取最新的数据

## 18.2 理解Redux单向数据流流程

React 只是 DOM 的一个抽象层，并不是 Web 应用的完整解决方案。有两个方面，它没涉及。

- 代码结构 
- 组件之间的通信

2013年 Facebook 提出了 Flux 架构的思想，引发了很多的实现。2015年，Redux 出现，将 Flux 与[函数式编程](<https://llh911001.gitbooks.io/mostly-adequate-guide-chinese/content/>)结合一起，很短时间内就成为了最热门的前端架构。

如果你不知道是否需要 Redux，那就是不需要它

只有遇到 React 实在解决不了的问题，你才需要 Redux

简单说，如果你的UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

**需要使用Redux的项目:**

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

**从组件层面考虑，什么样子的需要Redux：**

- 某个组件的状态，需要共享
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另一个组件的状态

**Redux的设计思想：**

1. Web 应用是一个状态机，视图与状态是一一对应的。
2. 所有的状态，保存在一个对象里面（唯一数据源）。

> 注意：flux、redux都不是必须和react搭配使用的，因为flux和redux是完整的架构，在学习react的时候，只是将react的组件作为redux中的视图层去使用了。

![image-20220812002936378](image\redux)



## 18.3 redux使用三大原则

- Single Source of Truth(唯一的数据源)
- State is read-only(状态是只读的) - 无法直接修改状态
- Changes are made with pure function(数据的改变必须通过纯函数完成)

```sh
$ cnpm i redux -S
```

`src/13_redux/store/index.js` 创建状态管理器

```js
// src/13_redux/store/index.js
//  1.引入 createStore 方法，新版本 redux 不建议这么使用 
import { createStore } from 'redux'

// 2..创建reducer 纯函数
// 包含需要共享的状态以及 action
// 状态是只读的，数据的改变必须通过纯函数完成
const reducer = (state = { // 初始化状态
  bannerList: [],
  proList: []
}, action) => {
  switch (action.type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: action.payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: action.payload }
    default:
      return state
  }
}

// 3.创建store
const store  = createStore(reducer)

console.log(store)

export default store
```

`src/index.js` 配置状态管理器 - 状态管理器修改订阅视图的更新

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import store from './13_redux/store/index'

import App from './13_redux/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

function fn () {
  root.render(
    <ErrorBoundary>
      <App root={ root }/>
    </ErrorBoundary>
  )  
}
fn()

// 监听store的变化 订阅store变化
store.subscribe(fn)
```

`src/13_redux/App.jsx`使用状态管理器

```jsx
// src/13_redux/App.jsx
import React from 'react';
import Child1 from './views/Child1';
import Child2 from './views/Child2';

const App = () => {
  return (
    <div>
      <Child1 />
      <hr />
      <Child2 />
    </div>
  );
};

export default App;
```

`src/13_redux/views/Child1.jsx`

```jsx
// src/13_redux/views/Child1.jsx
import React, { Component } from 'react';
import store from './../store/index'

class Child1 extends Component {
  componentDidMount () {
    fetch('http://121.89.205.189:3000/api/banner/list')
      .then(res => res.json())
      .then(res => {
        store.dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
  }
  render() {
    const state = store.getState()
    return (
      <div>
        {
          state.bannerList &&  state.bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          state.proList &&  state.proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
      </div>
    );
  }
}

export default Child1;
```

`src/13_redux/views/Child2.jsx`

```jsx
// src/13_redux/views/Child2.jsx
import React, { useEffect } from 'react';
import store from '../store'
const Child2 = () => {
  useEffect(() => {
    fetch('http://121.89.205.189:3000/api/pro/list')
    .then(res => res.json())
    .then(res => {
      store.dispatch({
        type: 'CHANGE_PRO_LIST',
        payload: res.data
      })
    })
  }, [])
  return (
    <div>
      {
          store.getState().bannerList &&  store.getState().bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          store.getState().proList &&  store.getState().proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};

export default Child2;
```

以上代码是最最最最最最最基本的redux的使用，现在企业里基本上不使用这种原始的方法

## 18.4 redux + react-redux配合使用

> redux是属于js的状态管理模式，不独属于react，在react中需要结合 react-redux 模块使用

react-redux提供两个核心的api：

- Provider: 提供store `<Provider store = { store }><App /></store>`

- connect: 用于连接容器组件和展示组件

  - Provider

    根据单一store原则 ，一般只会出现在整个应用程序的最顶层。

  - connect

    语法格式为

    `connect(mapStateToProps?, mapDispatchToProps?, mergeProps?, options?)(component)`

    一般来说只会用到前面两个，它的作用是：

    - 把`store.getState()`的状态转化为展示组件的`props`
    - 把`actionCreators`转化为展示组件`props`上的方法

只要上层中有`Provider`组件并且提供了`store`, 那么，子孙级别的任何组件，要想使用`store`里的状态，都可以通过`connect`方法进行连接。如果只是想连接`actionCreators`，可以第一个参数传递为`null`

```sh
$ cnpm i react-redux -S
```

### 18.4.1 创建状态管理器

`src/14_redux_react-redux/store/index.js`

```js
// src/14_redux_react-redux/store/index.js
import { createStore } from 'redux'

const reducer = (state = {
  bannerList: [],
  proList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: payload }
    default:
      return state
  }
}

const store = createStore(reducer)

export default store
```

> 1.引入 createStore 方法（新版本中建议使用rtk）
>
> 2.创建reducer（包含初始化状态，包含了修改状态的标识，返回了新的状态（对象合并））
>
> 3.创建store
>
> 4.暴露store

### 18.4.2  使用react-redux

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './14_redux_react-redux/store'
import App from './14_redux_react-redux/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

> 使用了  react-redux 提供的组件组件 Provider 配合 store 完成传递数据

`src/14_redux_react-redux/App.jsx`

```jsx
// src/14_redux_react-redux/App.jsx
import React from 'react';
import Home from './views/Home';
import Kind from './views/Kind';

const App = () => {
  return (
    <div>
      <Home />
      <hr />
      <Kind />
    </div>
  );
};

export default App;
```

`src/14_redux_react-redux/views/Home.jsx` 

```jsx
// src/14_redux_react-redux/views/Home.jsx
import React, { Component } from 'react';
import { connect } from 'react-redux'
class Home extends Component {

  componentDidMount () {
    this.props.getBannerList()
  }

  render() {

    // 从容器组件 获取 状态管理器中的状态
    const { bannerList, proList } = this.props
    return (
      <div>
        {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
      </div>
    );
  }
}

const mapStateToProps = (state) => { // state 即为函数调用模式下的 store.getState(),为所有的状态管理器中管理的状态
  return { // 后续在展示组件中 就可以通过 props.bannerList 以及 props.proList 调用数据了
    bannerList: state.bannerList,
    proList: state.proList
  }
}

const mapDispatchToProps = (dispatch) => { // dispatch可以用来触发修改reducer 函数中的状态 等同于 store.dispatch()
  return {
    getBannerList () {
      fetch('http://121.89.205.189:3000/api/banner/list')
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_BANNER_LIST',
            payload: res.data
          })
        })
    }
  }
}
// export default connect()(Home);
export default connect(mapStateToProps, mapDispatchToProps)(Home);
// connect(mapStateToProps, mapDispatchToProps)(Home) 暴露出去的叫容器组件
// Home 展示组件

```

`src/14_redux_react-redux/views/Kind.jsx` 业务逻辑写到容器组件中

```jsx
// src/14_redux_react-redux/views/Kind.jsx
import React from 'react';
import { useEffect } from 'react';
import { connect } from 'react-redux'
const Kind = (props) => {
  const { bannerList, proList } = props
  useEffect(() => {
    props.getProList()
  }, [props])
  return (
    <div>
      {
        bannerList &&  bannerList.map(item => {
          return (
            <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
          )
        })
      }
      {
        proList &&  proList.map(item => {
          return (
            <li key = { item.proid }> { item.proname } </li>
          )
        })
      }
    </div>
  );
};

export default connect((state) => {
  return {
    bannerList: state.bannerList,
    proList: state.proList
  }
}, (dispatch) => {
  return {
    getProList () {
      fetch('http://121.89.205.189:3000/api/pro/list')
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_PRO_LIST',
            payload: res.data
          })
        })
    }
  }
})(Kind);
```



> vuex中 可以把异步操作提取到vuex中的actions中，react中异步操作方式会有很多，最常见的就是 redux-thunk

## 18.5 redux+react-redux分模块使用

> 如果项目很大，或者项目划分比较明确，需要将状态管理器也分模块去处理
>
> 假设现在有一个home模块，管理 bannerList 以及 proList
>
> 还有一个模块 kind 模块，管理 kindList 

`src/15_redux_react-redux_combine/store/modules/home.js`

```js
// src/15_redux_react-redux_combine/store/modules/home.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: payload }
    default:
      return state
  }
}

export default reducer
```

`src/15_redux_react-redux_combine/store/modules/kind.js`

```js
// src/15_redux_react-redux_combine/store/modules/kind.js
const reducer = (state = {
  kindList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_KIND_LIST':
      return { ...state, kindList: payload }
    default:
      return state
  }
}

export default reducer
```

> 整合多个reducer 为一个，因为 有一个原则 ： 单一数据源 的原则

`src/15_redux_react-redux_combine/store/index.js`

```js
// src/15_redux_react-redux_combine/store/index.js
import { createStore, combineReducers } from "redux";

import homeReducer from './modules/home'
import kindReducer from './modules/kind'

const reducer = combineReducers({
  home: homeReducer, // home 模块 使用了 homeReducer
  kind: kindReducer // kind 模块 使用了 kindReducer
})

const store = createStore(reducer)

export default store
```

> 入口文件处引入状态管理器

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './15_redux_react-redux_combine/store'
import App from './15_redux_react-redux_combine/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

> 组件中使用状态管理器

`src/15_redux_react-redux_combine/App.jsx`

```jsx
// src/15_redux_react-redux_combine/App.jsx

import React from 'react';
import Home from './views/Home';
import Kind from './views/Kind';

const App = () => {
  return (
    <div>
      <Home />
      <Kind />
    </div>
  );
};

export default App;
```

`src/15_redux_react-redux_combine/views/Home.jsx` 业务逻辑在展示型组件中

```jsx
// src/15_redux_react-redux_combine/views/Home.jsx
import React, { useEffect } from 'react';
import { connect } from 'react-redux'
const Home = (props) => {
  const { bannerList, proList } = props
  useEffect(() => {
    props.getBannerList()
  }, [props])
  return (
    <div>
      2222
      {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};
// state => { return {}}  <===> state => ({})
// dispatch => { return {}}  <===> dispatch => ({})
export default connect(state => ({ // state = { home: { bannerList: [], proList: []}, kind: { kindList: []}}
  bannerList: state.home.bannerList,
  proList: state.home.proList
}), dispatch => ({
  getBannerList () {
    fetch('http://121.89.205.189:3000/api/banner/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
    fetch('http://121.89.205.189:3000/api/pro/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_PRO_LIST',
          payload: res.data
        })
      })
  }
}))(Home);
```

`src/15_redux_react-redux_combine/views/Kind.jsx ` 业务逻辑在容器组件中

```jsx
// src/15_redux_react-redux_combine/views/Kind.jsx
import React, { useEffect } from 'react';
import { connect } from 'react-redux' 
const Kind = (props) => {
  const { kindList } = props
  useEffect(() => {
    props.getKindList()
  }, [props])
  return (
    <div>
      333
      {
          kindList &&  kindList.map(item => {
            return (
              <p key = { item }>{ item }</p>
            )
          })
        }
    </div>
  );
};
// (state) => { return { bannerList: state.bannerList, proList: state.proList }}
// (state) => ({ bannerList: state.bannerList, proList: state.proList })
// ({ bannerList, proList }) => ({ bannerList: bannerList, proList: proList })
// ({ bannerList, proList }) => ({ bannerList, proList })

// state => ({ kindList: state.kind.kindList })
// ({ kind }) => ({ kindList: kind.kindList })
// ({ kind: { kindList } }) => ({ kindList: kindList })
export default connect(({ kind: { kindList }}) => ({ kindList }), dispatch => ({
  getKindList () {
    fetch('http://121.89.205.189:3000/api/pro/categorylist')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_KIND_LIST',
          payload: res.data
        })
      })
  }
}))(Kind);
```

> 细心的你发现，虽然展示组件的代码变少了，但是容器组件中还有 异步相关操作，能否把这些异步操作提取出去

## 18.6 redux + react-redux + 分模块+ 异步操作

> 配合redux 常用的异步模块 为 `redux-thunk`,  `redux-saga`

### 18.6.1 redux-thunk

```sh
$ cnpm i redux-thunk -S
```

`src/16_redux_react-redux_redux-thunk_combine/store/modules/home.js`

```js
// src/16_redux_react-redux_redux-thunk_combine/store/modules/home.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: payload }
    default:
      return state
  }
}

export default reducer
```

`src/16_redux_react-redux_redux-thunk_combine/store/modules/kind.js`

```js
// src/16_redux_react-redux_redux-thunk_combine/store/modules/kind.js
const reducer = (state = {
  kindList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_KIND_LIST':
      return { ...state, kindList: payload }
    default:
      return state
  }
}

export default reducer
```

> 整合多个reducer 为一个，因为 有一个原则 ： 单一数据源 的原则
>
> 并且后续需要使用 异步操作，将异步操作模块 使用进来

`src/16_redux_react-redux_redux-thunk_combine/store/index.js`

```jsx
// src/16_redux_react-redux_redux-thunk_combine/store/index.js
import { createStore, combineReducers, applyMiddleware } from 'redux'

import thunk from 'redux-thunk' // 异步

import home from './modules/home'
import kind from './modules/kind'

const reducer = combineReducers({ home, kind })

const store = createStore(reducer, applyMiddleware(thunk))

export default store
```

`src/index.js`

```jsx
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './16_redux_react-redux_redux-thunk_combine/store'
import App from './16_redux_react-redux_redux-thunk_combine/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

`src/16_redux_react-redux_redux-thunk_combine/App.jsx`

```jsx
// src/16_redux_react-redux_redux-thunk_combine/App.jsx
import React from 'react';
import Home from './views/Home';
import Kind from './views/Kind';

const App = () => {
  return (
    <div>
      <Home />
      <Kind />
    </div>
  );
};

export default App;
```

> 上一个案例 异步操作在组件，现在需要将其放到 异步操作模块中去 actionCreator

`src/16_redux_react-redux_redux-thunk_combine/store/actions/home.js`

```js
// src/16_redux_react-redux_redux-thunk_combine/store/actions/home.js

const action = {
  getBannerListData (dispatch) {// 函数的默认参数
    fetch('http://121.89.205.189:3000/api/banner/list')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_BANNER_LIST',
          payload: res.data
        })
      })
  },
  // getProListData (dispatch) {
  //   fetch('http://121.89.205.189:3000/api/pro/list')
  //     .then(res => res.json())
  //     .then(res => {
  //       dispatch({
  //         type: 'CHANGE_PRO_LIST',
  //         payload: res.data
  //       })
  //     })
  // }
  // { count: 1 , limitNum: 10}
  getProListData (params) {// 需要传递参数时，返回dispatch的默认参数的函数
    return (dispatch) => {
      fetch('http://121.89.205.189:3000/api/pro/list?limitNum=' + params.limitNum)
        .then(res => res.json())
        .then(res => {
          dispatch({
            type: 'CHANGE_PRO_LIST',
            payload: res.data
          })
        })
    }
  }
}
export default action
```

`src/16_redux_react-redux_redux-thunk_combine/store/actions/kind.js`

```jsx
// src/16_redux_react-redux_redux-thunk_combine/store/actions/kind.js

const action = {
  getKindListData (dispatch) {
    fetch('http://121.89.205.189:3000/api/pro/categorylist')
      .then(res => res.json())
      .then(res => {
        dispatch({
          type: 'CHANGE_KIND_LIST',
          payload: res.data
        })
      })
  }
}

export default action
```

> 记住接口有无参数影响 action 的写法，还影响其调用

`src/16_redux_react-redux_redux-thunk_combine/views/Home.jsx`

```jsx
// src/16_redux_react-redux_redux-thunk_combine/views/Home.jsx
import React, { useEffect} from 'react';
import { connect } from 'react-redux'
import action from '../store/actions/home'
const Home = (props) => {
  const { bannerList, proList, getBannerList, getProList } = props
  useEffect(() => {
    getBannerList()
    getProList()
  }, [getBannerList, getProList])
  return (
    <div>
      {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};

export default connect(({ home: { bannerList, proList }})=> ({ bannerList, proList }), dispatch => {
  return {
    getBannerList () {
      // 触发异步操作
      dispatch(action.getBannerListData)
    },
    getProList () {
      dispatch(action.getProListData({ limitNum: 5 }))
    }
  }
})(Home);
```

`src/16_redux_react-redux_redux-thunk_combine/views/Kind.jsx` 异步在store

```jsx
// src/16_redux_react-redux_redux-thunk_combine/views/Kind.jsx
import React, { useEffect } from 'react';
import { connect } from 'react-redux'
import action from '../store/actions/kind'
const Kind = (props) => {
  const { kindList, getKindList } = props
  useEffect(() => {
    getKindList()
  }, [getKindList])
  return (
    <div>
      {
          kindList &&  kindList.map(item => {
            return (
              <p key = { item }>{ item }</p>
            )
          })
        }
    </div>
  );
};

export default connect(({ kind: { kindList }}) => ({ kindList }), dispatch => ({
  getKindList () {
   dispatch(action.getKindListData)
  }
}))(Kind);
```

> 虽然引入了redux-thunk，但是仍然可以把 异步放在组件中，具体根据项目的需求而定

> 创建状态模块
>
> 整合模块
>
> 创建异步的 actionCreator
>
> 组件触发 actionCreator
>
> 传递数据以及修改界面

### 18.6.2 redux-saga - 最好掌握

> 先要了解解决异步操作方案：回调函数、promise、async await、generator yield

https://es6.ruanyifeng.com/#docs/generator-async

```sh
$ cnpm i redux-saga -S
```

`src/17_redux_react-redux_redux-saga_combine/store/modules/home.js`

```jsx
// src/17_redux_react-redux_redux-saga_combine/store/modules/home.js
const reducer = (state = {
  bannerList: [],
  proList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_BANNER_LIST':
      return { ...state, bannerList: payload }
    case 'CHANGE_PRO_LIST':
      return { ...state, proList: payload }
    default:
      return state
  }
}

export default reducer
```

`src/17_redux_react-redux_redux-saga_combine/store/modules/kind.js`

```js
// src/17_redux_react-redux_redux-saga_combine/store/modules/kind.js
const reducer = (state = {
  kindList: []
}, { type, payload }) => {
  switch (type) {
    case 'CHANGE_KIND_LIST':
      return { ...state, kindList: payload }
    default:
      return state
  }
}

export default reducer
```

`src/17_redux_react-redux_redux-saga_combine/store/mySaga.js`

```js
// src/17_redux_react-redux_redux-saga_combine/store/mySaga.js

// call 用来触发异步请求
// put 理解为 原来 store.dispatch
// takeLatest 用来分发告诉程序执行哪一个异步行为
import { call, put, takeLatest } from 'redux-saga/effects'

function getBannerListData () {
  return fetch('http://121.89.205.189:3000/api/banner/list').then(res => res.json())
}
function getProListData (params) {
  console.log(params)
  return fetch('http://121.89.205.189:3000/api/pro/list').then(res => res.json())
}
function getKindListData () {
  return fetch('http://121.89.205.189:3000/api/pro/categorylist').then(res => res.json())
}

function * getBannerListAction () { // * 代表generator函数，函数内部使用yield继续执行
  const res = yield call(getBannerListData)
  yield put({
    type: 'CHANGE_BANNER_LIST',
    payload: res.data
  })
}
function * getProListAction (action) { // * 代表generator函数，函数内部使用yield继续执行, action理解为参数
  const res = yield call(getProListData, action.payload) // action.payload 即为参数
  yield put({
    type: 'CHANGE_PRO_LIST',
    payload: res.data
  })
}
function * getKindListAction () { // * 代表generator函数，函数内部使用yield继续执行
  const res = yield call(getKindListData)
  yield put({
    type: 'CHANGE_KIND_LIST',
    payload: res.data
  })
}

// 分发异步操作
function * mySaga () {
  // takeLatest 第一个参数调用标识，第二个参数是一个函数
  // 有人触发 REQUEST_BANNER_LIST 标识，那么调用 getBannerListAction 函数，不要()
  yield takeLatest('REQUEST_BANNER_LIST', getBannerListAction)
  yield takeLatest('REQUEST_PRO_LIST', getProListAction)
  yield takeLatest('REQUEST_KIND_LIST', getKindListAction)
}

export default mySaga
```

`src/17_redux_react-redux_redux-saga_combine/store/index.js`

```jsx
// src/17_redux_react-redux_redux-saga_combine/store/index.js
import { createStore, combineReducers, applyMiddleware } from 'redux'

import createSagaMiddleware from 'redux-saga'

import mySaga from './mySaga'

import home from './modules/home'
import kind from './modules/kind'

const reducer = combineReducers({ home, kind })

const middleware = createSagaMiddleware()

const store = createStore(reducer, applyMiddleware(middleware))

middleware.run(mySaga)

export default store
```

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './17_redux_react-redux_redux-saga_combine/store'
import App from './17_redux_react-redux_redux-saga_combine/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

```jsx
// src/17_redux_react-redux_redux-saga_combine/App.jsx
import React from 'react';
import Home from './views/Home';
import Kind from './views/Kind';

const App = () => {
  return (
    <div>
      <Home />
      <Kind />
    </div>
  );
};

export default App;
```



`src/17_redux_react-redux_redux-saga_combine/views/Home.jsx`

```jsx
// src/17_redux_react-redux_redux-saga_combine/views/Home.jsx
import React, { useEffect } from 'react';
import { connect } from 'react-redux'
const Home = (props) => {
  console.log(props)
  const {bannerList, proList, dispatch} = props
  useEffect(() => {
    dispatch({
      type: 'REQUEST_BANNER_LIST'
    })
    dispatch({
      type: 'REQUEST_PRO_LIST',
      payload: { limitNum: 2 }
    })
  }, [dispatch])
  return (
    <div>
       {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};

export default connect(state => {
  return {
    bannerList: state.home.bannerList,
    proList: state.home.proList
  }
})(Home);
```

`src/17_redux_react-redux_redux-saga_combine/views/Kind.jsx`

```jsx
// src/17_redux_react-redux_redux-saga_combine/views/Kind.jsx
import React, { useEffect } from 'react';
import {connect} from 'react-redux'
const Kind = (props) => {
  const { kindList, dispatch} = props
  useEffect(() => {
    dispatch({ type: 'REQUEST_KIND_LIST'})
  }, [dispatch])
  return (
    <div>
        {
          kindList &&  kindList.map(item => {
            return (
              <p key = { item }>{ item }</p>
            )
          })
        }
    </div>
  );
};

export default connect(({ kind: { kindList }}) => ({kindList}))(Kind);
```

## 18.7 使用immutable不可变数据数据结构

immutable不可变的数据解构，意思就是一旦数据被创建，需要使用特殊的方法进行修改（修改过后的数据也是符合immutable数据解构的，他是一个全新的对象）

学习immutable： https://www.npmjs.com/package/immutable

```sh
$ cnpm i immutable redux-immutable -S
```

重点就是修改 reducer 的数据机构以及组件中获取状态的方式（整合reducer时使用 redux-immutable 提供的 combineReducers）

拷贝17文件，修改 store/modules/home.js    store/modules/kind.js    store/index.js  

views/Home.jsx  views/Kind.jsx

# 十九、redux toolkit

rtk

https://cn.redux.js.org/

## 19.1 什么是Redux Toolkit

https://cn.redux.js.org/redux-toolkit/overview/

**[Redux Toolkit](https://redux-toolkit.js.org/)** 是我们官方的，有观点的，开箱即用的高效 Redux 开发工具集。它旨在成为标准的 Redux 逻辑开发方式，我们强烈建议您使用它。

它包括几个实用程序功能，这些功能可以简化最常见场景下的 Redux 开发，包括配置 store、定义 reducer，不可变的更新逻辑、甚至可以立即创建整个状态的 “切片 slice”，而无需手动编写任何 action creator 或者 action type。它还包括使用最广泛的 Redux 插件，例如 Redux Thunk 用于异步逻辑，而 Reselect 用于编写选择器 selector 函数，因此你可以立即使用它们。

## 19.2 如何使用redux-toolkit

https://cn.redux.js.org/tutorials/quick-start

```sh
$ cnpm i @reduxjs/toolkit -S
```

### 19.2.1 创建状态管理器

`src/18_rtk/store/index.js`

```js
// src/18_rtk/store/index.js
// 从 Redux Toolkit 引入 configureStore API。
import { configureStore } from '@reduxjs/toolkit'

// 我们从创建一个空的 Redux store 开始，并且导出它:
export default configureStore({
  reducer: {}
})
```

### 19.2.2 入口文件配置状态管理器

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './18_rtk/store'
import App from './18_rtk/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

### 19.2.3 创建分模块切片

创建 slice 需要一个字符串名称来标识切片、一个初始 state 以及一个或多个定义了该如何更新 state 的 reducer 函数。slice 创建后 ，我们可以导出 slice 中生成的 Redux action creators 和 reducer 函数。

`src/18_rtk/store/modules/home.js`

```js
// src/18_rtk/store/modules/home.js
// 从 Redux Toolkit 引入 createSlice API
import { createSlice } from '@reduxjs/toolkit'

const homeSlice = createSlice({
  name: 'home', // 一个字符串名称来标识切片
  initialState: { // 初始 state 
    bannerList: [],
    proList: []
  },
  // 一个或多个定义了该如何更新 state 的 reducer 函数
  reducers: {
    changeBannerList (state, action) {
      state.bannerList = action.payload
    },
    changeProList (state, action) {
      state.proList = action.payload
    }
  }
})

console.log(homeSlice)
// actions {changeBannerList: fn, changeProList: fn}
// reducer: fn

// slice 创建后 ，我们可以导出 slice 中生成的 Redux action creators
// 后续组件中调用此函数修改状态
export const { changeBannerList, changeProList } = homeSlice.actions
// 导出 reducer 函数
export default homeSlice.reducer
```

`src/18_rtk/store/modules/kind.js`

```js
// src/18_rtk/store/modules/kind.js

import { createSlice } from '@reduxjs/toolkit'

const kindSlice = createSlice({
  name: 'kind',
  initialState: {
    kindList: []
  },
  reducers: {
    changeKindList (state, action) {
      state.kindList = action.payload
    }
  }
})

export const { changeKindList } = kindSlice.actions

export default kindSlice.reducer
```

### 19.2.4 **整合切片**

`src/18_rtk/store/index.js`

```js
// src/18_rtk/store/index.js
// 从 Redux Toolkit 引入 configureStore API。
import { configureStore } from '@reduxjs/toolkit'

import homeReducer from './modules/home'
import kindReducer from './modules/kind'

// 我们从创建一个空的 Redux store 开始，并且导出它:
export default configureStore({
  reducer: {
    home: homeReducer,
    kind: kindReducer
  }
})
```



### 19.2.5 组件中使用状态管理器

`src/18_rtk/views/Home.jsx`

```jsx
// src/18_rtk/views/Home.jsx
import React, { useEffect } from 'react';
// 使用 React-Redux 钩子让 React 组件与 Redux store 交互
import { useDispatch, useSelector } from 'react-redux'
// 引入状态管理器修改状态的方法
import { changeBannerList, changeProList } from '../store/modules/home'

function getBannerListData () {
  return fetch('http://121.89.205.189:3000/api/banner/list').then(res => res.json())
}
function getProListData (params) {
  console.log(params)
  return fetch('http://121.89.205.189:3000/api/pro/list').then(res => res.json())
}

const Home = () => {
  // 使用 useSelector 从 store 中读取数据
  const bannerList = useSelector(state => state.home.bannerList)
  const proList = useSelector(state => state.home.proList)
  // 使用 useDispatch dispatch actions
  const dispatch = useDispatch()

  useEffect(() => {
    getBannerListData().then(res => {
      dispatch(changeBannerList(res.data))
    })
    getProListData().then(res => {
      dispatch(changeProList(res.data))
    })
  }, [])
  return (
    <div>
      {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};

export default Home;
```

`src/18_rtk/views/Kind.jsx`

```jsx
// src/18_rtk/views/Kind.jsx
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux'
import { changeKindList } from './../store/modules/kind'

function getKindListData () {
  return fetch('http://121.89.205.189:3000/api/pro/categorylist').then(res => res.json())
}

const Kind = () => {
  const kindList = useSelector(state => state.kind.kindList)
  const dispatch = useDispatch()

  useEffect(() => {
    getKindListData().then(res => {
      dispatch(changeKindList(res.data))
    })
  }, [])
  return (
    <div>
       {
          kindList &&  kindList.map(item => {
            return (
              <p key = { item }>{ item }</p>
            )
          })
        }
    </div>
  );
};

export default Kind;
```



> - 使用`configureStore`创建 Redux store
>   - `configureStore` 接受 `reducer` 函数作为命名参数
>   - `configureStore` 使用的好用的默认设置自动设置 store
>
> - 为 React 应用程序组件提供 Redux store
>
>   - 使用 React-Redux `<Provider>` 组件包裹你的 `<App />`
>   - 传递 Redux store 如 `<Provider store={store}>`
>
> - 使用 `createSlice` 创建 Redux "slice" reducer
>
>   - 使用字符串名称、初始状态和命名的 reducers 函数调用“createSlice”
>
>   - Reducer 函数可以使用 Immer 来“改变”状态
>   - 导出生成的 slice reducer 和 action creators
>
> - 在 React 组件中使用 React-Redux `useSelector/useDispatch` 钩子
>
>   - 使用 `useSelector` 钩子从 store 中读取数据
>
>   - 使用 `useDispatch` 钩子获取 `dispatch` 函数，并根据需要 dispatch actions



> 此时发现，组件中的异步操作在 组件内部调用，接下来需要研究如何将异步操作从组件提取出来

## 19.3 提取异步操作

### 19.3.1 自己定义请求函数

`src/19_rtk_async/api/home.js`

```js
// src/19_rtk_async/api/home.js
export function getBannerListData () {
  return fetch('http://121.89.205.189:3000/api/banner/list').then(res => res.json())
}

export function getProListData (params) {
  console.log(params)
  return fetch('http://121.89.205.189:3000/api/pro/list').then(res => res.json())
}
```

`src/19_rtk_async/api/kind.js`

```js
// src/19_rtk_async/api/kind.js
export function getKindListData () {
  return fetch('http://121.89.205.189:3000/api/pro/categorylist').then(res => res.json())
}
```

`src/19_rtk_async/store/modules/home.js`

```js
// src/19_rtk_async/store/modules/home.js
import { createSlice } from '@reduxjs/toolkit'
import { getBannerListData, getProListData } from './../../api/home'

const homeSlice = createSlice({
  name: 'home',
  initialState: {
    bannerList: [],
    proList: []
  },
  reducers: {
    changeBannerList (state, action) {
      state.bannerList = action.payload
    },
    changeProList (state, action) {
      state.proList = action.payload 
    }
  }
})

export const {changeBannerList, changeProList} = homeSlice.actions

export default homeSlice.reducer

export function getBannerListAction (dispatch) {
  getBannerListData().then(res => {
    dispatch(changeBannerList(res.data)) // 修改数据
  })
}

export function getProListAction () {
  return (dispatch) => {
    getProListData().then(res => {
      dispatch(changeProList(res.data))
    })
  }
}
```

`src/19_rtk_async/store/index.js`

```js
// src/19_rtk_async/store/index.js
import { configureStore } from '@reduxjs/toolkit'
import home from './modules/home'
import kind from './modules/kind'
export default configureStore({
  reducer: {
    home,
    kind
  }
})
```

`src/index.js`

```js
// src/index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import { Provider } from 'react-redux'

import store from './19_rtk_async/store'
import App from './19_rtk_async/App'

import ErrorBoundary from './ErrorBoundary'

const root = ReactDOM.createRoot(document.getElementById('root'))

root.render(
  <ErrorBoundary>
    <Provider store = { store }>
      <App root={ root }/>
    </Provider>
  </ErrorBoundary>
)  

```

`src/19_rtk_async/App.jsx`

```jsx
// src/19_rtk_async/App.jsx
import React from 'react'
import Home from './views/Home'
import Kind from './views/Kind'

function App() {
  return (
    <div>
      App
      <Home ></Home>
      <hr/>
      <Kind></Kind>
    </div>
  )
}

export default App

```

`src/19_rtk_async/views/Home.jsx`

```jsx
// src/19_rtk_async/Home.jsx
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux'
import { getBannerListAction, getProListAction } from '../store/modules/home'
const Home = () => {
  const bannerList = useSelector(state => state.home.bannerList)
  const proList = useSelector(state => state.home.proList)

  const dispatch = useDispatch()

  useEffect(() => {
    dispatch(getBannerListAction)
    dispatch(getProListAction())
  }, [])
  return (
    <div>
      {
          bannerList &&  bannerList.map(item => {
            return (
              <img key = { item.bannerid } src={item.img} alt="" style={{ height: 40 }}/>
            )
          })
        }
        {
          proList &&  proList.map(item => {
            return (
              <li key = { item.proid }> { item.proname } </li>
            )
          })
        }
    </div>
  );
};

export default Home;
```



### 19.3.2 使用 `createAsyncThunk` 请求数据

Redux Toolkit 的 `createAsyncThunk` API 生成 thunk，为您自动 dispatch 那些 "start/success/failure" action。

`src/19_rtk_async/store/modules/kind.js`

```js
// src/19_rtk_async/store/modules/kind.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'
import { getKindListData } from '../../api/kind'
const kindSlice = createSlice({
  name: 'kind',
  initialState: {
    kindList: []
  },
  reducers: {
    changeKindList (state, action) {
      state.kindList = action.payload
    }
  },
  extraReducers(builder) {
    builder
      // .addCase(getKindListAction.pending, (state, action) => {
      //   state.status = 'loading'
      // })
      .addCase(getKindListAction.fulfilled, (state, action) => {
        // state.status = 'succeeded'
        // Add any fetched posts to the array
        state.kindList = action.payload
      })
      // .addCase(getKindListAction.rejected, (state, action) => {
      //   state.status = 'failed'
      //   state.error = action.error.message
      // })
  }
})

export const { changeKindList } = kindSlice.actions

export default kindSlice.reducer

export const getKindListAction = createAsyncThunk('kind/getKindList', async () => {
  const res = await getKindListData()
  return res.data
})
```

```js
// src/19_rtk_async/store/index.js
import { configureStore } from '@reduxjs/toolkit'
import home from './modules/home'
import kind from './modules/kind'
export default configureStore({
  reducer: {
    home,
    kind
  }
})
```



`src/19_rtk_async/views/Kind.jsx`

```jsx
// src/19_rtk_async/Kind.jsx
import React, { useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux'
import { getKindListAction } from '../store/modules/kind'
const Kind = () => {
  const kindList = useSelector(state => state.kind.kindList)
  const dispatch = useDispatch()
  useEffect(() => {
    dispatch(getKindListAction()) // 添加（）调用
  }, [])
  return (
    <div>
      {
        kindList &&  kindList.map(item => {
          return (
            <p key = { item }>{ item }</p>
          )
        })
      }
    </div>
  );
};

export default Kind;
```

redux

Redux + redux-thunk

redux + react-redux

Redux + react-redux 分模块

redux + react-redux + redux-thunk

redux + react-redux + redux-thunk + 分模块

redux + react-redux + redux-saga

redux + react-redux + redux-saga + 分模块

redux-toolkit

(Redux-thunk\redux-saga\redux-promise....)



