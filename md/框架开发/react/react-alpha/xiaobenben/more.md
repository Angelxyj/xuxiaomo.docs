# 1.vue中动态组件

```sh
$ cnpm i element-plus -S
```

```js
// src/main.js
import { createApp } from 'vue'
import ElementPlus from 'element-plus'
import 'element-plus/dist/index.css'
import App from '@/App'

createApp(App).use(ElementPlus).mount('#root')
```

## 1.1 单纯的动态组件

```vue
<!-- src/App.vue -->
<template>
  <div>
    <el-button type="primary" @click="com='Home'">首页</el-button>
    <el-button type="primary" @click="com='Kind'">分类</el-button>
    <el-button type="primary" @click="com='Cart'">购物车</el-button>
    <el-button type="primary" @click="com='User'">我的</el-button>

    <!-- 动态组件 -->
    <component :is="com"></component>
  </div>
</template>

<script>
  import Home from './components/Home.vue'
  import Kind from './components/Kind.vue'
  import Cart from './components/Cart.vue'
  import User from './components/User.vue'
  export default {
    components: {
      Home, Kind, Cart, User
    },
    data () {
      return {
        com: 'Home'
      }
    }
  }
</script>
```

```vue
<!-- src/components/Home.vue -->
<template>
  <div>这里是首页</div>
</template>
```

```vue
<!-- src/components/Kind.vue -->
<template>
  <div>这里分类</div>
</template>
```

```vue
<!-- src/components/Cart.vue -->
<template>
  <div>这里是购物车</div>
</template>
```

```vue
<!-- src/components/User.vue -->
<template>
  <div>这里是个人中心</div>
</template>
```

## 1.2 页面添加表单查看效果

```vue
<!-- src/components/Home.vue -->
<template>
  <div>
    这里是首页
    <input type="text" placeholder="home">
  </div>
</template>
```

```vue
<!-- src/components/Kind.vue -->
<template>
  <div>这里分类 <input type="text" placeholder="kind"></div>
</template>
```

```vue
<!-- src/components/Cart.vue -->
<template>
  <div>这里是购物车<input type="text" placeholder="cart"></div>
</template>
```

```vue
<!-- src/components/User.vue -->
<template>
  <div>这里是个人中心<input type="text" placeholder="user"></div>
</template>
```

> 每一个页面添加了内容，切换之后再回到原来的位置，发现输入框内容缺失
>
> * 此处涉及到了组件的销毁和重建

## 1.3 保留组件的状态，避免再次重新渲染

```vue
<!-- src/App.vue -->
<template>
  <div>
    <el-button type="primary" @click="com='Home'">首页</el-button>
    <el-button type="primary" @click="com='Kind'">分类</el-button>
    <el-button type="primary" @click="com='Cart'">购物车</el-button>
    <el-button type="primary" @click="com='User'">我的</el-button>

    <!-- 动态组件 -->
    <KeepAlive>
      <component :is="com"></component>
    </KeepAlive>
  </div>
</template>

<script>
  import Home from './components/Home.vue'
  import Kind from './components/Kind.vue'
  import Cart from './components/Cart.vue'
  import User from './components/User.vue'
  export default {
    components: {
      Home, Kind, Cart, User
    },
    data () {
      return {
        com: 'Home'
      }
    }
  }
</script>
```

> 每一个页面添加了内容，切换之后再回到原来的位置，发现输入框内容还保持了原来的状态
>
> * 此处没有涉及到组件的销毁和重建
> * 其实就是页面的显示和隐藏
> * 此时触发两个钩子函数 activated deactivated

```vue
<!-- src/components/Home.vue -->
<template>
  <div>
    这里是首页
    <input type="text" placeholder="home">
  </div>
</template>

<script>
  export default {
    mounted () {
      console.log('home mounted')
    },
    activated () {
      console.log('home activated')
    },
    deactivated () {
      console.log('home deactivated')
    },
    unmounted () {
      console.log('home unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/Kind.vue -->
<template>
  <div>这里分类 <input type="text" placeholder="kind"></div>
</template>
<script>
  export default {
    mounted () {
      console.log('kind mounted')
    },
    activated () {
      console.log('kind activated')
    },
    deactivated () {
      console.log('kind deactivated')
    },
    unmounted () {
      console.log('kind unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/Cart.vue -->
<template>
  <div>这里是购物车<input type="text" placeholder="cart"></div>
</template>
<script>
  export default {
    mounted () {
      console.log('cart mounted')
    },
    activated () {
      console.log('cart activated')
    },
    deactivated () {
      console.log('cart deactivated')
    },
    unmounted () {
      console.log('cart unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/User.vue -->
<template>
  <div>这里是个人中心<input type="text" placeholder="user"></div>
</template>
<script>
  export default {
    mounted () {
      console.log('user mounted')
    },
    activated () {
      console.log('user activated')
    },
    deactivated () {
      console.log('user deactivated')
    },
    unmounted () {
      console.log('user unmounted')
    },
  }
</script>
```

> 要不都不缓存，要不就都缓存，不符合实际的需求

## 1.4 选择缓存组件

组件内添加name属性

```vue
	<!-- src/components/Home.vue -->
<template>
  <div>
    这里是首页
    <input type="text" placeholder="home">
  </div>
</template>

<script>
  export default {
    name: 'Home',
    mounted () {
      console.log('home mounted')
    },
    activated () {
      console.log('home activated')
    },
    deactivated () {
      console.log('home deactivated')
    },
    unmounted () {
      console.log('home unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/Kind.vue -->
<template>
  <div>这里分类 <input type="text" placeholder="kind"></div>
</template>
<script>
  export default {
    name: 'Kind',
    mounted () {
      console.log('kind mounted')
    },
    activated () {
      console.log('kind activated')
    },
    deactivated () {
      console.log('kind deactivated')
    },
    unmounted () {
      console.log('kind unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/Cart.vue -->
<template>
  <div>这里是购物车<input type="text" placeholder="cart"></div>
</template>
<script>
  export default {
    name: 'Cart',
    mounted () {
      console.log('cart mounted')
    },
    activated () {
      console.log('cart activated')
    },
    deactivated () {
      console.log('cart deactivated')
    },
    unmounted () {
      console.log('cart unmounted')
    },
  }
</script>
```

```vue
<!-- src/components/User.vue -->
<template>
  <div>这里是个人中心<input type="text" placeholder="user"></div>
</template>
<script>
  export default {
    name: 'User',
    mounted () {
      console.log('user mounted')
    },
    activated () {
      console.log('user activated')
    },
    deactivated () {
      console.log('user deactivated')
    },
    unmounted () {
      console.log('user unmounted')
    },
  }
</script>
```

```vue
<!-- src/App.vue -->
<template>
  <div>
    <el-button type="primary" @click="com='Home'">首页</el-button>
    <el-button type="primary" @click="com='Kind'">分类</el-button>
    <el-button type="primary" @click="com='Cart'">购物车</el-button>
    <el-button type="primary" @click="com='User'">我的</el-button>

    <!-- 动态组件 -->
    <!-- include 缓存哪些组件 -->
    <!-- exclude 不缓存哪些组件 -->
    <!-- max 缓存组件数量最大数 -->
    <!-- <KeepAlive include="Home,Kind"> -->
    <!-- <KeepAlive :include="/Home|Kind/"> -->
    <KeepAlive :include="['Home', 'Kind']">
      <component :is="com"></component>
    </KeepAlive>
  </div>
</template>

<script>
  import Home from './components/Home.vue'
  import Kind from './components/Kind.vue'
  import Cart from './components/Cart.vue'
  import User from './components/User.vue'
  export default {
    components: {
      Home, Kind, Cart, User
    },
    data () {
      return {
        com: 'Home'
      }
    }
  }
</script>
```

# 2.过渡

建议使用vue-cli 以及vite创建的脚手架

```vue
<!-- src/App.vue -->
<template>
  <div>
    <!-- 过渡效果 -->
    <button @click="show=!show">显示/隐藏</button>
    <Transition>
      <div v-if="show">过渡动画测试</div>
    </Transition>

    <Transition name="fade">
      <div v-if="show">过渡动画测试</div>
    </Transition>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        show: false
      }
    }
  }
</script>

<style>
.v-enter-from { opacity: 0; }
.v-enter-active { transition: opacity 5s; }
.v-enter-to { opacity: 1;}

.v-leave-from { opacity: 1;}
.v-leave-active { transition: opacity 5s; }
.v-leave-to { opacity: 0;}

.fade-enter-from { opacity: 0; }
.fade-enter-active { transition: opacity 5s; }
.fade-enter-to { opacity: 1;}

.fade-leave-from { opacity: 1;}
.fade-leave-active { transition: opacity 5s; }
.fade-leave-to { opacity: 0;}
</style>
```

# 3.服务端渲染

vue：nuxt.js https://www.nuxtjs.cn/  vue2       vue3https://nuxt.com/

react: next.js https://www.nextjs.cn/

什么是服务端渲染？https://cn.vuejs.org/guide/scaling-up/ssr.html#server-side-rendering-ssr

这主要取决于首屏加载速度对应用的重要程度

在内容展示速度极其重要的场景下，SSR 可以尽可能地帮你实现最优的初始加载性能。

SSR： **服务端渲染**

SSG： **静态站点生成**

## 3.1 nuxt.js

使用nuxt2版本

### 3.1.1 创建项目

```sh
$ yarn create nuxt-app my-nuxt-app
$ npx create-nuxt-app my-nuxt-app
```

### 3.1.2 熟悉目录结构

* components 组件
* layouts 布局文件，可以写多个，页面可以控制显示哪一个（后期自行添加的，名字不可以更改）
* pages 页面路径，根据页面的命名可以自动生成了路由表
* plugins 插件，可以配置多个UI组件库

### 3.1.3 创建基本的页面

* pages/index.vue 系统首页   / 
* pages/banner/list.vue 轮播图列表  /banner/list
* pages/banner/add.vue 添加轮播图 / banner/add
* pages/pro/list.vue 产品列表  /pro/list
* pages/pro/search.vue 筛选列表 /pro/search
* pages/login/index.vue 登录页面 /login    /login/index
* pages/user/_id.vue 用户页面 - 动态路由 /login/:id

### 3.1.4 设置主布局页面

layouts/default.vue

```vue
<template>
  <el-container style="height: 500px; border: 1px solid #eee">
    <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
      <el-menu :default-active="$route.path" :router="true" unique-opened	>
        <el-menu-item index="/" >系统首页</el-menu-item>
        <el-submenu index="/banner">
          <template slot="title"><i class="el-icon-message"></i>轮播图管理</template>
          <el-menu-item-group>
            <el-menu-item index="/banner/list">轮播图列表</el-menu-item>
            <el-menu-item index="/banner/add">添加轮播图</el-menu-item>
          </el-menu-item-group>
          
        </el-submenu>
        <el-submenu index="/pro">
          <template slot="title"><i class="el-icon-message"></i>产品管理</template>
          <el-menu-item-group>
            <el-menu-item index="/pro/list">产品列表</el-menu-item>
            <el-menu-item index="/pro/search">筛选列表</el-menu-item>
          </el-menu-item-group>
          
        </el-submenu>
      </el-menu>
    </el-aside>

    <el-container>
      <el-header style="text-align: right; font-size: 12px">
        <el-dropdown>
          <i class="el-icon-setting" style="margin-right: 15px"></i>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item >
              <nuxt-link to="/login">退出</nuxt-link>
            </el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
        <span>王小虎</span>
      </el-header>

      <el-main>
        <!-- 路由 -->
        <nuxt />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    const item = {
      date: '2016-05-02',
      name: '王小虎',
      address: '上海市普陀区金沙江路 1518 弄'
    };
    return {
      tableData: Array(20).fill(item)
    }
  }
}
</script>

<style>
.el-header {
  background-color: #B3C0D1;
  color: #333;
  line-height: 60px;
}

.el-aside {
  color: #333;
}
</style>
```

### 3.1.5 设置其他布局页面

layouts/other.vue

```vue
<template>
  <nuxt />
</template>
```

### 3.1.6 登录使用其他布局

pages/login/index.vue

```vue
<template>
  <div>登录页面</div>
</template>

<script>
export default {
  layout: 'other'
}
</script>
```

### 3.1.7 如何使用其他UI组件库

```sh
$ cnpm i ant-design-vue@1 -S
```

```js
// plugins/ant-design-vue.js
import Vue from 'vue';
import Antd from 'ant-design-vue';

Vue.use(Antd);
```

```js
// nuxt.config.js
export default {
  // Global page headers: https://go.nuxtjs.dev/config-head
  head: {
    title: 'my-nuxt-app',
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: '' },
      { name: 'format-detection', content: 'telephone=no' }
    ],
    link: [
      { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
    ]
  },

  // Global CSS: https://go.nuxtjs.dev/config-css
  css: [
    'element-ui/lib/theme-chalk/index.css',
    'ant-design-vue/dist/antd.css' // ++++++++
  ],

  // Plugins to run before rendering page: https://go.nuxtjs.dev/config-plugins
  plugins: [
    '@/plugins/element-ui',
    '@/plugins/ant-design-vue' // ++++++++
  ],

  // Auto import components: https://go.nuxtjs.dev/config-components
  components: true,

  // Modules for dev and build (recommended): https://go.nuxtjs.dev/config-modules
  buildModules: [
    // https://go.nuxtjs.dev/eslint
    '@nuxtjs/eslint-module',
    // https://go.nuxtjs.dev/stylelint
    // '@nuxtjs/stylelint-module',
  ],

  // Modules: https://go.nuxtjs.dev/config-modules
  modules: [
    // https://go.nuxtjs.dev/axios
    '@nuxtjs/axios',
    // https://go.nuxtjs.dev/pwa
    '@nuxtjs/pwa',
    // https://go.nuxtjs.dev/content
    '@nuxt/content',
  ],

  // Axios module configuration: https://go.nuxtjs.dev/config-axios
  axios: {
    // Workaround to avoid enforcing hard-coded localhost:3000: https://github.com/nuxt-community/axios-module/issues/308
    baseURL: '/',
  },

  // PWA module configuration: https://go.nuxtjs.dev/pwa
  pwa: {
    manifest: {
      lang: 'en'
    }
  },

  // Content module configuration: https://go.nuxtjs.dev/config-content
  content: {},

  // Build Configuration: https://go.nuxtjs.dev/config-build
  build: {
    transpile: [/^element-ui/],
  }
}

```

## 3.2 next.js

### 3.2.1 创建项目

```sh
$ npx create-next-app nextjs-blog
```

### 3.2.2 熟悉目录结构

* components/layout.js 主界面布局  自己定义
* components/other.js 其他界面布局 自己定义
* pages/index.js系统首页   / 

```js
import Head from 'next/head'
import Layout from '../components/layout'

export default function Test() {
  return (
    <Layout>
      <Head>
        <title>系统首页</title>
      </Head>
      <h1>系统首页</h1>
    </Layout>
  )
}

```

* pages/banner/list.js轮播图列表  /banner/list

* pages/banner/add.js添加轮播图 / banner/add

* pages/pro/list.js 产品列表  /pro/list

* pages/pro/search.js筛选列表 /pro/search

* pages/login/index.js登录页面 /login    /login/index

  ```js
  // import Head from 'next/head'
  import OtherLayout from '../../components/other'
  
  export default function Test() {
    return (
      <OtherLayout>
        <h1>登录</h1>
      </OtherLayout>
    )
  }
  
  ```

  ```js
  // 声明式跳转
  import Link from 'next/link'
  <Link href=""></Link>
  // 编程式跳转
  import { useRouter } from 'next/router'
  const router = useRouter()
  router.push() router.replace() router.back()
  ```

  
