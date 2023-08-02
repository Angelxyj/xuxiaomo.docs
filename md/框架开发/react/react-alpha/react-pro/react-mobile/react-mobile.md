# 1.项目介绍

mobile-app https://www.aliyundrive.com/s/uLCS9D63dtX 点击链接保存，或者复制本段内容，打开「阿里云盘」APP ，无需下载极速在线查看，视频原画倍速播放。

实现react移动端项目

# 2.目标：
* 能够应用CRA+React+Mobx+Antd-mobile开发C端项目
* 掌握基于React的C端项目开发流程
* 学会如何应用next优化项目

# 3.使用技术栈

* 脚手架：cra
  * dva-cli
  * umi
* 脚本：ts

* react版本：react v18 2022年更新
  * react 18
* 路由：react-router v6   2021年10-11月
  * react-router v5
* 状态管理器：mobx v6
  * redux
  * redux + react-redux
  * redux + react-redux + 分模块
  * redux + react-redux + 分模块 + redux-thunk
  * redux + react-redux + 分模块 + redux-saga
  * redux + react-redux + 分模块 + redux-thunk + immutable + redux-immutable
  * redux + react-redux + 分模块 + redux-saga + immutable + redux-immutable
  * rtk
  * mobx v6
* 组件库：antd-mobile v5
  * http://ant-design-mobile.antgroup.com/zh
  * 更像是vant UI库了
* hooks

# 4.构建项目

```sh
$ npx create-react-app react-mobile-app --template typescript
```

## 4.1 是否抽离配置文件

一般企业级项目，很少会直接抽离配置文件

> 抽离配置文件目的：对webpack进行二次封装
>
> 推荐使用 craco 进行覆盖

## 4.2 使用craco覆盖webpack配置

https://www.npmjs.com/package/@craco/craco

```sh
$ cnpm i @craco/craco -D
```

为了支持 commonjs 规范，安装如下模块

```sh
$ cnpm i @types/node -D
```

`@types/*`这种文件称之为 ts 中的声明文件（`ts中的定义的类型的一个整合`）

项目根目录创建 `craco.config.js`，代码如下：

```ts
const path = require('path')
module.exports = {
  webpack: {
    alias: {
      '@': path.resolve(__dirname, 'src')
    }
  }
}

```

为了使 TS 文件引入时的别名路径能够正常解析，需要配置 `tsconifg.json`，在 `compilerOptions`选项里添加 path 等属性。为了防止配置被覆盖，需要单独创建一个文件 `tsconfig.path.json`，添加以下代码

```json
// tsconfig.path.json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    },
    "types": [
      "node"
    ]
  }
}
```

在 `tsconifg.json` 引入配置文件：

```json
// /tsconfig.json
{
  "compilerOptions": {
    "target": "es5",
    "lib": [
      "dom",
      "dom.iterable",
      "esnext"
    ],
    "allowJs": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "strict": true,
    "forceConsistentCasingInFileNames": true,
    "noFallthroughCasesInSwitch": true,
    "module": "esnext",
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx"
  },
  "extends": "./tsconfig.path.json", //+++
  "include": [
    "src"
  ]
}
```

修改 `package.json` 如下：

```json
"scripts": {
  "start": "craco start",
  "build": "craco build",
  "test": "craco test"
},
```

```sh
$ npm run start
```

## 4.3 确定项目 css 预处理器

https://create-react-app.bootcss.com/docs/adding-a-sass-stylesheet

```sh
$ cnpm i node-sass sass -D
```

cra 默认自带sass支持，只需要安装模块即可自动启动

## 4.4 改造项目目录结构

```
- mobile-react-app
  - src
    - api
    - components
    - router
    - store
    - utils
    - views
    App.tsx
    index.tsx
    logo.svg
    react-app-env.d.ts
    reportWebVitals.ts
```

```tsx
// src/index.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import ErrorBoundary from './ErrorBundary';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLDivElement
);
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <App />
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();

```

```tsx
// src/App.tsx
import React, { FC } from 'react';

interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <>App</>
  )
}

export default App
```

```tsx
// src/ErrorBundary.tsx
import React from 'react'
// 如何给类组件添加类型注解
interface IState {
  hasError: boolean
}
class ErrorBoundary extends React.Component <any, IState> {
// class ErrorBoundary extends React.Component <{ children: any }, {hasError: boolean}> {
  constructor(props: any) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error: any) {
    // 更新 state 使下一次渲染可以显示降级 UI
    return { hasError: true };
  }

  componentDidCatch(error: any, info: { componentStack: any; }) {
    // "组件堆栈" 例子:
    //   in ComponentThatThrows (created by App)
    //   in ErrorBoundary (created by App)
    //   in div (created by App)
    //   in App
    console.log(info.componentStack);
  }

  render() {
    if (this.state.hasError) {
      // 你可以渲染任何自定义的降级 UI
      return <h1>代码出错了，请仔细检查一下</h1>;
    }

    return this.props.children; 
  }
}

export default ErrorBoundary
```

# 5 构建项目基本结构

```scss
// src/App.scss
* {
  padding: 0;
  margin: 0;
  list-style: none;
  text-decoration: none;
}
html, body, #root, .container {
  height: 100%;
}

html {
  font-size: 26.6666667vw; // 100/375 * 100
}
body {
  font-size: 12px;
}

@media only screen and (orientation: landscape) { // 横屏
  html {
    font-size: 100px;
  }
}

.container {
  display: flex;
  flex-direction: column;
  .box {
    flex: 1;
    overflow: auto;
    display: flex;
    flex-direction: column;
    .header {
      height: 0.44rem;
      background-color: #f66;
    }
    .content {
      flex: 1;
      overflow: auto;
    }

  }

  .footer {
    height: 0.5rem;
    background-color: #efefef;
  }
}
```

```scss
// src/App.tsx
import React, { FC } from 'react';
import './App.scss';
interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <header className="header"></header>
        <div className="content"></div>
      </div>
      <footer className="footer"></footer>
    </div>
  )
}

export default App
```

# 6.构建项目基本页面

思考每个页面的头部和内容区域是根据用户的选择而一起改变的，那么可以创建以下四个基本页面

## 6.1 构建首页面

```tsx
// src/views/home/Index.tsx
import React, { FC } from 'react';

interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  return (
    <>
      <header className="header">home header</header>
      <div className="content">home content</div>
    </>
  )
};

export default Home;
```

## 6.2 构建分类页面

```tsx
// src/views/kind/Index.tsx
import React, { FC } from 'react';

interface IKindProps {
  
};

const Kind:FC<IKindProps> = () => {
  return (
    <>
      <header className="header">kind header</header>
      <div className="content">kind content</div>
    </>
  )
};

export default Kind;
```

## 6.3 构建购物车页面

```tsx
// src/views/cart/Index.tsx
import React, { FC } from 'react';

interface ICartProps {
  
};

const Cart:FC<ICartProps> = () => {
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">cart content</div>
    </>
  )
};

export default Cart;
```

## 6.4 构建个人中心页面

```tsx
// src/views/user/Index.tsx
import React, { FC } from 'react';

interface IUserProps {
  
};

const User:FC<IUserProps> = () => {
  return (
    <>
      <header className="header">user header</header>
      <div className="content">user content</div>
    </>
  )
};

export default User;
```

# 6.引入路由

https://reactrouter.com/en/main

```
cnpm i react-router-dom -S
```



```tsx
// src/index.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import { HashRouter } from 'react-router-dom'

import ErrorBoundary from './ErrorBundary';
import App from './App';
import reportWebVitals from './reportWebVitals';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLDivElement
);
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <HashRouter>
        <App />
      </HashRouter>
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();

```

```tsx
// src/App.tsx
import React, { FC } from 'react';

import { Routes, Route, Navigate } from 'react-router-dom'

import Home from '@/views/home/Index'
import Kind from '@/views/kind/Index'
import Cart from '@/views/cart/Index'
import User from '@/views/user/Index'
import NotFound from '@/views/error/404'

import './App.scss';
interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <Routes>
          <Route path="/" element={ <Navigate to="/home" />} />
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/user" element={<User />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </div>
      <footer className="footer"></footer>
    </div>
  )
}

export default App
```



> 此时地址栏分别输入
> `http://localhost:3000/home`、`http://localhost:3000/kind`、`http://localhost:3000/cart`、`http://localhost:3000/user`
> 查看项目运行结果，
> 可以得知已经可以通过路由显示不同的页面
> 但是用户一般都是通过底部选项卡来切换页面的

# 7.构建页面底部组件

在`src`文件夹下创建`components`文件夹，在`components`文件夹下创建底部组件

因为底部选项卡需要字体图标，可以选择 [iconfont](https://www.iconfont.cn/)阿里字体图标库，搜索图标，加入购物车，添加至项目`mobile-vue-app`，选择`font-class`,点击`查看在线链接`，拷贝css链接

项目根目录下`public/index.html`中引入css链接

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React App</title>
    <link rel="stylesheet" href="//at.alicdn.com/t/c/font_3665887_h3lsrioddkk.css">
</head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>

```

底部组件展示如下：

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';

interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <ul>
      <li>
        <span className="iconfont icon-shouye"></span>
        <p>首页</p>
      </li>
      <li>
        <span className="iconfont icon-fenlei"></span>
        <p>分类</p>
      </li>
      <li>
        <span className="iconfont icon-gouwuche"></span>
        <p>购物车</p>
      </li>
      <li>
        <span className="iconfont icon-shouye1"></span>
        <p>我的</p>
      </li>
    </ul>
  )
};

export default Footer;
```

```scss
// src/App.scss
* {
  padding: 0;
  margin: 0;
  list-style: none;
  text-decoration: none;
}
html, body, #root, .container {
  height: 100%;
}

html {
  font-size: 26.6666667vw; // 100/375 * 100
}
body {
  font-size: 12px;
}

@media only screen and (orientation: landscape) { // 横屏
  html {
    font-size: 100px;
  }
}

.container {
  display: flex;
  flex-direction: column;
  .box {
    flex: 1;
    overflow: auto;
    display: flex;
    flex-direction: column;
    .header {
      height: 0.44rem;
      background-color: #f66;
    }
    .content {
      flex: 1;
      overflow: auto;
    }

  }

  .footer {
    height: 0.5rem;
    background-color: #efefef;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      li {
        flex: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        span {
          font-size: 0.24rem;
        }
        p {
          font-size: 0.12rem;
        }
      }
    }
  }
}
```



# 8.点击页面底部跳转路由

此项选择使用声明式导航跳转

react提供了两个可以使用 声明式导航跳转方式 ： Link   NavLink

如果不需要设置选中的样式，可以使用Link 组件

如果需要设置选中的样式，建议使用NavLink

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';

import { NavLink } from 'react-router-dom'

interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <ul>
      <NavLink to="/home" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
        <span className="iconfont icon-shouye"></span>
        <p>首页</p>
      </NavLink>
      <NavLink to="/kind" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
        <span className="iconfont icon-fenlei"></span>
        <p>分类</p>
      </NavLink>
      <NavLink to="/cart" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
        <span className="iconfont icon-gouwuche"></span>
        <p>购物车</p>
      </NavLink>
      <NavLink to="/user" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
        <span className="iconfont icon-shouye1"></span>
        <p>我的</p>
      </NavLink>
    </ul>
  )
};

export default Footer;
```

```sccc
// src/App.scss
* {
  padding: 0;
  margin: 0;
  list-style: none;
  text-decoration: none;
}
html, body, #root, .container {
  height: 100%;
}

html {
  font-size: 26.6666667vw; // 100/375 * 100
}
body {
  font-size: 12px;
}

@media only screen and (orientation: landscape) { // 横屏
  html {
    font-size: 100px;
  }
}

.container {
  display: flex;
  flex-direction: column;
  .box {
    flex: 1;
    overflow: auto;
    display: flex;
    flex-direction: column;
    .header {
      height: 0.44rem;
      background-color: #f66;
    }
    .content {
      flex: 1;
      overflow: auto;
    }

  }

  .footer {
    height: 0.5rem;
    background-color: #efefef;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      a {
        flex: 1;
        height: 100%;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        color: #333;
        span {
          font-size: 0.24rem;
        }
        p {
          font-size: 0.12rem;
        }
      }
    }
  }
}
```



# 9.引入UI组件库

http://ant-design-mobile.antgroup.com/zh

react 移动端项目建议使用 Ant Design Mobile

```
$ cnpm i antd-mobile -S
```

直接引入组件即可，antd-mobile 会自动为你加载 css 样式文件

# 10.封装数据请求

在vue/react项目中建议使用 `axios` 作为数据请求的方案

axios官网：http://www.axios-js.com/

```sh
$ cnpm i axios -S
```

```ts
// src/utils/request.ts
// 1.引入axios
import axios from 'axios'

// 2.项目环境
// 生产环境 process.env.NODE_ENV === 'production'  cnpm run build
// 测试环境 ？
// 开发环境 process.env.NODE_ENV === 'devlopment   cnpm run start
const isDev = process.env.NODE_ENV === 'development'

// 3.给axios添加默认选项
// axios.defaults.withCredentials = false // 设置跨域是否需要携带凭证
// axios.defaults.timeout = 6000 // 6秒超时时间
// axios.defaults.baseURL = isDev ? 'http://121.89.205.189:3000/api' : 'http://121.89.205.189:3000/api'

// 4.自定义axios
const ins = axios.create({
  baseURL: isDev ? 'http://121.89.205.189:3000/api' : 'http://121.89.205.189:3000/api',
  timeout: 6000
})


// 5.设置拦截器
// 请求的拦截器 所有的请求在开始之前先执行请求拦截器，再执行自己的请求
ins.interceptors.request.use((config) => {
  // 设置请求的loading显示 --- 使用组件不必要  ----  js模块显示
  // 设置token，一般token传递给后端通过 请求头传递 config.headers.token = ''
  return config
}, (err) => {
  return Promise.reject(err)
})

// 响应拦截器 所有的接口返回值先执行响应拦截器，再返回自己的响应的数据
ins.interceptors.response.use((response) => {
  // 关闭loading动画  --- 使用组件不必要 ----  js模块隐藏
  // 验证token，如果验证通过，返回数，如果验证不通过，直接跳转到登录页面
  return response
}, (err) => Promise.reject(err))

// 6.暴露自定义axios
export default ins
```

# 11.构建首页

## 11.1 封装首页相关数据请求

```ts
// src/api/home.ts
import request from '@/utils/request'

interface IPager {
  count?: number
  limitNum?: number
}
// 轮播图数据
export function getBannerList () {
  return request.get('/banner/list')
}
// 秒杀列表数据
export function getSeckilllist (params?: IPager) {
  return request.get('/pro/seckilllist', { params })
}
// 产品列表数据
export function getProList (params?: IPager) {
  return request.get('/pro/list', { params })
}
```

## 11.2 构建首页轮播图组件以及渲染

```tsx
// src/views/home/components/BannerComponent.tsx
import React, { FC } from 'react';

interface IBannerComponentProps {
  
};

const BannerComponent:FC<IBannerComponentProps> = () => {
  return (
    <>
      <h1>轮播图</h1>
    </>
  )
};

export default BannerComponent;
```

```tsx
// src/views/home/Index.tsx
import React, { FC } from 'react';
import BannerComponent from './components/BannerComponent';

interface IHomeProps {
};

const Home:FC<IHomeProps> = () => {
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent />
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/Index.tsx
import { getBannerList } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent bannerList = { bannerList }/>
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/BannerComponent.tsx
import { Image, Swiper } from 'antd-mobile';
import React, { FC } from 'react';
import { IBanner } from '../Index';

interface IBannerComponentProps {
  bannerList: IBanner[]
};

const BannerComponent:FC<IBannerComponentProps> = ({ bannerList }) => {
  return (
    <>
      <Swiper style={{ height: '1.5rem', overflow: 'hidden' }}>
        {
          bannerList.map((item) => (
            <Swiper.Item key={ item.bannerid }>
              <Image style={ { width: '100%' } } src={ item.img } fit='cover'/>
            </Swiper.Item>
          ))
        }
      </Swiper>
    </>
  )
};

export default BannerComponent;
```

## 11.3 构建nav导航组件以及渲染

```ts
// src/utils/nav.ts
const navList = [
  { navid: 1, title: '嗨购超市', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/125678/35/5947/4868/5efbf28cEbf04a25a/e2bcc411170524f0.png' },
  { navid: 2, title: '数码电器', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/178015/31/13828/6862/60ec0c04Ee2fd63ac/ccf74d805a059a44.png' },
  { navid: 3, title: '嗨购服饰', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/41867/2/15966/7116/60ec0e0dE9f50d596/758babcb4f911bf4.png' },
  { navid: 4, title: '嗨购生鲜', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/177902/16/13776/5658/60ec0e71E801087f2/a0d5a68bf1461e6d.png' },
  { navid: 5, title: '嗨购到家', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196472/7/12807/7127/60ec0ea3Efe11835b/37c65625d94cae75.png' },
  { navid: 6, title: '充值缴费', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/185733/21/13527/6648/60ec0f31E0fea3e0a/d86d463521140bb6.png' },
  { navid: 7, title: '9.9元拼', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/36069/14/16068/6465/60ec0f67E155f9488/595ff3e606a53f02.png' },
  { navid: 8, title: '领券', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/186080/16/13681/8175/60ec0fcdE032af6cf/c5acd2f8454c40e1.png' },
  { navid: 9, title: '领金贴', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/196711/35/12751/6996/60ec1000E21b5bab4/38077313cb9eac4b.png' },
  { navid: 10, title: 'plus会员', imgurl: 'https://m.360buyimg.com/mobilecms/s120x120_jfs/t1/37709/6/15279/6118/60ec1046E4b5592c6/a7d6b66354efb141.png' }
]

export default navList
```

```tsx
// src/views/home/Index.tsx
import { getBannerList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent bannerList = { bannerList }/>
        <NavComponent list = { navList } />
        <SeckillComponent list = { seckillList }/>
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/NavComponent.tsx
import { Grid, Image } from 'antd-mobile';
import React, { FC } from 'react';

interface INavComponentProps {
  list: INav[]
};
export interface INav {
  navid: number
  title: string
  imgurl: string
}

const NavComponent:FC<INavComponentProps> = ({ list }) => {
  return (
    <>
      <Grid columns={5} gap={8}>
        {
          list.map(item => (
            <Grid.Item key = { item.navid }>
              <Image src={ item.imgurl } style={{ width: 50, height: 50 }}/>
              <p>{ item.title }</p>
            </Grid.Item>
          ))
        }
      </Grid>
    </>
  )
};

export default NavComponent;
```

## 11.4 构建秒杀列表实现

```tsx
// src/views/home/components/SeckillComponent.tsx
import React, { FC } from 'react';

interface ISeckillComponentProps {
  
};

const SeckillComponent:FC<ISeckillComponentProps> = () => {
  return (
    <>
      <h1>秒杀列表</h1>
    </>
  )
};

export default SeckillComponent;
```

```tsx
// src/views/home/Index.tsx
import { getBannerList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent bannerList = { bannerList }/>
        <NavComponent list = { navList } />
        <SeckillComponent list = { seckillList }/>
      </div>
    </>
  )
};

export default Home;
```

```tsx
// src/views/home/components/SeckillComponent.tsx
import { Grid, Image } from 'antd-mobile';
import React, { FC } from 'react';
import { IPro } from '../Index';

interface ISeckillComponentProps {
  list: IPro[]
};

const SeckillComponent:FC<ISeckillComponentProps> = ({ list }) => {
  return (
    <>
      <Grid columns={6} gap={8}>
        { 
          list.map(item => {
            return (
              <Grid.Item key = { item.proid }>
                <Image src= { item.img1 } style={{width: 55, height: 55}}/>
                <p style={{ color: '#f66', textAlign: 'center' }}>¥{ item.originprice }</p>
              </Grid.Item>
            )
          })
        }
      </Grid>
    </>
  )
};

export default SeckillComponent;
```

## 11.5 构建产品列表

```tsx
// src/views/home/ProComponent.tsx
import React, { FC } from 'react';
import { IPro } from '../Index';

interface IProComponentProps {
  list: IPro[]
};

const ProComponent:FC<IProComponentProps> = ({ list }) => {
  return (
    <>
      <h1>产品列表</h1>
    </>
  )
};

export default ProComponent;
```

```tsx
// src/views/home/Index.tsx
import { getBannerList, getProList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'
import ProComponent from './components/ProComponent';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  const [proList, setProList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
    getProList().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent bannerList = { bannerList }/>
        <NavComponent list = { navList } />
        <SeckillComponent list = { seckillList }/>
        <ProComponent list = { proList }></ProComponent>
      </div>
    </>
  )
};

export default Home;
```



```tsx
// src/views/home/ProComponent.tsx
import React, { FC, memo } from 'react';
import { Image } from 'antd-mobile'
import { IPro } from '../Index';

interface IProComponentProps {
  list: IPro[]

};

const WaterfallItem = ({context}: any) => {
  console.log(context)
  return (
    <div className='item'>
      <div className="itemImage">
        <Image src = {context.img1} />
      </div>
      <div className="itemInfo">
        <div>{ context.proname }</div>
        <div>¥{ context.originprice }</div>
      </div>
    </div>
  )
}
const ProComponent:FC<IProComponentProps> = memo(({ list }) => {
  console.log(list)
  return (
    <>
      <ul style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 0 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
      <ul  style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 1 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
    </>
  )
});

export default ProComponent
```

## 11.6 实现上拉加载操作

http://ant-design-mobile.antgroup.com/zh/components/infinite-scroll

```tsx
// src/views/home/Index.tsx
import { getBannerList, getProList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'
import ProComponent from './components/ProComponent';
import { InfiniteScroll } from 'antd-mobile';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  const [proList, setProList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
    getProList().then(res => setProList(res.data.data))
  }, [])

  const [hasMore, setHasMore] = useState(true)
  const [count, setCount] = useState(2)
  async function loadMore() {
    const res = await getProList({ count })
    setProList([...proList, ...res.data.data])
    setCount(count+1)
    setHasMore(res.data.data.length > 0)
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <BannerComponent bannerList = { bannerList }/>
        <NavComponent list = { navList } />
        <SeckillComponent list = { seckillList }/>
        <ProComponent list = { proList }></ProComponent>
        <InfiniteScroll loadMore={loadMore} hasMore={hasMore} />
      </div>
    </>
  )
};

export default Home;
```

## 11.7 实现下拉刷新

http://ant-design-mobile.antgroup.com/zh/components/pull-to-refresh

```tsx
// src/views/home/Index.tsx
import { getBannerList, getProList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'
import ProComponent from './components/ProComponent';
import { InfiniteScroll, PullToRefresh } from 'antd-mobile';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  const [proList, setProList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
    getProList().then(res => setProList(res.data.data))
  }, [])

  const [hasMore, setHasMore] = useState(true)
  const [count, setCount] = useState(2)
  async function loadMore() {
    const res = await getProList({ count })
    setProList([...proList, ...res.data.data])
    setCount(count+1)
    setHasMore(res.data.data.length > 0)
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content">
        <PullToRefresh
          onRefresh={async () => {
            const res = await getProList()
            setProList(res.data.data)
            setHasMore(true)
            setCount(2)
          }}
        >
          <BannerComponent bannerList = { bannerList }/>
          <NavComponent list = { navList } />
          <SeckillComponent list = { seckillList }/>
          <ProComponent list = { proList }></ProComponent>
        </PullToRefresh>
        <InfiniteScroll loadMore={loadMore} hasMore={hasMore} />
      </div>
    </>
  )
};

export default Home;
```

## 11.8返回顶部

分析清除到底是哪一个容器产生了滚动条

分析得知 content 容器产生了滚动条，可以给它绑定一个 scroll 事件用于判断 回到顶部按钮显示还是不显示

通过 content 的dom的scrollTop 属性可以设置滚动条距离

图标是在一个单独的 npm 包中，如果你想使用图标，需要先安装它：

```sh
$ cnpm install --save antd-mobile-icons
```

```scss
// src/views/home/style.scss
.backTop {
  position: fixed;
  bottom: 0.6rem;
  right: 10px;
  width: 36px;
  height: 36px;
  background-color: #fff;
  border: 1px solid #efefef;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  user-select: none;
}
```

```tsx
// src/views/home/Index.tsx
import { getBannerList, getProList, getSeckilllist } from '@/api/home';
import React, { FC, useEffect, useRef, useState } from 'react';
import BannerComponent from './components/BannerComponent';
import SeckillComponent from './components/SeckillComponent';
import NavComponent from './components/NavComponent'
import navList from '@/utils/nav'
import ProComponent from './components/ProComponent';
import { InfiniteScroll, PullToRefresh } from 'antd-mobile';
import { UpOutline } from 'antd-mobile-icons';
import './style.scss'

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])
  const [seckillList, setSeckillList] = useState<IPro[]>([])
  const [proList, setProList] = useState<IPro[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
    getSeckilllist().then(res => setSeckillList(res.data.data))
    getProList().then(res => setProList(res.data.data))
  }, [])

  const [hasMore, setHasMore] = useState(true)
  const [count, setCount] = useState(2)
  async function loadMore() {
    const res = await getProList({ count })
    setProList([...proList, ...res.data.data])
    setCount(count+1)
    setHasMore(res.data.data.length > 0)
  }
  const [top, setTop] = useState(0)
  const contentRef = useRef<any>()
  const backTop = () => {
    contentRef.current.scrollTop = 0
  }
  return (
    <>
      <header className="header">home header</header>
      <div className="content" ref={contentRef} onScroll={(event) => {
        setTop((event.target as HTMLDivElement).scrollTop)
      }}>
        <PullToRefresh
          onRefresh={async () => {
            const res = await getProList()
            setProList(res.data.data)
            setHasMore(true)
            setCount(2)
          }}
        >
          <BannerComponent bannerList = { bannerList }/>
          <NavComponent list = { navList } />
          <SeckillComponent list = { seckillList }/>
          <ProComponent list = { proList }></ProComponent>
        </PullToRefresh>
        <InfiniteScroll loadMore={loadMore} hasMore={hasMore} />
        {
          top > 300 && <div className="backTop" onClick={ backTop }><UpOutline /></div>
        }
      </div>
    </>
  )
};

export default Home;
```

## 11.9 优化项目

提取首页面组件的业务逻辑，封装自定义hooks，统一导出

```tsx
// src/views/home/components/index.ts
export { default as  BannerComponent } from './BannerComponent';
export { default as  SeckillComponent } from './SeckillComponent';
export { default as  NavComponent } from './NavComponent'
export { default as  ProComponent } from './ProComponent';
```

```tsx
// src/views/home/hooks/useBanner.ts

import { getBannerList } from "@/api/home"
import { useEffect, useState } from "react"
import { IBanner } from "../Index"

const useBanner = () => {
  const [bannerList, setBannerList] = useState<IBanner[]>([])

  useEffect(() => {
    getBannerList().then(res => setBannerList(res.data.data))
  }, [])

  return {
    bannerList
  }
}

export default useBanner
```

```ts
// src/views/home/hooks/useNav.ts

import navList from '@/utils/nav'

const useNav = () => {
  
  return {
    navList
  }
}

export default useNav
```

```ts
// src/views/home/hooks/useSeckill.ts

import { getSeckilllist } from "@/api/home"
import { useEffect, useState } from "react"
import { IPro } from "../Index"

const useSeckill = () => {
  const [seckillList, setSeckillList] = useState<IPro[]>([])

  useEffect(() => {
    getSeckilllist().then(res => setSeckillList(res.data.data))
  }, [])

  return {
    seckillList
  }
}

export default useSeckill
```

```tsx
// src/views/home/hooks/usePro.ts

import { getProList } from "@/api/home"
import { useEffect, useState } from "react"
import { IPro } from "../Index"

const usePro = () => {
  const [proList, setProList] = useState<IPro[]>([])  

  useEffect(() => {
    getProList().then(res => setProList(res.data.data))
  }, [])
  const [hasMore, setHasMore] = useState(true)
  const [count, setCount] = useState(2)
  async function loadMore() {
    const res = await getProList({ count })
    setProList([...proList, ...res.data.data])
    setCount(count+1)
    setHasMore(res.data.data.length > 0)
  }
  const pullRefresh = async () => {
    const res = await getProList()
    setProList(res.data.data)
    setHasMore(true)
    setCount(2)
  }
  return {
    proList,
    loadMore,
    hasMore,
    pullRefresh
  }
}

export default useProt
```



```ts
// src/views/home/hooks/useBackTop.tsx

import React, { useRef, useState } from "react"

const useBackTop = () => {
  const [top, setTop] = useState(0)
  const contentRef = useRef<any>()
  const backTop = () => {
    contentRef.current.scrollTop = 0
  }
  const scroll = (event: React.UIEvent<HTMLDivElement, UIEvent>) => {
    setTop((event.target as HTMLDivElement).scrollTop)
  }
  return {
    top, contentRef, backTop, scroll
  }
}

export default useBackTop
```

```
// src/views/home/hooks/index.ts

export { default as useBanner } from './useBanner'
export { default as useNav } from './useNav'
export { default as useSeckill } from './useSeckill'
export { default as usePro } from './usePro'
export { default as useBackTop } from './useBackTop'
```

```tsx
// src/views/home/Index.tsx
import React, { FC, useEffect, useRef, useState } from 'react';

import { BannerComponent, NavComponent, SeckillComponent, ProComponent } from './components'

import { InfiniteScroll, PullToRefresh } from 'antd-mobile';
import { UpOutline } from 'antd-mobile-icons';
import './style.scss'
import { useBackTop, useBanner, useNav, usePro, useSeckill } from './hooks';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const { bannerList } = useBanner()
  const { navList } = useNav()
  const { seckillList } = useSeckill()
  const { proList, loadMore, pullRefresh, hasMore } = usePro()
  const { contentRef, scroll, top, backTop } = useBackTop()
  
  return (
    <>
      <header className="header">home header</header>
      <div className="content" ref={contentRef} onScroll={ scroll }>
        <PullToRefresh
          onRefresh={ pullRefresh }
        >
          <BannerComponent bannerList = { bannerList }/>
          <NavComponent list = { navList } />
          <SeckillComponent list = { seckillList }/>
          <ProComponent list = { proList }></ProComponent>
        </PullToRefresh>
        <InfiniteScroll loadMore={loadMore} hasMore={hasMore} />
        {
          top > 300 && <div className="backTop" onClick={ backTop }><UpOutline /></div>
        }
      </div>
    </>
  )
};

export default Home;
```

现在主流手机都有安全区域，那么写代码时一定要注意

http://ant-design-mobile.antgroup.com/zh/components/safe-area

## 11.10 自定义头部

````scss
// src/views/home/components/Header.scss
.header {
  ul {
    width: 100%;
    height: 100%;
    display: flex;
    li {
      height: 100%;

      display: flex;
      justify-content: center;
      align-items: center;

      color: #fff;
      

      &:nth-child(1), &:nth-child(3) {
        width: 50px;
      }
      &:nth-child(2) {
        flex: 1;
        .searchBox {
          width: 100%;
          height: 70%;
          background-color: #fff;
          border-radius: 16px;
          color: #666;
          display: flex;
          .adm-image-img {
            width: 40px;
            margin-top: 4px;
            margin-left: 10px;
          }
          .divider {
            width: 12px;
            font-size: 24px;
            margin-left: 10px;
            color: #999;
          }
          .antd-mobile-icon {
            width: 18px;
            height: 18px;
            margin-top: 6px;
            display: flex;
            justify-content: center;
            align-items: center;
          }
          .searchText {
            flex: 1;
            line-height: .31rem;
            display: flex;
            align-items: center;
          }
        }
      }
    }
  }
}
````

```tsx
// src/views/home/components/HeaderComponent.tsx
import React, { FC } from 'react';
import { Image } from 'antd-mobile'
import { SearchOutline } from 'antd-mobile-icons';

import logo from './logo.png'
import './Header.scss';
interface IHeaderComponentProps {
  
};

const HeaderComponent:FC<IHeaderComponentProps> = ({}) => {
  return (
    <header className="header">
    <ul>
      <li>西安</li>
      <li>
        <div className="searchBox">
          <Image src={ logo } />
          <span className="divider ">|</span>
          <SearchOutline fontSize={18} />
          <span className="searchText">立柜式空调</span>
        </div>
      </li>
      <li>登录</li>
    </ul>
  </header>
  )
};

export default HeaderComponent;
```

```ts
// src/views/home/components/index.ts
export { default as  BannerComponent } from './BannerComponent';
export { default as  SeckillComponent } from './SeckillComponent';
export { default as  NavComponent } from './NavComponent'
export { default as  ProComponent } from './ProComponent';
export { default as  HeaderComponent } from './HeaderComponent';
```

```tsx
// src/views/home/Index.tsx
import React, { FC } from 'react';

import { BannerComponent, NavComponent, SeckillComponent, ProComponent, HeaderComponent } from './components'

import { InfiniteScroll, PullToRefresh } from 'antd-mobile';
import { UpOutline } from 'antd-mobile-icons';
import './style.scss'
import { useBackTop, useBanner, useNav, usePro, useSeckill } from './hooks';

interface IHomeProps {
};
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
}
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Home:FC<IHomeProps> = () => {
  const { bannerList } = useBanner()
  const { navList } = useNav()
  const { seckillList } = useSeckill()
  const { proList, loadMore, pullRefresh, hasMore } = usePro()
  const { contentRef, scroll, top, backTop } = useBackTop()
  
  return (
    <>
      {/* <header className="header">home header</header> */}
      <HeaderComponent></HeaderComponent>
      <div className="content" ref={contentRef} onScroll={ scroll }>
        <PullToRefresh
          onRefresh={ pullRefresh }
        >
          <BannerComponent bannerList = { bannerList }/>
          <NavComponent list = { navList } />
          <SeckillComponent list = { seckillList }/>
          <ProComponent list = { proList }></ProComponent>
        </PullToRefresh>
        <InfiniteScroll loadMore={loadMore} hasMore={hasMore} />
        {
          top > 300 && <div className="backTop" onClick={ backTop }><UpOutline /></div>
        }
      </div>
    </>
  )
};

export default Home;
```

# 12.实现详情

## 12.1 构建详情页面以及路由

* 构建详情页面组件

```tsx
// src/views/detail/Index.tsx
import React, { FC } from 'react';

interface IDetailProps {
};

const Detail:FC<IDetailProps> = () => {
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">detail content</div>
    </>
  )
};

export default Detail;
```

* 构建路由

```tsx
// src/App.tsx
import React, { FC } from 'react';

import { Routes, Route, Navigate } from 'react-router-dom'

import Home from '@/views/home/Index'
import Kind from '@/views/kind/Index'
import Cart from '@/views/cart/Index'
import User from '@/views/user/Index'
import NotFound from '@/views/error/404'

import Footer from '@/components/Footer'

import './App.scss';
import Detail from './views/detail/Index';
interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <Routes>
          <Route path="/" element={ <Navigate to="/home" />} />
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/user" element={<User />} />
          <Route path="/detail/:proid" element={<Detail />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </div>
      <footer className="footer">
        <Footer />
      </footer>
    </div>
  )
}

export default App
```

> 通过访问地址发现可以跳转到详情，但是详情页面不应有 底部选项卡，需要处理

```tsx
// src/components/Footer.tsx
import React, { FC } from 'react';

import { NavLink } from 'react-router-dom'

interface IFooterProps {
  
};

const Footer:FC<IFooterProps> = () => {
  return (
    <footer className="footer">
      <ul>
        <NavLink to="/home" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
          <span className="iconfont icon-shouye"></span>
          <p>首页</p>
        </NavLink>
        <NavLink to="/kind" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
          <span className="iconfont icon-fenlei"></span>
          <p>分类</p>
        </NavLink>
        <NavLink to="/cart" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
          <span className="iconfont icon-gouwuche"></span>
          <p>购物车</p>
        </NavLink>
        <NavLink to="/user" style = {({ isActive }) => isActive ? { color: '#f66' }: undefined }>
          <span className="iconfont icon-shouye1"></span>
          <p>我的</p>
        </NavLink>
      </ul>
    </footer>
  )
};

export default Footer;
```



```tsx
// src/App.tsx
import React, { FC } from 'react';

import { Routes, Route, Navigate } from 'react-router-dom'

import Home from '@/views/home/Index'
import Kind from '@/views/kind/Index'
import Cart from '@/views/cart/Index'
import User from '@/views/user/Index'
import NotFound from '@/views/error/404'

import Footer from '@/components/Footer'

import './App.scss';
import Detail from './views/detail/Index';
interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <Routes>
          <Route path="/" element={ <Navigate to="/home" />} />
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/user" element={<User />} />
          <Route path="/detail/:proid" element={<Detail />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </div>
      <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element={<Footer />} />
      </Routes>
    </div>
  )
}

export default App
```

## 12.2 点击列表进入产品详情

* 秒杀列表声明式进入详情

```tsx
// src/views/home/components/SeckillComponent.tsx
import { Grid, Image } from 'antd-mobile';
import React, { FC } from 'react';
import { Link } from 'react-router-dom';
import { IPro } from '../Index';

interface ISeckillComponentProps {
  list: IPro[]
};

const SeckillComponent:FC<ISeckillComponentProps> = ({ list }) => {
  return (
    <>
      <Grid columns={6} gap={8}>
        { 
          list.map(item => {
            return (
              <Link key = { item.proid } to={ '/detail/' + item.proid }>
                <Grid.Item >
                  <Image src= { item.img1 } style={{width: 55, height: 55}}/>
                  <p style={{ color: '#f66', textAlign: 'center' }}>¥{ item.originprice }</p>
                </Grid.Item>
              </Link>
            )
          })
        }
      </Grid>
    </>
  )
};

export default SeckillComponent;
```

* 产品列表编程式进入详情

```tsx
// src/views/home/ProComponent.tsx
import React, { FC, memo } from 'react';
import { Image } from 'antd-mobile'
import { IPro } from '../Index';
import { useNavigate } from 'react-router-dom';

interface IProComponentProps {
  list: IPro[]

};

const WaterfallItem = ({context}: any) => {
  console.log(context)
  const navigate = useNavigate()
  return (
    <div className='item' onClick={ () => {
      navigate('/detail/' + context.proid)
    }}>
      <div className="itemImage">
        <Image src = {context.img1} style={{maxHeight: '1.8rem'}}/>
      </div>
      <div className="itemInfo">
        <div style={{ maxHeight: '36px', overflow: 'hidden',}}>{ context.proname }</div>
        <div>¥{ context.originprice }</div>
      </div>
    </div>
  )
}
const ProComponent:FC<IProComponentProps> = memo(({ list }) => {
  console.log(list)
  return (
    <>
      <ul style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 0 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
      <ul  style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 1 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
    </>
  )
});

export default ProComponent
```

## 12.3 详情页获取路由参数

```tsx
// src/views/detail/Index.tsx
import React, { FC } from 'react';
import { useParams } from 'react-router-dom';

interface IDetailProps {
};

const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">detail content</div>
    </>
  )
};

export default Detail;
```

## 12.4 封装详情页数据请求

```ts
// src/api/detail.ts
import request from '@/utils/request'

interface IPager {
  count?: number
  limitNum?: number
}

export function getProDetail (proid: string) {
  return request.get('/pro/detail/' + proid)
}

// 详情 猜你喜欢 - 推荐
export function getRecommendList (params?: IPager) {
  return request.get('/pro/recommendlist', { params })
}
```

```tsx
// src/views/detail/Index.tsx
import React, { FC, useEffect, useState } from 'react';
import { useParams } from 'react-router-dom';
import { getDetailData } from '@/api/detail'
interface IDetailProps {
  
};

const Detail:FC<IDetailProps> = () => {
  const { proid } = useParams()
  console.log(proid)


  const [obj, setObj] = useState({
    banners: [],
    proname: '',
    originprice: 0,
    discount: 0,
    brand: '',
    category: '',
    sales: 0,
    issale: 1
  })
  useEffect(() => {
    getDetailData(proid!).then(res => {
      console.log(res.data.data)
      setObj({
        banners: res.data.data.banners[0].split(','),
        proname: res.data.data.proname,
        originprice:  res.data.data.originprice,
        discount: res.data.data.discount,
        brand:  res.data.data.brand,
        category: res.data.data.category,
        sales: res.data.data.sales,
        issale: res.data.data.issale
      })
    })
  }, [proid])

  return (
    <>
      <header className="header">Detail header</header>
      <div className="content">Detail content</div>
    </>
  )
};

export default Detail;
```

## 12.5 渲染详情页面

### 12.5.1 轮播图以及大图预览

```tsx
// src/views/detail/Index.tsx
import { getProDetail } from '@/api/detail';
import { Image, ImageViewer, Swiper } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })

  useEffect(() => {
    getProDetail(proid!).then(res => {
      res.data.data.banners = res.data.data.banners[0].split(',')
      console.log(res.data.data)
      setObj(res.data.data)
    })
  }, [])
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">
        <Swiper ref={swiperRef}>
          {
            obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
      </div>
    </>
  )
};

export default Detail;
```



> 大图预览遇到了 点击穿透问题
>
> * 使用tap事件代替 click 事件（tap事件原生不支持，需要额外引入插件）
>
> * 使用mouse事件代替click 事件
>
> * 使用fastclick事件代替click事件（一般页面引入插件即可）
>
>   https://antd-mobile-v2.surge.sh/docs/react/introduce-cn
>
>   > 引入 [FastClick](https://github.com/ftlabs/fastclick) 并且设置 html `meta` (更多参考 [#576](https://github.com/ant-design/ant-design-mobile/issues/576))
>   >
>   > 引入 Promise 的 fallback 支持 (部分安卓手机不支持 Promise)
>
> ```html
> <!DOCTYPE html>
> <html>
> <head>
>   <!-- set `maximum-scale` for some compatibility issues -->
>   <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no" />
>   <script src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></script>
>   <script>
>     if ('addEventListener' in document) {
>       document.addEventListener('DOMContentLoaded', function() {
>         FastClick.attach(document.body);
>       }, false);
>     }
>     if(!window.Promise) {
>       document.writeln('<script src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js"'+'>'+'<'+'/'+'script>');
>     }
>   </script>
> </head>
> <body></body>
> </html>
> ```
>
> 

`public/index.html`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />
    <!-- <meta name="viewport" content="width=device-width, initial-scale=1" /> -->
    <meta name="theme-color" content="#000000" />
    <meta
      name="description"
      content="Web site created using create-react-app"
    />
    <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
    <!--
      manifest.json provides metadata used when your web app is installed on a
      user's mobile device or desktop. See https://developers.google.com/web/fundamentals/web-app-manifest/
    -->
    <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
    <!--
      Notice the use of %PUBLIC_URL% in the tags above.
      It will be replaced with the URL of the `public` folder during the build.
      Only files inside the `public` folder can be referenced from the HTML.

      Unlike "/favicon.ico" or "favicon.ico", "%PUBLIC_URL%/favicon.ico" will
      work correctly both with client-side routing and a non-root public URL.
      Learn how to configure a non-root public URL by running `npm run build`.
    -->
    <title>React App</title>
    <link rel="stylesheet" href="//at.alicdn.com/t/c/font_3665887_h3lsrioddkk.css">
    <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no" />
  <script src="https://as.alipayobjects.com/g/component/fastclick/1.0.6/fastclick.js"></script>
  <script>
    if ('addEventListener' in document) {
      document.addEventListener('DOMContentLoaded', function() {
        FastClick.attach(document.body);
      }, false);
    }
    if(!window.Promise) {
      document.writeln('<script src="https://as.alipayobjects.com/g/component/es6-promise/3.2.2/es6-promise.min.js"'+'>'+'<'+'/'+'script>');
    }
  </script>
</head>
  <body>
    <noscript>You need to enable JavaScript to run this app.</noscript>
    <div id="root"></div>
    <!--
      This HTML file is a template.
      If you open it directly in the browser, you will see an empty page.

      You can add webfonts, meta tags, or analytics to this file.
      The build step will place the bundled scripts into the <body> tag.

      To begin the development, run `npm start` or `yarn start`.
      To create a production bundle, use `npm run build` or `yarn build`.
    -->
  </body>
</html>

```



### 12.5.2 构建产品详细信息

```scss
// src/views/detail/style.scss
.proInfo {
  background-color: #fff;
  padding: 15px;
  border-bottom-right-radius: 16px;
  border-bottom-left-radius: 16px;
  .priceBox {
    span {
      line-height: 32px;
      &:nth-child(1) {
        font-size: 24px;
        color: #f66;
      }
      &:nth-child(2) {
        float: right;
      }
    }
  }
  .proName {
    font-weight: bold;
    font-size: 0.14rem;
    .adm-tag {
      margin-right: 5px;
    }
  }
}
```

```tsx
// src/views/detail/Index.tsx
import { getProDetail } from '@/api/detail';
import { Image, ImageViewer, Swiper, Tag } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import './style.scss'
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })

  useEffect(() => {
    getProDetail(proid!).then(res => {
      res.data.data.banners = res.data.data.banners[0].split(',')
      console.log(res.data.data)
      setObj(res.data.data)
    })
  }, [])
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">
        <Swiper ref={swiperRef}>
          {
            obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
        <div className="proInfo">
          <div className="priceBox">
            <span>￥{ obj.originprice }</span>
            <span>销量：{ obj.sales }</span>
          </div>
          <div className="proName">
            <Tag color='danger'>{ obj.brand }</Tag>
            <Tag color='primary'>{ obj.category }</Tag>
            <span>{ obj.proname }</span>
          </div>
        </div>
      </div>
    </>
  )
};

export default Detail;
```

### 12.5.3 猜你喜欢

```
// src/views/detail/ProComponent.tsx
import React, { FC, memo } from 'react';
import { Image } from 'antd-mobile'
import { IPro } from './Index';
import { useNavigate } from 'react-router-dom';

interface IProComponentProps {
  list: IPro[]

};

const WaterfallItem = ({context}: any) => {
  console.log(context)
  const navigate = useNavigate()
  return (
    <div className='item' onClick={ () => {
      navigate('/detail/' + context.proid)
    }}>
      <div className="itemImage">
        <Image src = {context.img1} style={{maxHeight: '1.8rem'}}/>
      </div>
      <div className="itemInfo">
        <div style={{ maxHeight: '36px', overflow: 'hidden',}}>{ context.proname }</div>
        <div>¥{ context.originprice }</div>
      </div>
    </div>
  )
}
const ProComponent:FC<IProComponentProps> = memo(({ list }) => {
  console.log(list)
  return (
    <>
      <ul style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 0 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
      <ul  style={{ float: 'left', width: '48%', margin: '5px 1%'}}>
        {
          list.map((item, index) => {
            return index % 2 === 1 ? <WaterfallItem context={item}/> : null
          })
        }
      </ul>
    </>
  )
});

export default ProComponent
```



```tsx
// src/views/detail/Index.tsx
import { getProDetail, getRecommendList } from '@/api/detail';
import { Divider, Image, ImageViewer, Swiper, Tag } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import ProComponent from './ProComponent';
import './style.scss'
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })

  useEffect(() => {
    getProDetail(proid!).then(res => {
      res.data.data.banners = res.data.data.banners[0].split(',')
      console.log(res.data.data)
      setObj(res.data.data)
    })
  }, [])
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()

  const [proList, setProList] = useState([]) // ++++
  useEffect(() => { // ++++
    getRecommendList().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">
        <Swiper ref={swiperRef}>
          {
            obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
        <div className="proInfo">
          <div className="priceBox">
            <span>￥{ obj.originprice }</span>
            <span>销量：{ obj.sales }</span>
          </div>
          <div className="proName">
            <Tag color='danger'>{ obj.brand }</Tag>
            <Tag color='primary'>{ obj.category }</Tag>
            <span>{ obj.proname }</span>
          </div>
        </div>
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />
      </div>
    </>
  )
};

export default Detail;
```



### 12.5.4 详情底部

```scss
// src/views/detail/style.scss
.proInfo {
  background-color: #fff;
  padding: 15px;
  border-bottom-right-radius: 16px;
  border-bottom-left-radius: 16px;
  .priceBox {
    span {
      line-height: 32px;
      &:nth-child(1) {
        font-size: 24px;
        color: #f66;
      }
      &:nth-child(2) {
        float: right;
      }
    }
  }
  .proName {
    font-weight: bold;
    font-size: 0.14rem;
    .adm-tag {
      margin-right: 5px;
    }
  }
}

.detailFooter {
  position: fixed;
  bottom: 0;
  height: 0.5rem;
  border-top: 1px solid #000;
  background-color: #fff;
  width: 100%;
  display: flex;
  li {
    display: flex;
    height: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &:nth-child(1), &:nth-child(2), &:nth-child(3) {
      flex: 1
    }
    &:nth-child(4){
      flex: 3
    }
    &:nth-child(5) {
      flex: 3
    }
  }
}
```

```tsx
// src/views/detail/Footer.tsx
import { Button } from 'antd-mobile';
import React, { FC } from 'react';
import './style.scss'
interface IFooterProps {
};

const Footer:FC<IFooterProps> = () => {
  return (
    <ul className='detailFooter'>
      <li>店铺</li>
      <li>购物车</li>
      <li>收藏</li>
      <li>
        <Button color="warning" shape="rounded">加入购物车</Button>
      </li>
      <li>
        <Button color="danger" shape="rounded">立即购买</Button>
      </li>
    </ul>
  )
};

export default Footer;
```



```tsx
// src/views/detail/Index.tsx
import { getProDetail, getRecommendList } from '@/api/detail';
import { Divider, Image, ImageViewer, Swiper, Tag } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import Footer from './Footer';
import ProComponent from './ProComponent';
import './style.scss'
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })

  useEffect(() => {
    getProDetail(proid!).then(res => {
      res.data.data.banners = res.data.data.banners[0].split(',')
      console.log(res.data.data)
      setObj(res.data.data)
    })
  }, [])
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()

  const [proList, setProList] = useState([]) // ++++
  useEffect(() => { // ++++
    getRecommendList().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      <header className="header">detail header</header>
      <div className="content">
        <Swiper ref={swiperRef}>
          {
            obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
        <div className="proInfo">
          <div className="priceBox">
            <span>￥{ obj.originprice }</span>
            <span>销量：{ obj.sales }</span>
          </div>
          <div className="proName">
            <Tag color='danger'>{ obj.brand }</Tag>
            <Tag color='primary'>{ obj.category }</Tag>
            <span>{ obj.proname }</span>
          </div>
        </div>
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />
        <Footer />
      </div>
    </>
  )
};

export default Detail;
```

### 12.5.5 详情头部

```scss
// src/views/detail/style.scss
.proInfo {
  background-color: #fff;
  padding: 15px;
  border-bottom-right-radius: 16px;
  border-bottom-left-radius: 16px;
  .priceBox {
    span {
      line-height: 32px;
      &:nth-child(1) {
        font-size: 24px;
        color: #f66;
      }
      &:nth-child(2) {
        float: right;
      }
    }
  }
  .proName {
    font-weight: bold;
    font-size: 0.14rem;
    .adm-tag {
      margin-right: 5px;
    }
  }
}

.detailFooter {
  position: fixed;
  bottom: 0;
  height: 0.5rem;
  border-top: 1px solid #000;
  background-color: #fff;
  width: 100%;
  display: flex;
  li {
    display: flex;
    height: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &:nth-child(1), &:nth-child(2), &:nth-child(3) {
      flex: 1
    }
    &:nth-child(4){
      flex: 3
    }
    &:nth-child(5) {
      flex: 3
    }
  }
}
.myHeader {
  user-select: none;
  position: fixed;
  top: 0;
  width: 100%;
  z-index: 999;
  .header1 {
    height: 0.44rem;
    padding: 6px 15px;
    box-sizing: border-box;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      li {
        &:nth-child(1), &:nth-child(3) {
          font-size: 32px;
          width: 44px;
        }
        &:nth-child(2) {
          flex: 1;
        }
      }
    }
    
  }
  .header2 {
    height: 0.44rem;
    padding: 6px 15px;
    box-sizing: border-box;
    background-color: #fff;
    ul {
      width: 100%;
      height: 100%;
      display: flex;
      li {
        &:nth-child(1), &:nth-child(3) {
          font-size: 24px;
          width: 44px;
        }
        &:nth-child(2) {
          flex: 1;
          display: flex;
          span {
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
          }
        }
      }
    }
    
  }
}
```

```tsx
// src/views/detail/Header.tsx
import React, { FC } from 'react';
import {LeftOutline, MoreOutline} from 'antd-mobile-icons' 
import './style.scss'
import { useNavigate } from 'react-router-dom';
interface IHeaderComponentProps {
  
};

const HeaderComponent:FC<IHeaderComponentProps> = ({}) => {
  const navigate = useNavigate()
  return (
    <div className='myHeader'>
      <header className="header2" v-show="scrollTop > 300">
        <ul>
          <li className="left" onClick={ () => navigate(-1) } >
            <LeftOutline />
          </li>
          <li className="middle">
            <span>详情</span>
            <span>推荐</span>
          </li>
          <li className="right">
            <MoreOutline />
          </li>
        </ul>
      </header>
    </div>
  )
};

export default HeaderComponent;
```



```tsx
// src/views/detail/Index.tsx
import { getProDetail, getRecommendList } from '@/api/detail';
import { Divider, Image, ImageViewer, Swiper, Tag } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import Footer from './Footer';
import HeaderComponent from './Header';
import ProComponent from './ProComponent';
import './style.scss'
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })

  useEffect(() => {
    getProDetail(proid!).then(res => {
      res.data.data.banners = res.data.data.banners[0].split(',')
      console.log(res.data.data)
      setObj(res.data.data)
    })
  }, [])
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()

  const [proList, setProList] = useState([]) // ++++
  useEffect(() => { // ++++
    getRecommendList().then(res => setProList(res.data.data))
  }, [])
  return (
    <>
      {/* <header className="header">detail header</header> */}
      <HeaderComponent />
      <div className="content">
        <Swiper ref={swiperRef}>
          {
            obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper>
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
        <div className="proInfo">
          <div className="priceBox">
            <span>￥{ obj.originprice }</span>
            <span>销量：{ obj.sales }</span>
          </div>
          <div className="proName">
            <Tag color='danger'>{ obj.brand }</Tag>
            <Tag color='primary'>{ obj.category }</Tag>
            <span>{ obj.proname }</span>
          </div>
        </div>
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />
        <Footer />
      </div>
    </>
  )
};

export default Detail;
```

# 13.登录功能

其实要登录需要先注册，借用vue项目的注册账户，此处直接实现登录

## 13.1 构建注册组件

```tsx
// src/views/register/components/Step1.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep1Props {
  
};

const RegisterStep1:FC<IRegisterStep1Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第一步</h1>
      <Button color='danger' onClick={ () => navigate('/register/step2')}>下一步</Button>
    </>
  )
};

export default RegisterStep1;
```

```tsx
// src/views/register/components/Step2.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep2Props {
  
};

const RegisterStep2:FC<IRegisterStep2Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第二步</h1>
      <Button color='danger' onClick={ () => navigate('/register/step3')}>下一步</Button>
    </>
  )
};

export default RegisterStep2;
```

```tsx
// src/views/register/components/Step3.tsx
import React, { FC } from 'react';
import { Button } from 'antd-mobile'
import { useNavigate } from 'react-router-dom';
interface IRegisterStep3Props {
  
};

const RegisterStep3:FC<IRegisterStep3Props> = () => {
  const navigate = useNavigate()
  return (
    <>
      <h1>注册第三步</h1>
      <Button color='danger' onClick={ () => navigate(-3)}>完成</Button>
    </>
  )
};

export default RegisterStep3;
```

```tsx
// src/views/register/components/index.ts
export { default as Step1 } from './Step1'
export { default as Step2 } from './Step2'
export { default as Step3 } from './Step3'
```

```tsx
// src/views/regiter/Index.tsx
import React, { FC } from 'react';
import { Outlet } from 'react-router-dom';

interface IRegisterProps {
};

const Register:FC<IRegisterProps> = () => {
  return (
    <>
      <header className="header">Register header</header>
      <div className="content">
        <Outlet />
      </div>
    </>
  )
};

export default Register;
```

## 13.2 构建登录组件

```tsx
// src/views/login/Index.tsx
import React, { FC } from 'react';

interface ILoginProps {
};

const Login:FC<ILoginProps> = () => {
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">Login content</div>
    </>
  )
};

export default Login;
```

## 13.3 设置登录以及注册路由

注册路由使用嵌套路由

```tsx
// src/App.tsx
import React, { FC } from 'react';

import { Routes, Route, Navigate } from 'react-router-dom'

import Home from '@/views/home/Index'
import Kind from '@/views/kind/Index'
import Cart from '@/views/cart/Index'
import User from '@/views/user/Index'
import NotFound from '@/views/error/404'

import Footer from '@/components/Footer'

import './App.scss';
import Detail from './views/detail/Index';
import Login from './views/login/Index';
import Register from './views/register/Index';
import RegisterStep1 from './views/register/components/Step1';
import RegisterStep2 from './views/register/components/Step2';
import RegisterStep3 from './views/register/components/Step3';
interface IAppProps {
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <Routes>
          <Route path="/" element={ <Navigate to="/home" />} />
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          <Route path="/cart" element={<Cart />} />
          <Route path="/user" element={<User />} />
          <Route path="/login" element={<Login />} />
          <Route path="/register" element={<Register />} >
            <Route path="/register" element={<Navigate to="/register/step1" replace={true}  />} />
            <Route path="/register/step1" element={<RegisterStep1 />} />
            <Route path="/register/step2" element={<RegisterStep2 />} />
            <Route path="/register/step3" element={<RegisterStep3 />} />
          </Route>
          <Route path="/detail/:proid" element={<Detail />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </div>
      <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element={<Footer />} />
      </Routes>
    </div>
  )
}

export default App
```

## 13.4 修改登录组件

```tsx
// src/views/login/Index.tsx
import { Button, Form, Input } from 'antd-mobile';
import React, { FC } from 'react';

interface ILoginProps {
};

const Login:FC<ILoginProps> = () => {
  const loginFn = (values: any) => {
    console.log(values)
  }
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">
      <Form
        layout='horizontal'
        footer={
          <Button block type='submit' color='danger' size='large'>
            提交
          </Button>
        }
        onFinish = { loginFn }
      >
        <Form.Item
          name='loginname'
          rules={[{ required: true, message: '账户名不能为空' }]}
        >
          <Input onChange={console.log} placeholder='用户名/邮箱/手机号' />
        </Form.Item>
        <Form.Item
          name='password'
          rules={[{ required: true, message: '密码不能为空' }]}
        >
          <Input onChange={console.log} placeholder='请输入密码' />
        </Form.Item>
       
      </Form>
      </div>
    </>
  )
};

export default Login;
```

## 13.5 封装用户数据请求

```ts
// src/api/user.ts
import request from './../utils/request'

// 检测手机号是否被注册过
export function doCheckPhone (params: { tel: string }) {
  return request.post('/user/docheckphone', params)
}

// 发送短信验证码
export function doSendMsgCode (params: { tel: string }) {
  return request.post('/user/dosendmsgcode', params)
}

// 验证验证码
export function doCheckCode (params: { tel: string, telcode: string }) {
  return request.post('/user/docheckcode', params)
}

// 设置密码完成注册
export function doFinishRegister (params: { tel: string, password: string }) {
  return request.post('/user/dofinishregister', params)
}

// 登录
export function doLogin (params: { loginname: string, password: string }) {
  return request.post('/user/login', params)
}
```

## 13.6 实现登录功能

```tsx
// src/views/login/Index.tsx
import { doLogin } from '@/api/user';
import { Button, Form, Input, Toast } from 'antd-mobile';
import React, { FC } from 'react';
import { useNavigate } from 'react-router-dom';

interface ILoginProps {
};

const Login:FC<ILoginProps> = () => {
  const navigate = useNavigate()
  const loginFn = (values: any) => {
    console.log(values)
    doLogin(values).then(res => {
      if (res.data.code === '10011') {
        Toast.show({
          content: '密码错误',
          duration: 1000
        })
      } else if (res.data.code === '10010') {
        Toast.show({
          content: '该用户还未注册',
          duration: 1000
        })
      } else {
        Toast.show({
          content: '登录成功',
          duration: 1000
        })
        // 保存数据到本地
        // 保存数据到状态管理器
        // 返回上一页
        localStorage.setItem('loginState', String(true))
        localStorage.setItem('userid', res.data.data.userid)
        localStorage.setItem('token', res.data.data.token)

        navigate(-1)
      }
    })
  }
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">
      <Form
        layout='horizontal'
        footer={
          <Button block type='submit' color='danger' size='large'>
            提交
          </Button>
        }
        onFinish = { loginFn }
      >
        <Form.Item
          name='loginname'
          rules={[{ required: true, message: '账户名不能为空' }]}
        >
          <Input onChange={console.log} placeholder='用户名/邮箱/手机号' />
        </Form.Item>
        <Form.Item
          name='password'
          rules={[{ required: true, message: '密码不能为空' }]}
        >
          <Input onChange={console.log} placeholder='请输入密码' />
        </Form.Item>
       
      </Form>
      </div>
    </>
  )
};

export default Login;
```

# 14.mobx状态管理器

https://cn.mobx.js.org/ --- v5

https://www.mobxjs.com/   -v6

## 14.1 安装

```sh
$ cnpm i mobx mobx-react -S
```

## 14.2 创建状态管理器

```ts
// src/store/modules/user.ts

import { makeAutoObservable } from "mobx"

class UserStore {
  // 初始化数据
  loginState = localStorage.getItem('loginState') === 'true'
  token = localStorage.getItem('token') || ''
  userid = localStorage.getItem('userid') || ''
  
  constructor () {
    // 讲此类设置为可被观察的
    makeAutoObservable(this)
    // this.changeLoginState = this.changeLoginState.bind(this)
    // this.changeToken = this.changeToken.bind(this)
    // this.changeUserId = this.changeUserId.bind(this)
  }

  changeLoginState (action: { payload: boolean}) {
    this.loginState = action.payload
  }
  changeToken (action: { payload: string}) {
    this.token = action.payload
  }
  changeUserId (action: { payload: string}) {
    this.userid = action.payload
  }
}

export default UserStore
```

```ts
// src/store/index.ts

import { makeAutoObservable } from "mobx";
import UserStore from "./modules/user";


class Store {
  user
  constructor () {
    makeAutoObservable(this)
    this.user = new UserStore()
  }
}

export default new Store()
```

```ts
// src/index.tsx
import React from 'react';
import ReactDOM from 'react-dom/client';

import { HashRouter } from 'react-router-dom'

import ErrorBoundary from './ErrorBundary';
import App from './App';
import reportWebVitals from './reportWebVitals';

import { Provider } from 'mobx-react'
import store from './store';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLDivElement
);
root.render(
  <React.StrictMode>
    <ErrorBoundary>
      <HashRouter>
        <Provider store = { store }>
          <App />
        </Provider>
      </HashRouter>
    </ErrorBoundary>
  </React.StrictMode>
);

reportWebVitals();

```

```tsx
// src/views/login/Index.tsx
import { doLogin } from '@/api/user';
import { Button, Form, Input, Toast } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC } from 'react';
import { useNavigate } from 'react-router-dom';

interface ILoginProps {
};

const Login:FC<ILoginProps> = (props: any) => {
  const navigate = useNavigate()
  const loginFn = (values: any) => {
    console.log(values)
    doLogin(values).then(res => {
      if (res.data.code === '10011') {
        Toast.show({
          content: '密码错误',
          duration: 1000
        })
      } else if (res.data.code === '10010') {
        Toast.show({
          content: '该用户还未注册',
          duration: 1000
        })
      } else {
        Toast.show({
          content: '登录成功',
          duration: 1000
        })
        // 保存数据到本地
        // 保存数据到状态管理器
        // 返回上一页
        localStorage.setItem('loginState', String(true))
        localStorage.setItem('userid', res.data.data.userid)
        localStorage.setItem('token', res.data.data.token)

        console.log(props)
        props.store.user.changeLoginState({ payload: true })
        props.store.user.changeUserId({ payload: res.data.data.userid })
        props.store.user.changeToken({ payload: res.data.data.token })

        navigate(-1)
      }
    })
  }
  return (
    <>
      <header className="header">Login header</header>
      <div className="content">
      <Form
        layout='horizontal'
        footer={
          <Button block type='submit' color='danger' size='large'>
            提交
          </Button>
        }
        onFinish = { loginFn }
      >
        <Form.Item
          name='loginname'
          rules={[{ required: true, message: '账户名不能为空' }]}
        >
          <Input onChange={console.log} placeholder='用户名/邮箱/手机号' />
        </Form.Item>
        <Form.Item
          name='password'
          rules={[{ required: true, message: '密码不能为空' }]}
        >
          <Input onChange={console.log} placeholder='请输入密码' />
        </Form.Item>
       
      </Form>
      </div>
    </>
  )
};

// inject('store') 将入口的文件的 传递的 store 接收，传递组件的 props属性 ，组件可以通过 props.store访问状态管理器
// observer() 将此组件设置为观察者，一旦检测到store的数据发生改变，更新视图
export default inject('store')(observer(Login));
```

# 15.加入购物车

## 15.1 封装数据请求

给请求添加token

```ts
// src/utils/request.ts
// 1.引入axios
import axios from 'axios'

// 2.项目环境
// 生产环境 process.env.NODE_ENV === 'production'  cnpm run build
// 测试环境 ？
// 开发环境 process.env.NODE_ENV === 'devlopment   cnpm run start
const isDev = process.env.NODE_ENV === 'development'

// 3.给axios添加默认选项
// axios.defaults.withCredentials = false // 设置跨域是否需要携带凭证
// axios.defaults.timeout = 6000 // 6秒超时时间
// axios.defaults.baseURL = isDev ? 'http://121.89.205.189:3000/api' : 'http://121.89.205.189:3000/api'

// 4.自定义axios
const ins = axios.create({
  baseURL: isDev ? 'http://121.89.205.189:3000/api' : 'http://121.89.205.189:3000/api',
  timeout: 10000
})


// 5.设置拦截器
// 请求的拦截器 所有的请求在开始之前先执行请求拦截器，再执行自己的请求
ins.interceptors.request.use((config) => {
  // 设置请求的loading显示 --- 使用组件不必要  ----  js模块显示
  // 设置token，一般token传递给后端通过 请求头传递 config.headers.token = ''
  config.headers.token = localStorage.getItem('token')
  return config
}, (err) => {
  return Promise.reject(err)
})

// 响应拦截器 所有的接口返回值先执行响应拦截器，再返回自己的响应的数据
ins.interceptors.response.use((response) => {
  // 关闭loading动画  --- 使用组件不必要 ----  js模块隐藏
  // 验证token，如果验证通过，返回数，如果验证不通过，直接跳转到登录页面
  if (response.data.code === '10119') {
    window.location.href="/#/login"
    
  }
  return response
}, (err) => Promise.reject(err))

// 6.暴露自定义axios
export default ins
```

```ts
// src/api/cart.ts
import request from '@/utils/request'

// 加入购物车
export function addCart (params: { userid: string, proid: string, num: number }) {
  return request.post('/cart/add', params)
}

// 获取购物车列表数据
export function getCartListData (params: { userid: string }) {
  return request.post('/cart/list', params)
}

// 删除某个用户的购物车的所有数据
export function removeAllData (params: { userid: string }) {
  return request.post('/cart/removeall', params)
}

// 删除某个用户的一条购物车的数据
export function removeOneData (params: { cartid: string }) {
  return request.post('/cart/remove', params)
}

// 更新某个用户的一条购物车的数据的选中状态
export function selectOneData (params: { cartid: string, flag: boolean }) {
  return request.post('/cart/selectone', params)
}

// 更新某个用户的购物车的所有数据的选中状态
export function selectAllData (params: { userid: string, type: boolean }) {
  return request.post('/cart/selectall', params)
}

// 更新某个用户的购物车的某个产品的数量
export function updateOneDataNum (params: { cartid: string, num: number }) {
  return request.post('/cart/updatenum', params)
}

// 推荐商品接口
export function getCartRecommendData () {
  return request.get('/pro/recommendlist')
}
```

## 15.2 加入购物车

底部组件提供了 各个选项的事件，需要按照组件提供写法去写

```tsx
// src/views/detail/Index.tsx
import { getProDetail, getRecommendList } from '@/api/detail';
import { Divider, Image, ImageViewer, Swiper, Tag } from 'antd-mobile';
import React, { FC, useEffect, useRef, useState } from 'react';
import { useParams } from 'react-router-dom';
import Footer from './Footer';
import HeaderComponent from './Header';
import ProComponent from './ProComponent';
import './style.scss'
interface IDetailProps {
};
export interface IPro {
  banners: string[]
  brand: string
  category: string
  desc: string
  discount: number
  img1: string
  img2: string
  img3: string
  img4: string
  isrecommend: number
  issale: number
  isseckill: number
  originprice: number
  proid: string
  proname: string
  sales: number
  stock: number
}
const Detail:FC<IDetailProps> = () => {
  // const params = useParams()
  // console.log(params)

  const { proid } = useParams()

  const [obj, setObj] = useState<IPro>({
    banners: [],
    brand: '',
    category: '',
    desc: '',
    discount: 0,
    img1: '',
    img2: '',
    img3: '',
    img4: '',
    isrecommend: 0,
    issale: 0,
    isseckill: 0,
    originprice: 0,
    proid: '',
    proname: '',
    sales: 0,
    stock: 0
  })
  const [visible, setVisible] = useState(false)
  const [current, setCurrent] = useState(0)
  const swiperRef = useRef<any>()

  const [proList, setProList] = useState([]) // ++++
  useEffect(() => { // ++++
    getRecommendList().then(res => setProList(res.data.data))
  }, [])
  useEffect(() => {
    getProDetail(proid!).then(res => {
      // console.log(swiperRef)
      res.data.data.banners = res.data.data.banners[0].split(',')
      // console.log(res.data.data)
      setObj(res.data.data)
      swiperRef.current && swiperRef.current!.swipeTo(0)
    })
  }, [proid])
  
  return (
    <>
      {/* <header className="header">detail header</header> */}
      <HeaderComponent />
      <div className="content">
        {
          obj.banners && <Swiper ref={swiperRef}>
          {
            obj.banners && obj.banners.map((item, index) => {
              return (
                <Swiper.Item key={ item } onClick = { () => {
                  setCurrent(index)
                  setVisible(true)
                  // swiperRef.current.swipeTo(index, true)
                }}>
                  <Image src={ item } />
                </Swiper.Item>
              )
            })
          }
        </Swiper> 
        }
        {
          visible ? <ImageViewer.Multi
          images={obj.banners}
          visible={visible}
          defaultIndex = { current }
          onIndexChange= { index => {
            swiperRef.current!.swipeTo(index)
          }}
          onClose={() => {
            setVisible(false)
          }}
        /> : null
        }
        <div className="proInfo">
          <div className="priceBox">
            <span>￥{ obj.originprice }</span>
            <span>销量：{ obj.sales }</span>
          </div>
          <div className="proName">
            <Tag color='danger'>{ obj.brand }</Tag>
            <Tag color='primary'>{ obj.category }</Tag>
            <span>{ obj.proname }</span>
          </div>
        </div>
        <Divider
          style={{
            color: '#1677ff',
            borderColor: '#1677ff',
            borderStyle: 'dashed',
          }}
        >
          猜你喜欢
        </Divider>
        <ProComponent list = { proList } />
        <Footer proid = { proid! }/>
      </div>
    </>
  )
};

export default Detail;
```

```tsx
// src/views/detail/Footer.tsx
import { addCart } from '@/api/cart';
import { Button, Toast } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC } from 'react';
import { Link, useNavigate } from 'react-router-dom';
import './style.scss'
interface IFooterProps {
  store?: any;
  // [x: string]: any;
  proid: string
};

const Footer:FC<IFooterProps> = (props) => {
  // console.log('proid', proid)
  const navigate = useNavigate()
  const loginState = props.store.user.loginState
  const userid = props.store.user.userid
  const addCartFn = () => {
    if (loginState) {
      // 调用加入购物车接口
      addCart({
        userid,
        proid: props.proid,
        num: 1
      }).then((res) => {
        if (res.data.code !== '10119') {
          Toast.show('加入购物车成功')
        }
      })
    } else {
      navigate('/login')
    }
  }
  return (
    <ul className='detailFooter'>
      <li>店铺</li>
      <li>
        <Link to="/cart" >购物车</Link>
      </li>
      <li>收藏</li>
      <li>
        <Button size="small" color="warning" shape="rounded" onClick={ addCartFn }>加入购物车</Button>
      </li>
      <li>
        <Button size="small" color="danger" shape="rounded">立即购买</Button>
      </li>
    </ul>
  )
};

export default inject('store')(observer(Footer));
```



# 16.购物车相关

## 16.1 判断登录状态

实现类似于vue的导航守卫，定义路由时处理

```tsx
// src/App.tsx
import React, { FC } from 'react';

import { Routes, Route, Navigate } from 'react-router-dom'

import Home from '@/views/home/Index'
import Kind from '@/views/kind/Index'
import Cart from '@/views/cart/Index'
import User from '@/views/user/Index'
import NotFound from '@/views/error/404'

import Footer from '@/components/Footer'

import './App.scss';
import Detail from './views/detail/Index';
import Login from './views/login/Index';
import Register from './views/register/Index';
import RegisterStep1 from './views/register/components/Step1';
import RegisterStep2 from './views/register/components/Step2';
import RegisterStep3 from './views/register/components/Step3';
import { inject, observer } from 'mobx-react';
interface IAppProps {
  store?: any
}

const App: FC<IAppProps> = (props) => {
  
  return (
    <div className="container">
      <div className="box">
        <Routes>
          <Route path="/" element={ <Navigate to="/home" />} />
          <Route path="/home" element={<Home />} />
          <Route path="/kind" element={<Kind />} />
          {/* <Route path="/cart" element={<Cart />} /> */}
          <Route path="/cart" element={
            // localStorage.getItem('loginState') === 'true' ? <Cart /> : <Navigate to="/login"  />
            props.store.user.loginState ? <Cart /> : <Navigate to="/login" replace={true} />
          } />
          <Route path="/user" element={<User />} />
          <Route path="/login" element={<Login />} />
          <Route path="/register" element={<Register />} >
            <Route path="/register" element={<Navigate to="/register/step1" replace={true}  />} />
            <Route path="/register/step1" element={<RegisterStep1 />} />
            <Route path="/register/step2" element={<RegisterStep2 />} />
            <Route path="/register/step3" element={<RegisterStep3 />} />
          </Route>
          <Route path="/detail/:proid" element={<Detail />} />
          <Route path="*" element={<NotFound />} />
        </Routes>
      </div>
      <Routes>
        <Route path="/home" element={<Footer />} />
        <Route path="/kind" element={<Footer />} />
        <Route path="/cart" element={<Footer />} />
        <Route path="/user" element={<Footer />} />
      </Routes>
    </div>
  )
}

export default inject('store')(observer(App))
```

## 16.2 判断是否有数据

```tsx
// src/views/cart/Index.tsx
import { getCartListData } from '@/api/cart';
import { Button, Empty, List, Image, Stepper } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useState } from 'react';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <List>
            {
              cartList.map(item => (
                <List.Item
                  key={item.cartid}
                  prefix={
                    <Image
                      src={item.img1}
                      fit='cover'
                      width={40}
                      height={40}
                    />
                  }
                  description={
                    <div>
                      <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                      <Stepper
                        style={{ float: 'right'}}
                        defaultValue={item.num}
                        onChange={value => {
                          console.log(value)
                        }}
                      />
                    </div>
                  }
                >
                  {item.proname}
                </List.Item>
              ))
            }
          </List>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

## 16.3 数量更新

```tsx
// src/views/cart/Index.tsx
import { getCartListData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Stepper } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useState } from 'react';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <List>
            {
              cartList.map(item => (
                <List.Item
                  key={item.cartid}
                  prefix={
                    <Image
                      src={item.img1}
                      fit='cover'
                      width={40}
                      height={40}
                    />
                  }
                  description={
                    <div>
                      <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                      <Stepper
                        style={{ float: 'right'}}
                        defaultValue={item.num}
                        onChange={value => {
                          console.log(value)
                          updateOneDataNum({cartid: item.cartid, num: value}).then(() => {
                            getCartListDataFn()
                          })
                        }}
                      />
                    </div>
                  }
                >
                  {item.proname}
                </List.Item>
              ))
            }
          </List>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

## 16.4 删除

```tsx
// src/views/cart/Index.tsx
import { getCartListData, removeOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Stepper, SwipeAction } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useState } from 'react';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <List>
            {
              cartList.map(item => (
                <SwipeAction
                  key={item.cartid}
                  rightActions={[
                    {
                      key: 'delete',
                      text: '删除',
                      color: 'danger',
                    },
                  ]}
                  onAction = { ({key}) => {
                    console.log(key)
                    if (key === 'delete') {
                      removeOneData({cartid: item.cartid}).then(() => getCartListDataFn())
                    }
                  }}
                >
                  <List.Item
                  key={item.cartid}
                  prefix={
                    <Image
                      src={item.img1}
                      fit='cover'
                      width={40}
                      height={40}
                    />
                  }
                  description={
                    <div>
                      <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                      <Stepper
                        style={{ float: 'right'}}
                        defaultValue={item.num}
                        onChange={value => {
                          console.log(value)
                          updateOneDataNum({cartid: item.cartid, num: value}).then(() => {
                            getCartListDataFn()
                          })
                        }}
                      />
                    </div>
                  }
                >
                  {item.proname}
                </List.Item>
                </SwipeAction>
                
              ))
            }
          </List>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

## 16.4 选择

```tsx
// src/views/cart/Index.tsx
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Stepper, SwipeAction, Checkbox } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useState } from 'react';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const [checked, setChecked] = useState<boolean>(true)
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
        const flag = res.data.data.every((item: ICartItem) => item.flag)
        setChecked(flag)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <>
            <List>
              {
                cartList.map(item => (
                  <SwipeAction
                    key={item.cartid}
                    rightActions={[
                      {
                        key: 'delete',
                        text: '删除',
                        color: 'danger',
                      },
                    ]}
                    onAction = { ({key}) => {
                      console.log(key)
                      if (key === 'delete') {
                        removeOneData({cartid: item.cartid}).then(() => getCartListDataFn())
                      }
                    }}
                  >
                    <List.Item
                    key={item.cartid}
                    prefix={
                      <div style={{ display: 'flex'}}>
                        <div onClick={() => {
                          selectOneData({ cartid: item.cartid, flag: !item.flag}).then(res => {
                            getCartListDataFn()
                          })
                        }}>
                          <Checkbox checked={ item.flag }></Checkbox>
                        </div>
                        <Image
                          src={item.img1}
                          fit='cover'
                          width={80}
                          height={80}
                        />
                      </div>
                    }
                    description={
                      <div>
                        <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                        <Stepper
                          style={{ float: 'right'}}
                          defaultValue={item.num}
                          onChange={value => {
                            console.log(value)
                            updateOneDataNum({cartid: item.cartid, num: value}).then(() => {
                              getCartListDataFn()
                            })
                          }}
                        />
                      </div>
                    }
                  >
                    {item.proname}
                  </List.Item>
                  </SwipeAction>
                  
                ))
              }
            </List>
            <div style={{
              position: 'fixed',
              bottom: '0',
              width: '100%',
              height: '0.5rem',
              backgroundColor: '#ccc',
              display: 'flex',
              zIndex: 999
            }}>
              <div onClick={ event => {
                event.preventDefault()
                selectAllData({
                  userid, type: !checked
                }).then(() => {
                  getCartListDataFn()
                  setChecked(!checked)
                })
              }}>
                <Checkbox checked={checked} >全选</Checkbox>
              </div>
              <div>
                <p>总价：</p>
                <p>总数：</p>
              </div>
              <Button color='danger' size="small">提交订单</Button>
            </div>
          </>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

## 16.5 计算总价以及总数量

使用useMemo计算属性

```tsx
// src/views/cart/Index.tsx
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Stepper, SwipeAction, Checkbox } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useMemo, useState } from 'react';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const [checked, setChecked] = useState<boolean>(true)

  const totalNum = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num : sum += 0
    }, 0)
  }, [cartList])

  const totalPrice = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.originprice * item.num : sum += 0
    }, 0)
  }, [cartList])
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
        const flag = res.data.data.every((item: ICartItem) => item.flag)
        setChecked(flag)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])
  return (
    <>
      <header className="header">cart header</header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <>
            <List>
              {
                cartList.map(item => (
                  <SwipeAction
                    key={item.cartid}
                    rightActions={[
                      {
                        key: 'delete',
                        text: '删除',
                        color: 'danger',
                      },
                    ]}
                    onAction = { ({key}) => {
                      console.log(key)
                      if (key === 'delete') {
                        removeOneData({cartid: item.cartid}).then(() => getCartListDataFn())
                      }
                    }}
                  >
                    <List.Item
                    key={item.cartid}
                    prefix={
                      <div style={{ display: 'flex'}}>
                        <div onClick={() => {
                          selectOneData({ cartid: item.cartid, flag: !item.flag}).then(res => {
                            getCartListDataFn()
                          })
                        }}>
                          <Checkbox checked={ item.flag }></Checkbox>
                        </div>
                        <Image
                          src={item.img1}
                          fit='cover'
                          width={80}
                          height={80}
                        />
                      </div>
                    }
                    description={
                      <div>
                        <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                        <Stepper
                          style={{ float: 'right'}}
                          defaultValue={item.num}
                          onChange={value => {
                            console.log(value)
                            updateOneDataNum({cartid: item.cartid, num: value}).then(() => {
                              getCartListDataFn()
                            })
                          }}
                        />
                      </div>
                    }
                  >
                    {item.proname}
                  </List.Item>
                  </SwipeAction>
                  
                ))
              }
            </List>
            <div style={{
              position: 'fixed',
              bottom: '0',
              width: '100%',
              height: '0.5rem',
              backgroundColor: '#ccc',
              display: 'flex',
              zIndex: 999
            }}>
              <div onClick={ event => {
                event.preventDefault()
                selectAllData({
                  userid, type: !checked
                }).then(() => {
                  getCartListDataFn()
                  setChecked(!checked)
                })
              }}>
                <Checkbox checked={checked} >全选</Checkbox>
              </div>
              <div>
                <p>总价：{ totalPrice }</p>
                <p>总数：{ totalNum }</p>
              </div>
              <Button color='danger' size="small">提交订单</Button>
            </div>
          </>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

## 16.6 购物车头部

```tsx
// src/views/cart/Index.tsx
import { getCartListData, removeOneData, selectAllData, selectOneData, updateOneDataNum } from '@/api/cart';
import { Button, Empty, List, Image, Stepper, SwipeAction, Checkbox, NavBar } from 'antd-mobile';
import { inject, observer } from 'mobx-react';
import React, { FC, useEffect, useMemo, useState } from 'react';
import { useNavigate } from 'react-router-dom';

interface ICartProps {
  store?: any
};
interface ICartItem {
  cartid: string
  discount: number
  flag: boolean
  img1: string
  num: number
  originprice: number
  proid: string
  proname: string
  userid: string
}
const Cart:FC<ICartProps> = ({ store:{ user: { userid }}}) => {
  const [cartList, setCartList] = useState<ICartItem[]>([])
  const [empty, setEmpty] = useState<boolean>(true)
  const [checked, setChecked] = useState<boolean>(true)

  const totalNum = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.num : sum += 0
    }, 0)
  }, [cartList])

  const totalPrice = useMemo(() => {
    return cartList.reduce((sum, item) => {
      return item.flag ? sum += item.originprice * item.num : sum += 0
    }, 0)
  }, [cartList])
  const getCartListDataFn = () => {
    console.log('userid', userid)
    getCartListData({ userid }).then(res => {
      console.log(res.data)
      if (res.data.code === '10020') {
        setEmpty(true)
      } else {
        setEmpty(false)
        setCartList(res.data.data)
        const flag = res.data.data.every((item: ICartItem) => item.flag)
        setChecked(flag)
      }
    })
  }
  useEffect(() => {
    getCartListDataFn()
  }, [])

  const navigate = useNavigate()
  return (
    <>
      <header className="header">
      <NavBar
          style={{
            '--height': '0.44rem',
            '--border-bottom': '1px #eee solid',
            color: '#fff'
          }}
          onBack={ () => navigate(-1) }
        >
          购物车
        </NavBar>
      </header>
      <div className="content">
        {
          empty ?
          <Empty
            style={{ padding: '64px 0' }}
            imageStyle={{ width: 128 }}
            description={ 
              <div>
                <p>购物车空空如也</p>
                <Button color="danger">立即购物</Button>
              </div>
            }
          /> : <>
            <List>
              {
                cartList.map(item => (
                  <SwipeAction
                    key={item.cartid}
                    rightActions={[
                      {
                        key: 'delete',
                        text: '删除',
                        color: 'danger',
                      },
                    ]}
                    onAction = { ({key}) => {
                      console.log(key)
                      if (key === 'delete') {
                        removeOneData({cartid: item.cartid}).then(() => getCartListDataFn())
                      }
                    }}
                  >
                    <List.Item
                    key={item.cartid}
                    prefix={
                      <div style={{ display: 'flex'}}>
                        <div onClick={() => {
                          selectOneData({ cartid: item.cartid, flag: !item.flag}).then(res => {
                            getCartListDataFn()
                          })
                        }}>
                          <Checkbox checked={ item.flag }></Checkbox>
                        </div>
                        <Image
                          src={item.img1}
                          fit='cover'
                          width={80}
                          height={80}
                        />
                      </div>
                    }
                    description={
                      <div>
                        <span style={{ color: '#f66'}}>¥{item.originprice}</span>
                        <Stepper
                          style={{ float: 'right'}}
                          defaultValue={item.num}
                          onChange={value => {
                            console.log(value)
                            updateOneDataNum({cartid: item.cartid, num: value}).then(() => {
                              getCartListDataFn()
                            })
                          }}
                        />
                      </div>
                    }
                  >
                    {item.proname}
                  </List.Item>
                  </SwipeAction>
                  
                ))
              }
            </List>
            <div style={{
              position: 'fixed',
              bottom: '0',
              width: '100%',
              height: '0.5rem',
              backgroundColor: '#ccc',
              display: 'flex',
              zIndex: 999
            }}>
              <div onClick={ event => {
                event.preventDefault()
                selectAllData({
                  userid, type: !checked
                }).then(() => {
                  getCartListDataFn()
                  setChecked(!checked)
                })
              }}>
                <Checkbox checked={checked} >全选</Checkbox>
              </div>
              <div>
                <p>总价：{ totalPrice }</p>
                <p>总数：{ totalNum }</p>
              </div>
              <Button color='danger' size="small">提交订单</Button>
            </div>
          </>

        }
      </div>
    </>
  )
};

export default inject('store')(observer(Cart));
```

# 17.项目上线

默认执行`cnpm run build` 打包出来的项目资源以绝对路径方式引入

通过给 `package.json`文件中添加`"homepage": "./"`更改为相对路径

执行`cnpm run build` 打包,上传服务器，服务器测试

http://121.89.205.189:3000/m-react/#/home

