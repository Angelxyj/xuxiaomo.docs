

# React项目说明

https://v3.umijs.org/zh-CN/docs/getting-started

## 一.通过脚手架创建项目

>cnpm i yarn tyarn -g
>
>mkdir  umi-app
>
>cd umi-app
>
>yarn create @umijs/umi-app
>
>> npx @umijs/create-umi-app
>
>yarn start
>
>如果运行时报错 ·`cannot find module 'umi'`,那么执行 `yarn add umi@3`

## 二.熟悉目录和文件

1.mock 

* http://rap2.taobao.org/

 - 模拟数据 http://mockjs.com/

   > 生成随机数据，拦截 Ajax 请求
   >
   > *  前后端分离
   >
   >   让前端攻城师独立于后端进行开发
   >
   >
   > * 开发无侵入
   >
   >   不需要修改既有代码，就可以拦截 Ajax 请求，返回模拟的响应数据
   >
   > * 数据类型丰富
   >
   >   支持生成随机的文本、数字、布尔值、日期、邮箱、链接、图片、颜色等。
   >
   > * 增加单元测试的真实性
   >
   >   通过随机数据，模拟各种场景。
   >
   > * 用法简单
   >
   >   符合直觉的接口。
   >
   > * 方便扩展
   >
   >   支持扩展更多数据类型，支持自定义函数和正则

2.src

  - pages 页面 --- 等同于vue脚手架中views，存放的就是页面组件
      - index.less
      - index.tsx 注意引入的less是局部生效的，使用`:global`放成全局的

3.editorconfig 编辑器配置

4.gitignore git上传的忽略文件

5.prettierignore 格式化忽略配置文件

6.prettierrc  格式化配置文件

7.umirc.ts 项目配置文件，类似vue的vue.config.js,老版本umi中config/config.js

8.package.json 项目记录文件

9.README.md 项目说明书

10.tsconfig.json  ts的配置文件

11.typings.d.ts   ts的声明文件，不声明不可以引入

> 如果代码提示：无法使用 JSX，除非提供了 "--jsx" 标志。修改tsconfig.json文件中的 
>
> "jsx": "react-jsx",   为 ‘"jsx": "react"

## 三.搭建项目基本结构

查看umi提供的umi插件

https://v3.umijs.org/zh-CN/plugins/plugin-layout

### 1.构建时配置

可以通过配置文件配置 `layout` 的主题等配置, 在 [`config/config.ts`](https://github.com/ant-design/ant-design-pro/blob/4a2cb720bfcdab34f2b41a3b629683329c783690/config/config.ts#L15)  或者 `umirc.ts` 中这样写：

```tsx
import { defineConfig } from 'umi';

export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes: [
    { path: '/', component: '@/pages/index' },
  ],
  layout: {
    name: '喜购后台管理系统',
    logo: 'https://images.innov100.com/companyicon/CgpEMllHpbyAcEt6AAAxhVZESNY624.png'
  },
  fastRefresh: {},
});

```



### 2.运行时配置

https://umijs.org/zh-CN/plugins/plugin-layout#%E8%BF%90%E8%A1%8C%E6%97%B6%E9%85%8D%E7%BD%AE

src文件夹下创建 app.tsx

```tsx
// src/app.tsx
import React from 'react';
import {
  BasicLayoutProps,
  Settings as LayoutSettings,
} from '@ant-design/pro-layout';

export const layout = ({
  initialState,
}: {
  initialState: { settings?: LayoutSettings; };
}): BasicLayoutProps => {
  return {
    rightContentRender: () => <div>header</div>,
    footerRender: () => <footer >footer</footer>,
    menuHeaderRender: undefined,
    ...initialState?.settings,
  };
};
```



### 3.扩展路由配置

config/route.ts

```sh
$ npx umi g page home/Index --typescript --less
$ npx umi g page login/Index --typescript --less
$ npx umi g page banner/List --typescript --less
$ npx umi g page banner/Add --typescript --less
$ npx umi g page pro/List --typescript --less
$ npx umi g page pro/Search --typescript --less
$ npx umi g page account/User --typescript --less
$ npx umi g page account/Admin --typescript --less
```



// config/route.ts

```ts
// config/route.ts
export const routes = [
  {
    path: '/',
    redirect: '/home',
    exact: true
  },
  {
    path: '/login',
    // name: '登录', // name 不写即可不出现在左侧菜单栏
    icon: 'HomeOutlined',
    component: '@/pages/login/Index',
    // 不展示顶栏
    headerRender: false,
    // 不展示页脚
    footerRender: false,
    // 不展示菜单
    menuRender: false,
  },
  {
    path: '/home',
    name: '系统首页',
    icon: 'HomeOutlined',
    component: '@/pages/home/Index'
  },
  {
    path: '/banner',
    name: '轮播图管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/banner/list',
        name: '轮播图列表',
        component: '@/pages/banner/List'
      },
      {
        path: '/banner/add',
        // name: '添加轮播图',
        component: '@/pages/banner/Add'
      },
    ]
  },
  {
    path: '/pro',
    name: '产品管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/pro/list',
        name: '产品列表',
        component: '@/pages/pro/List'
      },
      {
        path: '/pro/search',
        name: '筛选列表',
        component: '@/pages/pro/Search'
      },
    ]
  },
  {
    path: '/account',
    name: '账户管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/account/user',
        name: '用户列表',
        component: '@/pages/account/User'
      },
      {
        path: '/account/admin',
        name: '管理员列表',
        component: '@/pages/account/Admin'
      },
    ]
  }
]
```

umirc.ts中配置路由

```js
import { defineConfig } from 'umi';
import { routes } from './config/route'
export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  // routes: [
  //   { path: '/', component: '@/pages/index' },
  // ],
  routes,
  fastRefresh: {},
  layout:{
    name: '嗨购管理系统',
    logo: 'https://pinia.vuejs.org/logo.svg'
  }
});

```

src/global.css

https://v3.umijs.org/zh-CN/docs/assets-css

```
html, body, #root {
  height: 100%;
}
```

> 发现并没有如文档中所说生效，将其通过app.tsx引入到最前面即可

src/app.tsx

```ts
// src/app.tsx
import React from 'react';
import './global.less';
import {
  BasicLayoutProps,
  Settings as LayoutSettings,
} from '@ant-design/pro-layout';

export const layout = ({
  initialState,
}: {
  initialState: { settings?: LayoutSettings; };
}): BasicLayoutProps => {
  return {
    rightContentRender: () => <div>header</div>,
    footerRender: () => <footer >footer</footer>,
    menuHeaderRender: undefined,
    ...initialState?.settings,
  };
};
```



## 四.登录页面实现

使用ant-design-pro

```
cnpm i @ant-design/pro-components @ant-design/pro-form -S
```

https://procomponents.ant.design/components/login-form#packages-form-src-components-login-form-demo-login-form-page

移动端接口地址：http://121.89.205.189:3000/apidoc/

pc后台管理系统：http://121.89.205.189:3000/admindoc/

https://procomponents.ant.design/components/login-form#packages-form-src-components-login-form-demo-login-form-page

```tsx
// src/pages/login/Index.tsx
import {
  AlipayOutlined,
  LockOutlined,
  MobileOutlined,
  TaobaoOutlined,
  UserOutlined,
  WeiboOutlined,
} from '@ant-design/icons';
import {
  LoginFormPage,
  ProFormCaptcha,
  ProFormCheckbox,
  ProFormText,
} from '@ant-design/pro-components';
import { Button, Divider, message, Space, Tabs } from 'antd';
import type { CSSProperties } from 'react';
import { useState } from 'react';

type LoginType = 'phone' | 'account';

const iconStyles: CSSProperties = {
  color: 'rgba(0, 0, 0, 0.2)',
  fontSize: '18px',
  verticalAlign: 'middle',
  cursor: 'pointer',
};

export default () => {
  const [loginType, setLoginType] = useState<LoginType>('account');
  return (
    <div style={{ backgroundColor: 'white', height: 'calc(100vh - 48px)', margin: -24 }}>
      <LoginFormPage
        backgroundImageUrl="https://gw.alipayobjects.com/zos/rmsportal/FfdJeJRQWjEeGTpqgBKj.png"
        logo="https://github.githubassets.com/images/modules/logos_page/Octocat.png"
        title="喜购"
        subTitle="后台管理系统"
        activityConfig={{
          style: {
            boxShadow: '0px 0px 8px rgba(0, 0, 0, 0.2)',
            color: '#fff',
            borderRadius: 8,
            backgroundColor: '#1677FF',
          },
          title: '活动标题，可配置图片',
          subTitle: '活动介绍说明文字',
          action: (
            <Button
              size="large"
              style={{
                borderRadius: 20,
                background: '#fff',
                color: '#1677FF',
                width: 120,
              }}
            >
              去看看
            </Button>
          ),
        }}
        actions={
          <div
            style={{
              display: 'flex',
              justifyContent: 'center',
              alignItems: 'center',
              flexDirection: 'column',
            }}
          >
            {/* <Divider plain>
              <span style={{ color: '#CCC', fontWeight: 'normal', fontSize: 14 }}>
                其他登录方式
              </span>
            </Divider>
            <Space align="center" size={24}>
              <div
                style={{
                  display: 'flex',
                  justifyContent: 'center',
                  alignItems: 'center',
                  flexDirection: 'column',
                  height: 40,
                  width: 40,
                  border: '1px solid #D4D8DD',
                  borderRadius: '50%',
                }}
              >
                <AlipayOutlined style={{ ...iconStyles, color: '#1677FF' }} />
              </div>
              <div
                style={{
                  display: 'flex',
                  justifyContent: 'center',
                  alignItems: 'center',
                  flexDirection: 'column',
                  height: 40,
                  width: 40,
                  border: '1px solid #D4D8DD',
                  borderRadius: '50%',
                }}
              >
                <TaobaoOutlined style={{ ...iconStyles, color: '#FF6A10' }} />
              </div>
              <div
                style={{
                  display: 'flex',
                  justifyContent: 'center',
                  alignItems: 'center',
                  flexDirection: 'column',
                  height: 40,
                  width: 40,
                  border: '1px solid #D4D8DD',
                  borderRadius: '50%',
                }}
              >
                <WeiboOutlined style={{ ...iconStyles, color: '#333333' }} />
              </div>
            </Space> */}
          </div>
        }
      >
        <Tabs
          centered
          activeKey={loginType}
          onChange={(activeKey) => setLoginType(activeKey as LoginType)}
        >
          <Tabs.TabPane key={'account'} tab={'账号密码登录'} />
          <Tabs.TabPane key={'phone'} tab={'手机号登录'} />
        </Tabs>
        {loginType === 'account' && (
          <>
            <ProFormText
              name="adminname"
              fieldProps={{
                size: 'large',
                prefix: <UserOutlined className={'prefixIcon'} />,
              }}
              placeholder={'请输入管理员账户'}
              rules={[
                {
                  required: true,
                  message: '请输入管理员账户!',
                },
              ]}
            />
            <ProFormText.Password
              name="password"
              fieldProps={{
                size: 'large',
                prefix: <LockOutlined className={'prefixIcon'} />,
              }}
              placeholder={'请输入密码'}
              rules={[
                {
                  required: true,
                  message: '请输入密码！',
                },
              ]}
            />
          </>
        )}
        {loginType === 'phone' && (
          <>
            <ProFormText
              fieldProps={{
                size: 'large',
                prefix: <MobileOutlined className={'prefixIcon'} />,
              }}
              name="mobile"
              placeholder={'手机号'}
              rules={[
                {
                  required: true,
                  message: '请输入手机号！',
                },
                {
                  pattern: /^1\d{10}$/,
                  message: '手机号格式错误！',
                },
              ]}
            />
            <ProFormCaptcha
              fieldProps={{
                size: 'large',
                prefix: <LockOutlined className={'prefixIcon'} />,
              }}
              captchaProps={{
                size: 'large',
              }}
              placeholder={'请输入验证码'}
              captchaTextRender={(timing, count) => {
                if (timing) {
                  return `${count} ${'获取验证码'}`;
                }
                return '获取验证码';
              }}
              name="captcha"
              rules={[
                {
                  required: true,
                  message: '请输入验证码！',
                },
              ]}
              onGetCaptcha={async () => {
                message.success('获取验证码成功！验证码为：1234');
              }}
            />
          </>
        )}
        {/* <div
          style={{
            marginBlockEnd: 24,
          }}
        >
          <ProFormCheckbox noStyle name="autoLogin">
            自动登录
          </ProFormCheckbox>
          <a
            style={{
              float: 'right',
            }}
          >
            忘记密码
          </a>
        </div> */}
      </LoginFormPage>
    </div>
  );
};
```

## 五.mock数据

| 序号 | 接口地址           | 接口参数                      | 返回结果                                                     |
| ---- | ------------------ | ----------------------------- | ------------------------------------------------------------ |
| 1    | /admin/admin/login | {adminname: '', password: ''} | {        code: '10005',message: '该账户未注册' }   {        code: '10003',message: '密码错误' }   {        code: '200',message: '登录成功', data: { adminname: '', token: ''} } |
|      |                    |                               |                                                              |
|      |                    |                               |                                                              |

https://v3.umijs.org/zh-CN/docs/mock

### 5.1 开启mock

```ts
// .umirc.ts
import { defineConfig } from 'umi';
import { routes } from './config/route'
export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes,
  // routes: [
  //   { path: '/', component: '@/pages/index' },
  // ],
  layout: {
    name: '喜购后台管理系统',
    logo: 'https://images.innov100.com/companyicon/CgpEMllHpbyAcEt6AAAxhVZESNY624.png'
  },
  fastRefresh: {},
  mock: {}
});

```

### 5.2 设置mock数据

#### 5.2.1 设置登录接口

```ts
// mock/admin.ts

export default {
  // 'GET /api/users': { users: [1, 2] },
  'POST /admin/admin/login': (req: any, res: any) => {
    // 添加跨域请求头 - 无跨域
    res.setHeader('Access-Control-Allow-Origin', '*');
    const { adminname, password } = req.body 
    // 相当于数据中的数据
    const list = [ 
      { adminname: 'admin', password: '123456'},
      { adminname: 'editor', password: '654321'}
    ]
    const index = list.findIndex(item => {
      return item.adminname === adminname
    })
    if (index === -1) {
      res.json({
        code: '10005',
        message: '该账户未注册'
      })
    } else {
      if (list[index].password === password) {
        res.json({
          code: '200',
          message: '登录成功',
          data: {
            adminname,
            token: 'ty2206'
          }
        })
      } else {
        res.json({
          code: '10003',
          message: '密码错误'
        })
      }
    }

  }
}
```

#### 5.2.2 设置管理员列表接口

```sh
$ yarn add mockjs @types/mockjs -S
```

```ts
// mock/admin.ts
import Mock from 'mockjs';
export default {
  // 'GET /api/users': { users: [1, 2] },
  'POST /admin/admin/login': (req: any, res: any) => {
    // 添加跨域请求头 - 无跨域
    res.setHeader('Access-Control-Allow-Origin', '*');
    const { adminname, password } = req.body 
    // 相当于数据中的数据
    const list = [ 
      { adminname: 'admin', password: '123456'},
      { adminname: 'editor', password: '654321'}
    ]
    const index = list.findIndex(item => {
      return item.adminname === adminname
    })
    if (index === -1) {
      res.json({
        code: '10005',
        message: '该账户未注册'
      })
    } else {
      if (list[index].password === password) {
        res.json({
          code: '200',
          message: '登录成功',
          data: {
            adminname,
            token: 'ty2206'
          }
        })
      } else {
        res.json({
          code: '10003',
          message: '密码错误'
        })
      }
    }

  },
  'GET /admin/admin/list': (req: any, res: any) => {

    const token = req.headers.token || req.body.token || req.query.token

    if (token) {
      if (token !== 'ty2206') {
        res.json({
          code: '10119',
          message: 'token已失效'
        })
       } else {
        const data = []
        for (var i = 0; i < 200; i++) {
          data.push({
            adminid: 'admin' + (i + 1),
            adminname: Mock.mock('@cname()'),
            password: Mock.mock('@word(5, 8)')
          })
        }

        res.json({
          code: '200',
          message: '管理员列表',
          data
        })
       }
    } else {
      res.json({
        code: '10119',
        message: '请传递token'
      })
    }
    
  }
}
```

#### 5.2.3 添加接口

```ts
// mock/admin.ts
import Mock from 'mockjs';
export default {
  // 'GET /api/users': { users: [1, 2] },
  'POST /admin/admin/login': (req: any, res: any) => {
    // 添加跨域请求头 - 无跨域
    res.setHeader('Access-Control-Allow-Origin', '*');
    const { adminname, password } = req.body 
    // 相当于数据中的数据
    const list = [ 
      { adminname: 'admin', password: '123456'},
      { adminname: 'editor', password: '654321'}
    ]
    const index = list.findIndex(item => {
      return item.adminname === adminname
    })
    if (index === -1) {
      res.json({
        code: '10005',
        message: '该账户未注册'
      })
    } else {
      if (list[index].password === password) {
        res.json({
          code: '200',
          message: '登录成功',
          data: {
            adminname,
            token: 'ty2206'
          }
        })
      } else {
        res.json({
          code: '10003',
          message: '密码错误'
        })
      }
    }

  },
  'GET /admin/admin/list': (req: any, res: any) => {

    const token = req.headers.token || req.body.token || req.query.token

    if (token) {
      if (token !== 'ty2206') {
        res.json({
          code: '10119',
          message: 'token已失效'
        })
       } else {
        const data = []
        for (var i = 0; i < 200; i++) {
          data.push({
            adminid: 'admin' + (i + 1),
            adminname: Mock.mock('@cname()'),
            password: Mock.mock('@word(5, 8)')
          })
        }

        res.json({
          code: '200',
          message: '管理员列表',
          data
        })
       }
    } else {
      res.json({
        code: '10119',
        message: '请传递token'
      })
    }
    
  },
  'POST /admin/admin/add': (req: any, res: any) => {
    const token = req.headers.token || req.body.token || req.query.token
    const { adminname, password } = req.body
    if (token) {
      if (token !== 'ty2206') {
        res.json({
          code: '10119',
          message: 'token已失效'
        })
       } else {
         const data = {
            adminid: 'admin' + new Date().getTime(),
            adminname,
            password
         }

         res.json({
          code: '200',
          message: '添加成功',
          data // 真实接口一般不返回添加的数据
        })
       }
    } else {
      res.json({
        code: '10119',
        message: '请传递token'
      })
    }
  }
}
```



## 六.封装axios

> umi中必须使用axios请求数据吗？
>
> 不是，可以使用umi的插件： https://umijs.org/zh-CN/plugins/plugin-request

> cnpm i axios store2  -S

// utils/request.ts

```ts
// src/utils/request.ts
import { message } from 'antd'
import axios, { AxiosRequestConfig } from 'axios'
import store2 from 'store2'

const isDev = process.env.NODE_ENV === 'development'

const instance = axios.create({
  baseURL: isDev ? '/admin' : 'http://121.89.205.189:3000/admin',
  timeout: 60000
})

// 请求拦截器封装
instance.interceptors.request.use((config) => {
  // 可以从本地获取用户的信息，如果用户的信息存在，提取Token,通过头信息传递给服务器

  // 传递token信息
  config.headers!.token = store2.get('token')

  return config
}, (error) => Promise.reject(error))

// 响应拦截器封装
instance.interceptors.response.use((response: any) => {
  // 验证token
  if (response.data.code === '10119') {
    // token 无效
    message.error('登录失效，请重新登录');

    // 防止后期系统首页以及登录来回切换
    store2.remove('token')
    store2.remove('adminname')

    // 跳转到登录页面 --- 如果使用的是 HashRouter ,此处地址需要改为 '/#/login'
    // router.push('/login')
    window.location.href = '/login'

    // 返回
    return
  }

  // 登录成功 放行
  return response
}, (error) => Promise.reject(error))

// 自定义各种常用的restful api的请求
// axios.get('url', { params: { key: value } })
// axios.post('url', { key: value })
// axios({ url: '', method: 'GET', params: { key: value }})
// axios({ url: '', method: 'POST', data: { key: value }})
export default function request( config: AxiosRequestConfig ) {
  // 接口请求 必须参数  url method  data  headers
  const { url = '', method = 'GET', data = {}, headers = {} } = config

  // 区分不同的数据请求 为了执行时传入的数据请求方式统一性 GEt GeT get GET
  switch (method.toUpperCase()) {
    case 'GET':
      return instance.get(url, { params: data })

    case 'POST': 
      // 可能数据请求方式 表单提交  文件提交   默认json
      // 表单提交
      if (headers['content-type'] === 'application/x-www-form-url-encoded') {
        // 转换参数  URLSearchParams  / 第三方库 qs
        const p = new URLSearchParams()
        for (const key in data) {
          p.append(key, data[key])
        }
        return instance.post(url, p, { headers })
      }

      // 文件提交
      if (headers['content-type'] === 'multipart/form-data') {
        const p = new FormData()
        for (const key in data) {
          p.append(key, data[key])
        }
        return instance.post(url, p, { headers })
      }

      // 默认 application/json
      return instance.post(url, data)
    
    // 修改数据 - 所有的数据的更新
    case 'PUT':
      return instance.put(url, data)

    // 删除数据
    case 'DELETE': 
      return instance.delete(url, { data })  

    // 修改数据 - 部分的数据的更新
    case 'PATCH':
      return instance.patch(url, data)

    default:
      return instance(config)
  }
}
```

封装数据请求 src/api/admin.ts

```ts
// src/api/admin.ts

import request from '@/utils/request'
export interface IAdminParams {
  adminname: string
  password: string
}

export interface IAdminAddParams {
  adminname: string
  password: string
}

// 登录
export function adminLoginFn (params: IAdminParams) {
  return request({
    url: '/admin/login',
    method: 'POST',
    data: params
  })
}

export function getAdminList () {
  return request({
    url: '/admin/list'
  })
}



export function addAdmin (params: IAdminAddParams) {
  return request({
    url: '/admin/add',
    data: params,
    method: 'POST'
  })
}


```

## 七.dva数据流 - 状态管理器

https://v3.umijs.org/zh-CN/plugins/plugin-dva

- **内置 dva**，默认版本是 `^2.6.0-beta.20`，如果项目中有依赖，会优先使用项目中依赖的版本。
- **约定式的 model 组织方式**，不用手动注册 model
- **文件名即 namespace**，model 内如果没有声明 namespace，会以文件名作为 namespace
- **内置 dva-loading**，直接 connect `loading` 字段使用即可
- **支持 immer**，通过配置 `immer` 开启



约定式的 model 组织方式

符合以下规则的文件会被认为是 model 文件，

- `src/models` 下的文件
- `src/pages` 下，子目录中 models 目录下的文件
- `src/pages` 下，所有 model.ts 文件(不区分任何字母大小写)

### 1.配置

https://v3.umijs.org/zh-CN/plugins/plugin-dva#%E9%85%8D%E7%BD%AE

// umirc.ts

```ts
// .umirc.ts
import { defineConfig } from 'umi';
import { routes } from './config/route'
export default defineConfig({
  nodeModulesTransform: {
    type: 'none',
  },
  routes,
  // routes: [
  //   { path: '/', component: '@/pages/index' },
  // ],
  layout: {
    name: '喜购后台管理系统',
    logo: 'https://images.innov100.com/companyicon/CgpEMllHpbyAcEt6AAAxhVZESNY624.png'
  },
  fastRefresh: {},
  mock: {},
  dva: {
    immer: true,
    hmr: false,
  },
});

```

### 2.dva数据流引入
约定式的 model 组织方式

符合以下规则的文件会被认为是 model 文件，

- `src/models` 下的文件 - 常用  -- 全局性的redux数据
- `src/pages` 下，子目录中 models 目录下的文件
- `src/pages` 下，所有 model.ts 文件(不区分任何字母大小写) - 常用 - 表示某个模块下的redux数据



### 3.配置登录模块dva

使用第三种使用dva的方式

src/pages/login/model.ts

```ts
// src/pages/login/model.ts
// import { adminLoginFn } from '@/api/admin';
import { Effect, ImmerReducer } from 'umi';

export interface AdminModelState {
  adminname: string;
  token: string;
}

export interface AdminModelType {
  namespace: 'admin';
  state: AdminModelState;
  effects: {
    login: Effect;
  };
  reducers: {
    // save: Reducer<AdminModelState>;
    // 启用 immer 之后
    changeAdminname: ImmerReducer<AdminModelState>;
    changeToken: ImmerReducer<AdminModelState>;
  };
}

const IndexModel: AdminModelType = {
  namespace: 'admin',

  // 初始化状态
  state: {
    adminname: '',
    token: '',
  },

  // 异步操作
  effects: {
    *login({ payload }, { call, put }) {
      // const res = yield call(adminLoginFn, payload)
    },
  },

  // 修改状态
  reducers: {
    // 启用 immer 之后
    changeAdminname(state, action) {
      state.adminname = action.payload;
    },
    changeToken(state, action) {
      state.token = action.payload;
    },
  }
};

export default IndexModel;
```

```jsx
// src/pages/login/index.tsx
import {
  LockOutlined,
  UserOutlined,
} from '@ant-design/icons';
import {
  LoginForm,
  ProFormText,
} from '@ant-design/pro-components';
import { Tabs } from 'antd';
import { useState } from 'react';
import { connect, Dispatch, IndexModelState } from 'umi'
type LoginType = 'phone' | 'account';

export default connect(({ admin }: { admin: IndexModelState}) => {
  return {

  }
})(({ dispatch }: { dispatch: Dispatch}) => {
  const [loginType, setLoginType] = useState<LoginType>('account');
  return (
    <div style={{ marginTop: '100px'}}>
      <LoginForm
        logo="https://pinia.vuejs.org/logo.svg"
        title="嗨购管理系统"
        subTitle="电子商务平台"
        onFinish={ async (values) => {
          // console.log(values)
          // console.log(1)
          dispatch({
            type: 'admin/loginFn',
            payload: values
          })
        }}
      >
        <Tabs
          centered
          activeKey={loginType}
          onChange={(activeKey) => setLoginType(activeKey as LoginType)}
        >
          <Tabs.TabPane key={'account'} tab={'账号密码登录'} />
          {/* <Tabs.TabPane key={'phone'} tab={'手机号登录'} /> */}
        </Tabs>
        {loginType === 'account' && (
          <>
            <ProFormText
              name="adminname"
              fieldProps={{
                size: 'large',
                prefix: <UserOutlined className={'prefixIcon'} />,
              }}
              placeholder={'用户名'}
              rules={[
                {
                  required: true,
                  message: '请输入用户名!',
                },
              ]}
            />
            <ProFormText.Password
              name="password"
              fieldProps={{
                size: 'large',
                prefix: <LockOutlined className={'prefixIcon'} />,
              }}
              placeholder={'密码'}
              rules={[
                {
                  required: true,
                  message: '请输入密码！',
                },
              ]}
            />
          </>
        )}
        
        <div
          style={{
            marginBlockEnd: 24,
          }}
        >
        </div>
    </LoginForm>
    </div>
  );
});
```

```ts
// src/pages/login/model.ts
import store2 from 'store2'
import { adminLoginFn } from '@/api/admin';
import { Effect, history, ImmerReducer } from 'umi';
import { message } from 'antd'
export interface AdminModelState {
  adminname: string;
  token: string;
}

export interface AdminModelType {
  namespace: 'admin';
  state: AdminModelState;
  effects: {
    login: Effect;
  };
  reducers: {
    // save: Reducer<AdminModelState>;
    // 启用 immer 之后
    changeAdminname: ImmerReducer<AdminModelState>;
    changeToken: ImmerReducer<AdminModelState>;
  };
}

const IndexModel: AdminModelType = {
  namespace: 'admin',

  // 初始化状态
  state: {
    adminname: store2.get('adminname') || '',
    token: store2.get('token') || '',
  },

  // 异步操作
  effects: {
    *login({ payload }, { call, put }): any {
      console.log('666', payload)
      const res = yield call(adminLoginFn, payload)
      console.log('777', res.data)
      if (res.data.code === '10005') {
        message.error('账户名未注册')
      } else if (res.data.code === '10003') {
        message.error('密码错误')
      } else {
        message.success('登录成功')
        store2.set('adminname', res.data.data.adminname)
        store2.set('token', res.data.data.token)

        // 修改状态管理器中的状态
        yield put({
          type: 'changeAdminname',
          payload: res.data.data.adminname
        })
        yield put({
          type: 'changeToken',
          payload: res.data.data.token
        })
        history.push('/')
      }
    },
  },

  // 修改状态
  reducers: {
    // 启用 immer 之后
    changeAdminname(state, action) {
      state.adminname = action.payload;
    },
    changeToken(state, action) {
      state.token = action.payload;
    },
  }
};

export default IndexModel;
```



> 得到服务器的响应，如果登录成功，应该将信息保存到本地并且跳转到系统的首页

### 4.测试状态

```tsx
// src/components/Header.tsx
import { connect } from "umi"

export default connect((state: any) => {
  return {
    adminname: state.admin.adminname
  }
})((props: any) => {
  return (
    <div>header - { props.adminname }</div>
  )
})
```

```tsx
// src/app.tsx
import React from 'react';
import './global.less';
import {
  BasicLayoutProps,
  Settings as LayoutSettings,
} from '@ant-design/pro-layout';
import Header from './components/Header';

export const layout = ({
  initialState,
}: {
  initialState: { settings?: LayoutSettings; };
}): BasicLayoutProps => {
  return {
    rightContentRender: () => <Header/>,
    footerRender: () => <footer >footer</footer>,
    menuHeaderRender: undefined,
    ...initialState?.settings,
  };
};
```



## 八.头部 - 退出

```tsx
// src/components/Header.tsx
import { connect, history } from "umi"
import store2 from 'store2'
export default connect((state: any) => {
  return {
    adminname: state.admin.adminname
  }
})((props: any) => {
  return (
    <div> 
      { props.adminname }
      <span onClick={ () => {
        store2.clear()
        history.push('/login')
      }}>退出</span>
    </div>
  )
})
```



## 九.产品列表 - 了解

mock产品列表数据

```ts
// mock/pro.ts
import Mock from 'mockjs'
export default {
  'GET /admin/pro/list': (req: any, res: any) => {

    const token = req.headers.token || req.body.token || req.query.token

    if (token) {
      if (token !== 'ty2206') {
        res.json({
          code: '10119',
          message: 'token已失效'
        })
       } else {
        const data = [] // 产品列表数据
        const categoryArr = ['手机', '电脑', '方便面', '化妆品'] // 分类数组
        for (var i = 0; i < 2000; i++) {
          // http://mockjs.com/examples.html
          data.push({
            banners: [
              Mock.Random.image('120x120', Mock.Random.color().toUpperCase(), "#FFF", Mock.mock('@word(5)').toUpperCase()),
              Mock.Random.image('120x120', Mock.Random.color().toUpperCase(), "#FFF", Mock.mock('@word(5)').toUpperCase()), 
              Mock.Random.image('120x120', Mock.Random.color().toUpperCase(), "#FFF", Mock.mock('@word(5)').toUpperCase())
            ],
            proid: 'pro' + new Date().getTime(),
            category: categoryArr[Math.floor(Math.random() * categoryArr.length)],
            proname: Mock.mock('@cparagraph(1,3)'),
            originprice: Math.floor((Math.random()*10000)),
            sales: Math.floor((Math.random()*20000)),
            stock: Math.floor((Math.random()*50000)),
            desc: Mock.mock('@cparagraph(5,10)'),
            img1: Mock.Random.image('120x120', Mock.Random.color().toUpperCase(), "#FFF", Mock.mock('@word(5)').toUpperCase()),
          })
        }

        res.json({
          code: '200',
          message: '产品列表',
          data
        })
       }
    } else {
      res.json({
        code: '10119',
        message: '请传递token'
      })
    }
    
  },
  
}
```

### 1.新增一个声明文件 pro.d.ts

> *.d.ts 称之为 ts中的声明文件, 写代码时可以自动提示 数据类型
>
> Src/pages/pro/pro.d.ts

```ts
export interface IPro {
  banners: Array<string>
  proid: string
  category: string
  proname: string
  originprice: number
  sales: number
  stock: number
  desc: string
  img1: string
}
```

### 2.构建关于产品管理的dva数据流

dva数据流有三种使用方式

* Src/models/*.ts.      * 要和定义的模块的namespace保持一致。 ------  登录数据流
* src/pages/models/*.ts

```ts
// src/api/pro.ts

import request from '@/utils/request'

// 登录
export function getProList () {
  return request({
    url: '/pro/list',
    method: 'GET'
  })
}

```



//src/models/pro.ts

```ts
// src/models/pro.ts
import { getProList } from '@/api/pro';
import { IPro } from '@/pages/pro/pro';
import { Effect, ImmerReducer, Reducer, Subscription } from 'umi';

export interface ProModelState {
  proList: IPro[];
}

export interface ProModelType {
  namespace: 'pro';
  state: ProModelState;
  effects: {
    getProListAction: Effect;
  };
  reducers: {
    changeProList: ImmerReducer<ProModelState>;
   
  }
}

const IndexModel: ProModelType = {
  namespace: 'pro',

  state: {
    proList: []
  },

  effects: {
    *getProListAction({ payload }, { call, put }): any {
      const res = yield call(getProList)
      yield put({
        type: 'changeProList',
        payload: res.data.data
      })
    },
  },
  reducers: {
    changeProList (state, action) {
      state.proList = action.payload
    }
  }
};

export default IndexModel;
```



### 3.产品列表页面请求数据并且展示

https://procomponents.ant.design/components/table#%E5%88%97%E8%A1%A8%E5%B7%A5%E5%85%B7%E6%A0%8F

// src/pages/pro/index.tsx

```tsx
import { useEffect, useState } from 'react';
import { connect, ProModelState } from 'umi';
import styles from './index.less';
import { Table, Image, Button } from 'antd'
import { DeleteOutlined } from '@ant-design/icons';
const Column = Table.Column
function IndexPage({ proList, dispatch }: any) {
  useEffect(() => {
    dispatch({
      type: 'pro/getProListAction'
    })
  }, [])
  const [height] = useState(document.body.offsetHeight) // 计算body的高度
  const [current, setCurrent] = useState(1)
  const [pageSize, setPageSize] = useState(10)
  const onChange = (page: number, pageSize: number) => {
    setCurrent(page)
    setPageSize(pageSize)
  }
  return (
    <div>
      <Table dataSource={ proList } rowKey = "proid" scroll={ { y: height - 300 } }
        pagination = { {
          // position: ['bottomLeft', 'topRight']
          showQuickJumper: true,
          showSizeChanger: true,
          current,
          pageSize,
          onChange,
          total: proList.length,
          showTotal: (total: number) => `共有 ${total} 条数据`
        } }
      >
        <Column title="序号" render = {(text, record, index) => {
          return <span>{ (current - 1) * pageSize + index + 1 }</span>
        }} />
        <Column title="图片" dataIndex="img1" render = {(text) => {
          return <Image src = { text } style={{ height: 60, width: 100 }}></Image>
        }} />
        <Column title="产品名称" dataIndex="proname" />
        <Column title="价格" dataIndex="originprice" />
        <Column title="操作" dataIndex="img" render = {(text, record: any, index) => {
          return <Button danger shape="circle" icon={<DeleteOutlined />} />
        }} />
      </Table>
    </div>
  );
}


export default connect(({ pro }: { pro: ProModelState}) => {
  return {
    proList: pro.proList
  }
})(IndexPage)
```

## 十.权限管理 -了解

https://v3.umijs.org/zh-CN/docs/routing#wrappers

### 设定权限校验的组件 auth.tsx

Src/wrappers/auth.tsx

```tsx
// src/wrappers/auth.tsx
import { connect, Redirect } from 'umi'
import { AdminModelState } from '@/pages/login/model'
const Auth = (props: any) => {
  const isLogin = props.token !== '' && props.token === 'ty2206'
  if (isLogin) {
    return <div>{ props.children }</div>;
  } else {
    return <Redirect to="/login" />;
  }
}

export default connect(({ admin }: { admin: AdminModelState}) => {
  return {
    token: admin.token
  }
})(Auth)
```

### 配置路由的权限

// 其实是登录的权限

```ts
// config/route.ts
export const routes = [
  {
    path: '/',
    redirect: '/home',
    exact: true
  },
  {
    path: '/login',
    // name: '登录', // name 不写即可不出现在左侧菜单栏
    icon: 'HomeOutlined',
    component: '@/pages/login/Index',
    // 不展示顶栏
    headerRender: false,
    // 不展示页脚
    footerRender: false,
    // 不展示菜单
    menuRender: false,
  },
  {
    path: '/home',
    name: '系统首页',
    icon: 'HomeOutlined',
    component: '@/pages/home/Index',
    wrappers: [
      '@/wrappers/auth.tsx',
    ], 
  },
  {
    path: '/banner',
    name: '轮播图管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/banner/list',
        name: '轮播图列表',
        component: '@/pages/banner/List',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/banner/add',
        // name: '添加轮播图',
        component: '@/pages/banner/Add',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  },
  {
    path: '/pro',
    name: '产品管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/pro/list',
        name: '产品列表',
        component: '@/pages/pro/List',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/pro/search',
        name: '筛选列表',
        component: '@/pages/pro/Search',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  },
  {
    path: '/account',
    name: '账户管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/account/user',
        name: '用户列表',
        component: '@/pages/account/User',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/account/admin',
        name: '管理员列表',
        component: '@/pages/account/Admin',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  }
]
```

## 左侧菜单权限

// src/access.ts

```ts
// export default function() {
//   // 假如后端登录以后返回的数据格式为 ['0-0', '0-1', '0-2-1']
//   const checkedKeys = ['0-0', '0-2-0', '0-3-1']
//   return {
//     canReadHome: checkedKeys.includes('0-0'),
//     canReadBannerList: checkedKeys.includes('0-2-0'),
//     canReadBannerAdd: checkedKeys.includes('0-2-1'),
//     canReadProList: checkedKeys.includes('0-3-0'),
//     canReadProSearch: checkedKeys.includes('0-3-1')
//   };
// }

export default function(initialState: any) {
  console.log('initialState', initialState)
  // 假如后端登录以后返回的数据格式为 ['0-0', '0-1', '0-2-1']
  // const checkedKeys = ['0-0', '0-2-0', '0-3-1']
  if (initialState) {
    const  {checkedKeys}  = initialState
    return {
      canReadHome: checkedKeys.includes('0-0'),
      canReadBannerList: checkedKeys.includes('0-2-0'),
      canReadBannerAdd: checkedKeys.includes('0-2-1'),
      canReadProList: checkedKeys.includes('0-3-0'),
      canReadProSearch: checkedKeys.includes('0-3-1')
    };
  } else {
    return {}
  }
  
}
```

> 本项目逻辑
>
> ```ts
> // src/access.ts
> export default function(initialState: any) {
>     if (initialState) {
>       const  adminname = initialState
>       return {
>         canReadHome: adminname === 'admin' || adminname === 'editor',
>          canReadBannerList: adminname === 'editor',
>          canReadBannerAdd: adminname === 'editor',
>          canReadProList: adminname === 'admin',
>          canReadProSearch: adminname === 'admin'
>        };
>       } else {
>        return {}
>       }
>      
>    }
>   ```
>   
> 

// src/app.tsx

```tsx
import {
  BasicLayoutProps
} from '@ant-design/pro-layout';
import {history} from 'umi' 
import { getAdminDetail } from './api/admin';

import './global.less'

export const layout: () => BasicLayoutProps = (): BasicLayoutProps => {
  return {
    rightContentRender: () => <header>
      头部 <button onClick={ () => {
        localStorage.clear()
        history.push('/login')
      }}>退出</button>
    </header>,
    footerRender: () => <footer>底部</footer>
  }
}

export async function getInitialState() {
  const res = await getAdminDetail({ adminname: localStorage.getItem('adminname') as string})
  console.log('data', res)
  return res.data.data[0]
}
```

> 本项目逻辑
>
> ```ts
> // src/app.tsx
> import React from 'react';
> import './global.less';
> import store2 from 'store2'
> import {
>     BasicLayoutProps,
>     Settings as LayoutSettings,
> } from '@ant-design/pro-layout';
> import Header from './components/Header';
> 
> export const layout = ({
>     initialState,
> }: {
>     initialState: { settings?: LayoutSettings; };
> }): BasicLayoutProps => {
>     return {
>        rightContentRender: () => <Header/>,
>        footerRender: () => <footer >footer</footer>,
>        menuHeaderRender: undefined,
>        ...initialState?.settings,
>     };
> };
> 
> export async function getInitialState() {
>     // const data = await fetchXXX();
>     // res.data.data[0] ===   { checkedKeys, adminid, adminname, role }
>     return store2.get('adminname');
>   }
> ```
> 
>

// config/routes.tsx

```tsx
// config/route.ts
export const routes = [
  {
    path: '/',
    redirect: '/home',
    exact: true
  },
  {
    path: '/login',
    // name: '登录', // name 不写即可不出现在左侧菜单栏
    icon: 'HomeOutlined',
    component: '@/pages/login/Index',
    // 不展示顶栏
    headerRender: false,
    // 不展示页脚
    footerRender: false,
    // 不展示菜单
    menuRender: false,
  },
  {
    path: '/home',
    name: '系统首页',
    icon: 'HomeOutlined',
    component: '@/pages/home/Index',
    access: 'canReadHome',
    wrappers: [
      '@/wrappers/auth.tsx',
    ], 
  },
  {
    path: '/banner',
    name: '轮播图管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/banner/list',
        name: '轮播图列表',
        component: '@/pages/banner/List',
        access: 'canReadBannerList',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/banner/add',
        // name: '添加轮播图',
        component: '@/pages/banner/Add',
        access: 'canReadBannerAdd',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  },
  {
    path: '/pro',
    name: '产品管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/pro/list',
        name: '产品列表',
        component: '@/pages/pro/List',
        access: 'canReadProList',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/pro/search',
        name: '筛选列表',
        component: '@/pages/pro/Search',
        access: 'canReadProSearch',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  },
  {
    path: '/account',
    name: '账户管理',
    icon: 'HomeOutlined',
    routes: [
      {
        path: '/account/user',
        name: '用户列表',
        component: '@/pages/account/User',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
      {
        path: '/account/admin',
        name: '管理员列表',
        component: '@/pages/account/Admin',
        wrappers: [
          '@/wrappers/auth.tsx',
        ], 
      },
    ]
  }
]
```

```ts
import { Effect, ImmerReducer, history } from 'umi';
import { adminLoginFn } from '@/api/admin'
import { message } from 'antd'
import store2 from 'store2'
export interface IndexModelState {
  adminname: string;
  token: string
}

export interface IndexModelType {
  namespace: 'admin';
  state: IndexModelState;
  effects: {
    loginFn: Effect;
  };
  reducers: {
    // 启用 immer 之后
    changeAdminname: ImmerReducer<IndexModelState>;
    changeToken: ImmerReducer<IndexModelState>;
  }
}

const IndexModel: IndexModelType = {
  namespace: 'admin',

  state: {
    adminname: store2.get('adminname')|| '',
    token: store2.get('token')|| ''
  },

  effects: {
    *loginFn({ payload }, { call, put }): any {
      // console.log(2)
      // console.log('payload', payload)
      const res = yield call(adminLoginFn, payload)
      console.log(res.data)

      if (res.data.code === '10001') {
        message.error('账户名未注册')
      } else if (res.data.code === '10002') {
        message.error('密码错误')
      } else {
        message.success('登录成功')
        store2.set('adminname', res.data.data.adminname)
        store2.set('token', res.data.data.token)
        // 保存数据到状态管理器
        yield put({
          type: 'changeAdminname',
          payload: res.data.data.adminname
        })
        yield put({
          type: 'changeToken',
          payload: res.data.data.token
        })
        // history.push('/')
        window.location.href="/" //++++++++++++
      }
    },
  },
  reducers: {
    changeAdminname (state, action) {
      state.adminname = action.payload;
    },
    changeToken (state, action) {
      state.token = action.payload;
    }
  }
};

export default IndexModel;
```







## 十二.轮播图相关

Src/pages/banner-manager/banner.d.ts

```ts
export interface IBanner {
  bannerid: string
  img: string
  alt: string
  link: string
  flag: boolean
}

```

Src/pages/banner-manager/list.tsx

```tsx
import * as React from 'react';
import { Table, Button, Image, Space } from 'antd'
import { IBanner } from './banner';
export interface IBannerListProps {
  bannerList: IBanner[]
}

export default function BannerList (props: IBannerListProps) {
  const { bannerList } = props
  const columns = [
    {
      title: '序号',
      render (text: any, record: IBanner, index: number) {
        return <span>{ index + 1}</span>
      }
    },
    {
      title: '链接',
      dataIndex: 'link'
    },
    {
      title: '描述',
      dataIndex: 'alt'
    },
    {
      title: '图片',
      dataIndex: 'img',
      render (text: string, record: IBanner, index: number) {
        return (
          <Image src = { text} width="200" />
        )
      }
    },
    {
      title: '操作',
      render (text: any, record: IBanner, index: number) {
        return (
          <Space>
              <Button type="primary">编辑</Button>
              <Button danger>删除</Button>
          </Space>
        )
      }
    }
  ]
  return (
    <div>
      <Button onClick = { () => {

      }}>添加轮播图</Button>
      <Table dataSource = { bannerList } columns = { columns } rowKey = "bannerid"/>
    </div>
  );
}

```



### 1.构建以及请求轮播图的数据

// services/banner.ts

```ts
import request from './../utils/request'
export interface IBanner {
  img: string;
  alt: string;
  link: string;
}
// 获取轮播图数据
export function getBannerList () {
  return request({
    url: '/banner/list'
  })
}

// 添加轮播图
export function addBanner (params: IBanner) {
  return request({
    url: '/banner/add',
    method: 'POST',
    data: params
  })
}
// 删除单条数据
export function deleteBanner (params: { bannerid: string }) {
  return request({
    url: '/banner/delete',
    data: params
  })
}

```

### 2.构建数据流

> Dva 数据流的使用方式
>
> **src/models/*.ts**
>
> **src/pages/models/*.ts**
>
> **src/pages/model.ts**
>
> ****

// src/pages/banner/banner.d.ts

```ts
// 这是一个声明文件，可以将接口写入到说明文件中
export interface IBanner {
  bannerid: string;
  img: string;
  alt: string;
  link: string;
}

```

// src/models/banner.ts

```tsx
import { getBannerList } from '@/api/banner'
import { IBanner } from '@/pages/banner/banner'
import { Effect, ImmerReducer } from 'umi'
interface IState {
  bannerList: IBanner[]
}

interface BannerModelType {
  namespace: 'banner';
  state: IState;
  effects: {
    getBannerListAction: Effect
  };
  reducers: {
    CHANGE_BANNER_LIST: ImmerReducer<IState>
  };
}

const bannerModel: BannerModelType = {
  namespace: 'banner',
  state: {
    bannerList: []
  },
  effects: {
    * getBannerListAction (action, { put, call }) {
      const res = yield call(getBannerList)
      yield put({
        type: 'CHANGE_BANNER_LIST',
        payload: res.data.data
      })
    }
  },
  reducers: {
    CHANGE_BANNER_LIST (state, action) {
      state.bannerList = action.payload
    }
  }
}
export default bannerModel

```



### 3.列表页面请求数据

```tsx
import * as React from 'react';
import { Table, Button, Image, Space } from 'antd'
import { IBanner } from './banner';
import { useMount } from 'ahooks';
import { ConnectRC, connect } from 'umi';
export interface IBannerListProps {
  bannerList: IBanner[]
}

const BannerList: ConnectRC<IBannerListProps> =  (props) =>  {
  useMount(() => {
    props.dispatch({
      type: 'banner/getBannerListReq'
    })
  })
  const { bannerList } = props
  const columns = [
    {
      title: '序号',
      render (text: any, record: IBanner, index: number) {
        return <span>{ index + 1}</span>
      }
    },
    {
      title: '链接',
      dataIndex: 'link'
    },
    {
      title: '描述',
      dataIndex: 'alt'
    },
    {
      title: '图片',
      dataIndex: 'img',
      render (text: string, record: IBanner, index: number) {
        return (
          <Image src = { text} width={200} />
        )
      }
    },
    {
      title: '操作',
      render (text: any, record: IBanner, index: number) {
        return (
          <Space>
              <Button type="primary">编辑</Button>
              <Button danger>删除</Button>
          </Space>
        )
      }
    }
  ]
  return (
    <div>
      <Button onClick = { () => {

      }}>添加轮播图</Button>
      <Table dataSource = { bannerList } columns = { columns } rowKey = "bannerid"/>
    </div>
  );
}

export default connect(({ banner }: any) => ({ bannerList: banner.bannerList}))(BannerList)

```

### 

### 5.数据的删除

```tsx
import * as React from 'react';
import { Table, Button, Image, Space, Popconfirm } from 'antd'
import { IBanner } from './banner';
import { useMount } from 'ahooks';
import { ConnectRC, connect } from 'umi';
import { removeBanner } from '@/services/banner';
export interface IBannerListProps {
  bannerList: IBanner[]
}

const BannerList: ConnectRC<IBannerListProps> =  (props) =>  {
  useMount(() => {
    props.dispatch({
      type: 'banner/getBannerListReq'
    })
  })
  const { bannerList } = props
  const columns = [
    {
      title: '序号',
      render (text: any, record: IBanner, index: number) {
        return <span>{ index + 1}</span>
      }
    },
    {
      title: '链接',
      dataIndex: 'link'
    },
    {
      title: '描述',
      dataIndex: 'alt'
    },
    {
      title: '图片',
      dataIndex: 'img',
      render (text: string, record: IBanner, index: number) {
        return (
          <Image src = { text} width={200} />
        )
      }
    },
    {
      title: '操作',
      render (text: any, record: IBanner, index: number) {
        return (
          <Space>
              <Button type="primary">编辑</Button>
              <Popconfirm
                title="确认删除此轮播图吗?"
                onConfirm={ () => {
                  removeBanner({ bannerid: record.bannerid }).then(() => {
                    // 删除此数据成功
                    // 重新获取一次数据
                    props.dispatch({
                      type: 'banner/getBannerListReq'
                    })
                  })
                }}
                onCancel={ () => {}}
              >
                <Button danger >删除</Button>
              </Popconfirm>
          </Space>
        )
      }
    }
  ]
  return (
    <div>
      <Button onClick = { () => {

      }}>添加轮播图</Button>
      <Table dataSource = { bannerList } columns = { columns } rowKey = "bannerid"/>
    </div>
  );
}

export default connect(({ banner }: any) => ({ bannerList: banner.bannerList}))(BannerList)

```

### 6.添加轮播图数据

// src/pages/banner_manager/add.tsx

```tsx
import React, { useState, useRef } from 'react'
import { Input, Space, Button, Image } from 'antd';
import { addBanner } from '../../api/banner'
import { useHistory } from 'react-router-dom'
function Com() {
  const history = useHistory()
  const linkRef = useRef<any>()
  const altRef = useRef<any>()
  const fileRef = useRef<any>()
  const [img, setImg] = useState('')
  return (
    <Space direction = "vertical">
      <Input ref = { linkRef } placeholder = "link"></Input>
      <Input ref = { altRef } placeholder = "alt"></Input>
      <Input ref = { fileRef } type="file" onChange = { () => {
        console.log(fileRef.current.input)
        const reader = new FileReader()
        reader.readAsDataURL(fileRef.current.input.files[0])
        reader.onload = function () {
          setImg(this.result as string)
        }
      }}></Input>
      <Image src = { img } />
      <Button type="primary" onClick = { () => {
        // console.log(linkRef.current.input.value)
        // console.log({
        //   link: linkRef.current.input.value,
        //   alt: altRef.current.input.value,
        //   img: img
        // })
        addBanner({
          link: linkRef.current.input.value,
          alt: altRef.current.input.value,
          img: img
        }).then(() => {
          history.push('/banner/list')
        })
      }}>添加</Button>
    </Space>
  )
}

export default Com



```

// 配置路由

```ts
export interface IChildRoute {
  path: string
  name?: string
  component: string,
  wrappers?: string[]
}
export interface IBestAFSRoute {
  routes?: IChildRoute[] // Array<IChildRoute>
  path: string
  redirect?: string,
  exact?: boolean,
  component?: string
  name?: string // 兼容此写法
  icon?: string
  // 更多功能查看
  // https://beta-pro.ant.design/docs/advanced-menu
  // ---
  // 新页面打开
  target?: string
  // 不展示顶栏
  headerRender?: boolean
  // 不展示页脚
  footerRender?: boolean
  // 不展示菜单
  menuRender?: boolean
  // 不展示菜单顶栏
  menuHeaderRender?: boolean
  // 权限配置，需要与 plugin-access 插件配合使用
  access?: string
  // 隐藏子菜单
  hideChildrenInMenu?: boolean
  // 隐藏自己和子菜单
  hideInMenu?: boolean
  // 在面包屑中隐藏
  hideInBreadcrumb?: boolean
  // 子项往上提，仍旧展示,
  flatMenu?: boolean,
  wrappers?: string[],
  role?: number
}
const routes: IBestAFSRoute[] = [
  {
    path: '/',
    exact: true,
    redirect: '/home'
  },
  {
    path: '/home',
    icon: 'HomeOutlined',
    name: '首页', // 如果需要出现在左侧的菜单栏
    component: '@/pages/home/index'
  },
  {
    // 登录页面不需要侧边菜单栏等
    path: '/login',
    // name: '登录',
    component: '@/pages/login/index',
    // 不展示顶栏
    headerRender: false,
    // 不展示页脚
    footerRender: false,
    // 不展示菜单
    menuRender: false
  },
  {
    path: '/banner',
    name: '轮播图管理',
    icon: 'FileImageOutlined',
    routes: [
      {
        path: '/banner/list',
        name: '轮播图列表',
        wrappers: [
          '@/wrappers/auth',
        ],
        component: '@/pages/banner-manager/list'
      },
      {
        path: '/banner/add',
        // name: '添加轮播图',
        wrappers: [
          '@/wrappers/auth',
        ],
        component: '@/pages/banner-manager/add'
      }
    ]
  },
  {
    path: '/pro',
    name: '产品管理',
    icon: 'UnorderedListOutlined',
    routes: [
      {
        path: '/pro/list',
        name: '产品列表',
        wrappers: [
          '@/wrappers/role',
        ],
        component: '@/pages/pro-manager/list'
      },
      {
        path: '/pro/recommend',
        name: '推荐列表',
        wrappers: [
          '@/wrappers/role',
        ],
        component: '@/pages/pro-manager/recommend'
      },
      {
        path: '/pro/seckill',
        name: '秒杀列表',
        wrappers: [
          '@/wrappers/role',
        ],
        component: '@/pages/pro-manager/seckill'
      }
    ]
  }
]

export default routes

```

## 十三.面包屑导航

// src/components/MyBreadcrumb/index.tsx

```tsx
import * as React from 'react';
import { Breadcrumb } from 'antd'
import { useMount } from 'ahooks';
import { useLocation } from 'umi';
interface IMyBreadcrumbProps {
}

const MyBreadcrumb: React.FunctionComponent<IMyBreadcrumbProps> = (props) => {
  const { pathname } = useLocation()
  console.log(pathname)
  const [ title, setTitle ] = React.useState('')
  const [ subTitle, setSubTitle ] = React.useState('')
  const breadcrumbList = {
    '/banner/list': {
      title: '轮播图管理',
      subTitle: '轮播题列表'
    },
    '/pro/list': {
      title: '产品管理',
      subTitle: '产品列表'
    },
    '/pro/recommend': {
      title: '产品管理',
      subTitle: '推荐列表'
    },
    '/pro/seckill': {
      title: '产品管理',
      subTitle: '秒杀列表'
    }
  }
  useMount(() => {
    // 获取地址栏的 pathname
    setTitle(breadcrumbList[pathname].title)
    setSubTitle(breadcrumbList[pathname].subTitle)
  })
  return (
    <Breadcrumb>
      <Breadcrumb.Item>首页</Breadcrumb.Item>
      <Breadcrumb.Item>
        <a href="">{ title }</a>
      </Breadcrumb.Item>
      <Breadcrumb.Item>
        <a href=""> { subTitle }</a>
      </Breadcrumb.Item>
    </Breadcrumb>
  );
};

export default MyBreadcrumb;

```

// 页面中使用 以产品列表为例

```tsx
import React from 'react';
import {MyBreadcrumb} from '@/components/myBreadcrumb'
type ProListProps = {

};

const index:React.FC<ProListProps> = () => {

  return (
    <>
      <MyBreadcrumb />
      <div>产品列表</div>
    </>
  )
}
export default index;

```

## 十四、管理员操作

详情见代码

编辑使用抽屉，添加使用对话框



token的使用 --- 拦截器

```ts
import axios, { AxiosInstance, AxiosRequestConfig, AxiosResponse, Method, AxiosError } from 'axios'
import { getItem } from './cookie';
import { history } from 'umi'

const isDev: boolean = process.env.NODE_ENV === 'development'

const ins: AxiosInstance = axios.create({
  baseURL: isDev ? 'http://121.89.205.189/admin' : 'http://121.89.205.189/admin',
  timeout: 6000
})
// 请求拦截器
ins.interceptors.request.use(function (config: AxiosRequestConfig): AxiosRequestConfig {
  // 在发送请求之前做些什么
  // 设定加载的进度条  / 统一传递token 信息(从本地获取) **********************************
  config.headers.common['token'] = getItem('token') || ''
  return config;
}, function (error: any): Promise<never> {
  // 对请求错误做些什么
  return Promise.reject(error);
});

// 添加响应拦截器
ins.interceptors.response.use(function (response: AxiosResponse<any>): AxiosResponse<any> {
  // 对响应数据做点什么
  // 进度条消失 /  验证token的有效性    **********************************
  if (response.data.code === '10119') { // 我的接口token失效 { code: '10119', message: 'token无效'}
    //跳转到登录页面
    window.location.href = "/login"
    return response;
  }
  return response;
}, function (error: any): Promise<never> {
  // 对响应错误做点什么
  return Promise.reject(error);
});

// http://www.axios-js.com/zh-cn/docs/#axios-config
// 自定义各种数据请求 axios({})
export default function request(config: AxiosRequestConfig): Promise<AxiosResponse<any>>{
  let { url = '', method = 'GET', data = {}, headers = '' } = config
  // url = url || ''
  // method = method || 'get'
  // data = data || {}
  // headers = headers || ''
  // method 转换为大写
  switch (method.toUpperCase()) {
    case 'GET':
      return ins.get(url, { params: data })
    case 'POST':
      // 表单提交  application/x-www-form-url-encoded
      if (headers['content-type'] === 'application/x-www-form-url-encoded') {
        // 转参数 URLSearchParams/第三方库qs
        const p = new URLSearchParams()
        for(let key in data) {
          p.append(key, data[key])
        }
        return ins.post(url, p, {headers})
      }
      // 文件提交  multipart/form-data
      if (headers['content-type'] === 'multipart/form-data') {
        const p = new FormData()
        for(let key in data) {
          p.append(key, data[key])
        }
        return ins.post(url, p, {headers})
      }
      // 默认 application/json
      return ins.post(url, data)
    case 'PUT': // 修改数据 --- 所有的数据的更新
      return ins.put(url, data)
    case 'DELETE': // 删除数据
      return ins.delete(url, {data})
    case 'PATCH': // 更新局部资源
      return ins.patch(url, data)
    default:
      return ins(config)
  }
}

```



### 十三作业：

需要自主完成管理员列表的相关功能

adminname

password

role    2 超级管理员。1管理员



## 

## 十五.富文本编辑器的使用

https://braft.margox.cn/demos/basic

```
cnpm i braft-editor -S
```

创建页面以及路由

### 1.创建表单页面

Src/pages/editor/Rich.tsx

```tsx
import * as React from 'react';
// 引入编辑器组件
import BraftEditor from 'braft-editor'
// 引入编辑器样式
import 'braft-editor/dist/index.css'

interface IRichProps {
}

const Rich: React.FunctionComponent<IRichProps> = (props) => {
  const [ editorState, setEditorState ] = React.useState('')
  const [ text, setText ] = React.useState('')
  const handleEditorChange = (editorState) => {
    console.log('change', editorState )
    setEditorState(editorState)
  }
  const submitContent = () => {
    const html = editorState.toHTML()
    setText(html)
    console.log(html)
  }
  return (
    <div>
      <BraftEditor
        value={editorState}
        onChange={handleEditorChange}
        onSave={submitContent}
      />
      asdasdasdsads
      <div dangerouslySetInnerHTML = {{ __html: text}} style={{ height: 600}}>
      </div>
    </div>
  );
};

export default Rich;

```

```
{
    path: '/rich',
    icon: 'HomeOutlined',
    name: '富文本编辑器', // 如果需要出现在左侧的菜单栏
    component: '@/pages/editor/Rich'
  },
```

可以使用markdown编辑器

https://gitee.com/uiw/react-markdown-editor#https://github.com/jaywcjlove/react-monacoeditor

s r c/pages/editor/MarkDown.tsx

```tsx
import * as React from 'react';
import MarkdownEditor from '@uiw/react-markdown-editor';
interface IMarkDownProps {
}

const MarkDown: React.FunctionComponent<IMarkDownProps> = (props) => {
  const [markdown, setMarkdown] = React.useState('')
  const updateMarkdown = (editor,data,value) => {
    console.log(editor)
    console.log(data)
    console.log(value)
    setMarkdown(value)

  }
  return (
    <div>
      markdown编辑器
      <MarkdownEditor
        value={markdown}
        onChange={updateMarkdown}
        height = { 600 }
      />
      { markdown }
    </div>
  )
};

export default MarkDown;

```

```ts
export interface IChildRoute {
  path: string
  name?: string
  component: string,
  wrappers?: string[]
}
export interface IBestAFSRoute {
  routes?: IChildRoute[] // Array<IChildRoute>
  path: string
  redirect?: string,
  exact?: boolean,
  component?: string
  name?: string // 兼容此写法
  icon?: string
  // 更多功能查看
  // https://beta-pro.ant.design/docs/advanced-menu
  // ---
  // 新页面打开
  target?: string
  // 不展示顶栏
  headerRender?: boolean
  // 不展示页脚
  footerRender?: boolean
  // 不展示菜单
  menuRender?: boolean
  // 不展示菜单顶栏
  menuHeaderRender?: boolean
  // 权限配置，需要与 plugin-access 插件配合使用
  access?: string
  // 隐藏子菜单
  hideChildrenInMenu?: boolean
  // 隐藏自己和子菜单
  hideInMenu?: boolean
  // 在面包屑中隐藏
  hideInBreadcrumb?: boolean
  // 子项往上提，仍旧展示,
  flatMenu?: boolean,
  wrappers?: string[],
  role?: number
}
const routes: IBestAFSRoute[] = [
  ....
  {
    path: '/mk',
    icon: 'HomeOutlined',
    name: 'markdown编辑器', // 如果需要出现在左侧的菜单栏
    component: '@/pages/editor/MarkDown'
  },
]

export default routes

```



## 十六.数据可视化

ECharts https://echarts.apache.org/zh/index.html

### 1.html中使用echarts

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>ECharts</title>
    <!-- 引入 echarts.js -->
    <script src="echarts.min.js"></script>
</head>
<body>
    <!-- 为ECharts准备一个具备大小（宽高）的Dom -->
    <div id="main" style="width: 600px;height:400px;"></div>
    <script type="text/javascript">
        // 基于准备好的dom，初始化echarts实例
        var myChart = echarts.init(document.getElementById('main'));

        // 指定图表的配置项和数据
        var data = genData(50);

option = {
    title: {
        text: '同名数量统计',
        subtext: '纯属虚构',
        left: 'center'
    },
    tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b} : {c} ({d}%)'
    },
    legend: {
        type: 'scroll',
        orient: 'vertical',
        right: 10,
        top: 20,
        bottom: 20,
        data: data.legendData,

        selected: data.selected
    },
    series: [
        {
            name: '姓名',
            type: 'pie',
            radius: '55%',
            center: ['40%', '50%'],
            data: data.seriesData,
            emphasis: {
                itemStyle: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                }
            }
        }
    ]
};




function genData(count) {
    var nameList = [
        '赵', '钱', '孙', '李', '周', '吴', '郑', '王', '冯', '陈', '褚', '卫', '蒋', '沈', '韩', '杨', '朱', '秦', '尤', '许', '何', '吕', '施', '张', '孔', '曹', '严', '华', '金', '魏', '陶', '姜', '戚', '谢', '邹', '喻', '柏', '水', '窦', '章', '云', '苏', '潘', '葛', '奚', '范', '彭', '郎', '鲁', '韦', '昌', '马', '苗', '凤', '花', '方', '俞', '任', '袁', '柳', '酆', '鲍', '史', '唐', '费', '廉', '岑', '薛', '雷', '贺', '倪', '汤', '滕', '殷', '罗', '毕', '郝', '邬', '安', '常', '乐', '于', '时', '傅', '皮', '卞', '齐', '康', '伍', '余', '元', '卜', '顾', '孟', '平', '黄', '和', '穆', '萧', '尹', '姚', '邵', '湛', '汪', '祁', '毛', '禹', '狄', '米', '贝', '明', '臧', '计', '伏', '成', '戴', '谈', '宋', '茅', '庞', '熊', '纪', '舒', '屈', '项', '祝', '董', '梁', '杜', '阮', '蓝', '闵', '席', '季', '麻', '强', '贾', '路', '娄', '危'
    ];
    var legendData = [];
    var seriesData = [];
    for (var i = 0; i < count; i++) {
        var name = Math.random() > 0.65
            ? makeWord(4, 1) + '·' + makeWord(3, 0)
            : makeWord(2, 1);
        legendData.push(name);
        seriesData.push({
            name: name,
            value: Math.round(Math.random() * 100000)
        });
    }

    return {
        legendData: legendData,
        seriesData: seriesData
    };

    function makeWord(max, min) {
        var nameLen = Math.ceil(Math.random() * max + min);
        var name = [];
        for (var i = 0; i < nameLen; i++) {
            name.push(nameList[Math.round(Math.random() * nameList.length - 1)]);
        }
        return name.join('');
    }
}

        // 使用刚指定的配置项和数据显示图表。
        myChart.setOption(option);
    </script>
</body>
</html>
```

### 2.在react中使用echarts

https://echarts.apache.org/zh/tutorial.html#%E5%9C%A8%E6%89%93%E5%8C%85%E7%8E%AF%E5%A2%83%E4%B8%AD%E4%BD%BF%E7%94%A8%20ECharts

```
cnpm install echarts --save
```

Src/pages/data-manager/echarts.tsx

```tsx
import React from 'react';
import * as echarts from 'echarts';

export interface IAppProps {
}

export interface IAppState {
  myChart: any,
  option: any
}

export default class App extends React.Component<IAppProps, IAppState> {
  constructor(props: IAppProps) {
    super(props);

    this.state = {
      myChart: null,
      option: {}
    }
  }
  componentDidMount () {
    this.setState({
      myChart: echarts.init((document.getElementById('box') as any)),
      option: { // https://echarts.apache.org/zh/option.html#title
        title: {
          text: 'ECharts 入门示例',
          subtext: 'echarts简单',
          left: '50%',
          textAlign: 'center',
          // show: false
          link: 'https://www.baidu.com',
          textStyle: {
            color: '#f66'
          }
        },
        legend: {
          bottom: 10
        },
        grid: {
          show: true
        },
        brush: {
          xAxisIndex: 'all',
          brushLink: 'all',
          outOfBrush: {
              colorAlpha: 0.1
          }
      },
        tooltip: {},
        axisPointer: {
          show: true,
          link: {xAxisIndex: 'all'},
          label: {
              backgroundColor: '#777'
          }
        },
        toolbox: {
          show: true,
        showTitle: false, // 隐藏默认文字，否则两者位置会重叠
        feature: {
            saveAsImage: {
                show: true,
                title: 'Save As Image'
            },
            dataView: {
                show: true,
                title: 'Data View'
            },
        },
        tooltip: { // 和 option.tooltip 的配置项相同
            show: true,
            formatter: function (param) {
                return '<div>' + param.title + '</div>'; // 自定义的 DOM 结构
            },
            backgroundColor: '#222',
            textStyle: {
                fontSize: 12,
            },
            extraCssText: 'box-shadow: 0 0 3px rgba(0, 0, 0, 0.3);' // 自定义的 CSS 样式
        }
        },
        xAxis: {
          data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        dataZoom: {},
        yAxis: {},
        // darkMode: 'dark',
        // backgroundColor: 'rgba(0,0,0, 0.9)',
        // polar: {},
        series: [{
            name: '销量',
            type: 'bar',
            data: [5, 20, 36, 10, 10, 20]
        },
        {
          name: '库存',
          type: 'line',
          data: [15, 120, 136, 110, 110, 120]
      }]
      }
    }, () => {
      this.state.myChart.setOption(this.state.option)
    })
  }
  changeLine = () => {
    this.setState({
      option: {
        title: {
            text: 'ECharts 入门示例'
        },
        tooltip: {},
        xAxis: {
            data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [{
            name: '销量',
            type: 'line',
            data: [5, 20, 36, 10, 10, 20]
        }]
      }
    }, () => {
      this.state.myChart.setOption(this.state.option)
    })
  }
  changeLineAndData = () => {
    this.setState({
      option: {
        title: {
          text: 'ECharts 入门示例'
        },
        tooltip: {},
        xAxis: {
            data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子']
        },
        yAxis: {},
        series: [{
            name: '销量',
            type: 'line',
            data: [15, 25, 16, 50, 70, 120]
        }]
      }
    }, () => {
      this.state.myChart.setOption(this.state.option)
    })
  }
  public render() {
    return (
      <div>
        <h1>echarts案例</h1>
        <button onClick = { this.changeLine }>折线图</button>
        <button onClick = { this.changeLineAndData }>折线图-改变数据</button>
        <div id="box" style={{ width: 1000, height: 800, backgroundColor: '#fff'}}></div>
      </div>
    );
  }
}


```

```ts
{
    path: '/data',
    name: '数据可视化',
    icon: 'PictureOutlined',
    routes: [
      {
        path: '/data/echarts',
        name: 'echarts数据可视化',
        component: '@/pages/data-manager/echarts',
      }
    ]
  }
```

```tsx
import React from 'react'
import * as echarts from 'echarts'
import { useMount } from 'ahooks';
export default function Echarts() {
  let myChart = ''
  const [option, setOption] = React.useState({
    xAxis: {
      type: 'category',
      data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
    },
    yAxis: {
      type: 'value'
    },
    series: [{
      data: [150, 230, 224, 218, 135, 147, 260],
      type: 'line'
    }]
  })
  useMount(() => {

    draw()
  })
  function draw (){
    myChart = echarts.init(document.getElementById('main'))
    myChart.setOption(option)
  }
  function changeData1 () {
    setOption({
      xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
        type: 'value'
      },
      series: [{
        data: [150, 230, 224, 218, 135, 147, 260],
        type: 'bar'
      }]
    })
    draw()
  }
  function changeData2 () {
    setOption({
      xAxis: {
        type: 'category',
        data: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
      },
      yAxis: {
        type: 'value'
      },
      series: [{
        data: [150, 230, 224, 218, 135, 147, 260],
        type: 'pie'
      }]
    })
    draw()
  }
  return (
    <div>
      <button onClick = { changeData1 }>改变数据-柱状图</button>
      <button onClick = { changeData2 }>改变数据-饼状图</button>
      <div id="main" style={{ width: '600px',height: '500px'}}></div>
    </div>
  );
}

```



### 3.其余的数据可视化工具

highcharts https://www.highcharts.com.cn/ 使用方法类似于echarts，但是。。。。

antv https://antv.vision/   https://antv.gitee.io/zh/

react - https://charts.ant.design/

### 在 React / Vue / Angular 中使用 G2

基于 AntV 技术栈还有许多优秀的项目，在 React 环境下使用 G2，我们推荐使用 Ant Design Charts，BizCharts 和 Viser。这三个产品都是基于 G2 的 React 版本封装，使用体验更符合 React 技术栈的习惯，他们都与 AntV 有着紧密的协同，他们很快也将同步开源和发布基于 G2 4.0 的版本。Viser 除了 React 外，还提供了 Vue 和 Angular 不同的分发版本。

- Ant Design Charts 地址：[https://charts.ant.design](https://charts.ant.design/)
- BizCharts 地址：[https://bizcharts.net](https://bizcharts.net/)
- Viser 地址：https://viserjs.github.io/



### 自己写

D3.js自定义数据可视化



Access-token. Refresh-token

80. vue里面有一个MVVM的模型，对此怎么理解？它有什么作用？
81. 双向数据绑定怎么实现？
82. 立即反映到视图层或模型层是怎么做到的？
83. 简单实现一下双向数据绑定，用JS去实现，这个要怎么做？
84. 它可以监听哪些事件？
85. 有自己封装过vue组件吗？
86. Vue组件之间参数传递
87. 遇到跨域问题，是怎么处理的？
88. 页面上有比较复杂的数据结构，页面的表格可能比较复杂，当我要修改其中内容提交的时候，需要将这些数据提交上去，这个表格可能是一个list，里面是个对象，对象里可能又有一些数组，这样多层嵌套的数据结构，页面处理的时候需要怎么做？需要注意些什么？（比如调一些接口，服务端接口去查询，然后给返回了一个这样的列表，你要再页面上去展示，需要做些什么？）
89. 正常显示一个列表是怎么显示的？通过什么组件？
90. 开发过程中对组件库不熟，平时用到一个组件的时候怎么办呢？去哪找这个组件？
91. 平常你在工作当中你是怎么去找我现在要用一个什么组件来完成现在的工作，因为你说你没封装过组件，那工作当中就是找现成的组件用，那你是从哪找的？
92. 举个例子讲讲你平常工作中某一个业务的开发过程，当时你拿到了一个什么样的需求，你是怎么去理解这个需求的？怎么去跟后端开发人员以及其他相关人员交互的，最后怎么去完成这个工作？
93. 这些接口是你告诉后端你需要什么接口，还是后端人员告诉你他有什么接口，然后你来想页面上怎么用？
94. 需求给过来是个什么样子，是文档还是原型图，还是一些其他的形式呢？
95. 从原型图效果图到实际的页面，这中间切图的工作也是你们做的吗？
96. 你的首页和详情页是两个不同的页面，你们是单页面模式还是多页面模式？（我回答的单页面）
97. 那首页跟详情页你是怎么路由的
98. 假如在首页定义了一个定时器，（你是单页面），跳转到详情页面去了后，定时器还会继续生效吗？是还在后台继续运行还是已经被销毁了？
99. 会在生命周期哪个阶段销毁？
100. destroy这个阶段其实有两个方法会被调用，beforeDestroy和destroyed，beforeDestroy里面一般会做什么事情？
说一下你工作中处理过最难的一个前端的问题是什么？

