# react中使用ts

创建一个 react+ts 的项目 
参考地址: https://create-react-app.bootcss.com/docs/getting-started

```shell
npx create-react-app 项目名称 --template typescript
```



## 01: ts在类组件中的使用

```jsx
// 定义类组件
import React, { Component } from 'react';
// 声明 state 的数据类型
interface stateType {
    username: string
}
// Component 后面接收泛型约束, <属性props约束, 自身 的state约束>
class Father extends Component<any, stateType>{
    state: stateType = {
        username: '张三'
    }
    render() {
        return (
            <div>
                <p onClick={() => {
                    this.setState({
                        username: 1  //这样就会有类型校验
                    })
                }}>{this.state.username}</p>
            </div>
        );
    }
}

export default Father;

```

案例: 

```jsx

import React, { Component, createRef } from 'react';
import Son from './Son'
interface stateType {
    username: string
}
class Father extends Component<any, stateType>{
    state = {
        username: '张三'
    }
    ref1 = createRef<HTMLInputElement>();  // 声明ref 的类型
    editFn = () => {
        this.setState({
            username: '李四'
        })
    }
    render() {
        return (
            <div>
                {/* <p onClick={() => {
                    this.setState({
                        username: '123'
                    })
                }}>{this.state.username}</p> */}
                {/* 受控组件写法 */}
                <input type="text" value={this.state.username} onChange={(e) => {
                    this.setState({
                        username: e.target.value
                    })
                }} />
                {/* 非受控组件写法 */}
                <input ref={this.ref1} type='text' defaultValue={this.state.username} />
                <button onClick={() => {
                    // 写法1: 可选链写法
                    console.log(this.ref1?.current?.value);
                    // 写法2: 采用类型断言
                    console.log((this.ref1.current as HTMLInputElement).value);
                    this.setState({
                        username: (this.ref1.current as HTMLInputElement).value
                    })

                }}>提交表单</button>
				{/* 父传子传递参数*/}
                <Son username={this.state.username} editFn={this.editFn}></Son>
            </div >
        );
    };
}

export default Father;

```

Son子组件:

```jsx
import React, { Component } from 'react';
// 定义接收的属性的类型
interface propsType {
    username: string,
    editFn: () => void
}
 // 定义state 的类型
interface stateType {
 isshow: boolean
}    
 // 指定
class Son extends Component<propsType,stateType>{
    state = {
        isshow: true
    }
    render() {
        return (
            <div>
                <p>{this.props.username}</p>
                <button onClick={() => this.props.editFn()}>点击修改</button>
            </div>
        );
    }
}

export default Son;

```



## 02: ts在函数组件中的使用

```jsx
import React, { useState, useRef } from 'react';
import Son from './Son';

const Father = () => {
    // 如果非要设置了类行,可以 useState<类型>(初始值)
    const [username, setusername] = useState<string>('123');
    const ref1 = useRef<HTMLInputElement>(null);
    const changeUsernameFn = () => {
        setusername('测试')
    }
    return (
        <div>
            {/* 受控组件 */}
            {/* <input type='text' value={username} onChange={
                (e) => {
                    // setusername 函数本身会有隐式类行校验, 所以可以不用单独设置类行
                    setusername(e.target.value)
                }
            } /> */}
            {/* 非受控组件 */}
            <input type='text' ref={ref1} defaultValue={username} />
            <button onClick={() => {
                console.log((ref1.current as HTMLInputElement).value);
                setusername((ref1.current as HTMLInputElement).value)
            }}>提交表单</button>
            <Son username={username} changeUsernameFn={changeUsernameFn}></Son>
        </div>
    );
}

export default Father;

```

子组件Son组件:

```jsx
import React from 'react';
// 定义props 的属性类型
interface propsType {
    username?: string  //可设置为可选属性
    changeUsernameFn(): void
}
const Son = (props: propsType) => {
    return (
        <div>
            <p>{props.username}</p>
            <button onClick={() => {
                props.changeUsernameFn()
            }}>点击修改username</button>
        </div>
    );
}
export default Son;

```

注意函数子组件Son组件也可以 写成如下方式:

```jsx
import React from 'react';

interface propsType {
    username: string
    changeUsernameFn(): void
}
// propsType 为props 的类型
const Son: React.FC<propsType> = (props) => {
    return <div>
        <p>{props.username}</p>
        <button onClick={() => {
            props.changeUsernameFn()
        }}>点击修改username</button>
    </div>
}
export default Son;
```



## 03: ts在路由中的使用

在入口文件index.tsx  文件中引入;  当提示类型  'react-router-dom' 的类型错误的时候, 根据提示需要安装
npm i   "@types/react-router-dom"

```jsx
 import { BrowserRouter as Router } from 'react-router-dom';
```

案例: 列表页 跳转到详情页

列表页: 

```tsx
import React from 'react';
import axios from 'axios';
import { useEffect, useState, } from 'react';
import { RouteComponentProps, useHistory } from 'react-router-dom';

// 如果需要使用props中提供的
const Home = () => {
    const [list, setList] = useState<Array<any>>([]);
    const his = useHistory();
    useEffect(() => {
        axios.get('http://47.94.148.165:3001/api/pro/list?count=1').then(res => {
           // console.log(res.data.data);
            setList(res.data.data);
        })
    }, []);
    return (
        <div>
            <ul>
                {
                    list.map((item, index) => {
                        return <li key={index} style={{ width: '100px', height: 'auto' }} onClick={
                            () => {
                                // props.history.push('/detail/' + item.proid)
                                his.push('/detail/' + item.proid)
                            }
                        }><img style={{ width: '100%' }} src={item.img1} /></li>
                    })
                }
            </ul>
        </div>
    );
}

export default Home;

```

list.tsx

~~~
import React,{useState,useEffect} from 'react'
import axios from 'axios'
import type { AxiosResponse } from 'axios';
type Props = {}
// type ArrayItemType=Array<{  //返回值数组的类型
//     brand:string,
//     category:string,
//     img1:string,
//     [key:string]:any
// }> 
// type ResType={  //返回值的类型
//     code:string,
//     data: Array<{
//     brand:string,
//     category:string,
//     img1:string,
//     [key:string]:any
//     }>
// }
type ItemType={  //数组每一项的类型
   brand:string,
   category:string,
   img1:string,
   proid:string,
   [key:string]:string
}
type ArrayType=Array<ItemType>  //整个数组的约束
type ResType={  //返回值类型的约束
    data: ArrayType
}
export default function List({}: Props) {
    const [list,setList]=useState<ArrayType>([]); //返回值是任意类型的数组
    useEffect(()=>{
            axios.get("http://47.94.148.165:3001/api/pro/list?count=1").then((res:AxiosResponse<ResType>)=>{
              setList(res.data.data)
            })
    },[])
  return (
    <div>List
        <ul>
            {
                list.map((item,index)=>{
                    return <li key={index}>
                                <img width="100" src={item.img1} />
                    </li>
                })
            }
        </ul>
    </div>
  )
}
~~~





详情页:

```tsx

import { useLocation, useParams } from 'react-router-dom';
import axios from 'axios';
import { useEffect, useState } from 'react';
interface paramsObj {
    id: string,
    [key: string]: any
}

interface resObj {
    img1: string,
    [key: string]: any
}

const Deatil = () => {
    const loc = useLocation();
    // 需要指定paramsObj 类型,否则 par.id 报错
    const par = useParams<paramsObj>();
    const [detailObj, setDetailObj] = useState<resObj | {}>({})
    // console.log(loc);
    // console.log(par.id)
    useEffect(() => {
        axios.get('http://47.94.148.165:3001/api/pro/detail/' + par.id).then(res => {
            // console.log(res);
            setDetailObj(res.data.data)
        })
    }, [])

    return <div>
        详情页
        <div>
            <img src={(detailObj as resObj).img1} alt="" />
        </div>
    </div>
}

export default Deatil
```



## 04: ts 在redux 中使用

定义store下的index.ts文件

```ts
import { legacy_createStore as createStore } from 'redux';

interface stateType {
    count: number,
    [key: string]: any
}

const defaultState: stateType = {
    count: 0
}

interface actionType {
    type: string,
    payload?: any
}

const reducer = (state: stateType = defaultState, action: actionType) => {
    if (action.type == '+') {
        state.count += action.payload
        return state
    }
    return state
}
// 此处声明一下window, 否则提示报错
declare let window: any;
const store = createStore(reducer, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__());

export default store
```

在项目的入口文件 index.ts 中如下: 

```ts
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

import { BrowserRouter as Router } from 'react-router-dom';
import store from './views/redux/store';
import { Provider } from 'react-redux';

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <Provider store={store}>
    <Router>
      <App />
    </Router>
  </Provider>
);
```

在页面中使用store仓库的数据:

```tsx
import { useSelector, useDispatch } from 'react-redux';
const Car = () => {
    interface stateType {
        count: number,
        [key: string]: any
    }
    const state = useSelector((state: stateType) => state);
    const dispatch = useDispatch()
    console.log(state)
    return <div>
        购物车页
        <p onClick={
            () => {
                dispatch({ type: '+', payload: 10 })
            }
        }>{state.count}</p>
    </div>
};

export default Car
```

npm i  react-redux  @reduxjs/toolkit -S

RTK

store/index.ts

~~~
import {configureStore} from '@reduxjs/toolkit';
import countReducer from './counterSlice'

const store =configureStore({
    reducer:countReducer
  
})

export default store;
~~~

store/countSlice.ts

~~~
import {createSlice} from '@reduxjs/toolkit'

export type stateType ={  //state的类型
    n:number
}

type actionType={  //actionType的类型
    type:string,
    payload:number
}
let countSlice = createSlice({
    name:"counter",
    initialState:{
        n:1
    },
    reducers:{
        change(state:stateType,action:actionType){
            state.n+=action.payload
        }
    }
})

export const {change}=countSlice.actions;
export default countSlice.reducer;
~~~

index.tsx

~~~
import {Provider} from 'react-redux'
import store from './store/index'

<Provider store={store}>
 <Router>
    <App />
 </Router>
 </Provider>
~~~

组件里

~~~
import React from 'react'
import {useSelector,useDispatch} from 'react-redux'
import type {stateType} from '../../store/counterSlice'
import  {change} from '../../store/counterSlice'
type Props = {}

export default function Counter({}: Props) {
    let n=useSelector((state:stateType)=>state.n)
    let dispatch=useDispatch();
  return (
    <div>Counter {n} <button onClick={()=>dispatch(change(10))}>+</button></div>
  )
}
~~~



## 05: react-router @v6版本

react-router-dom@6 是在2021/11  月份 推出的新的路由版本, 目前官方默认安装就是 6版本

```shell
npm i react-router-dom
```

**与 react-router-dom@5版本比较**

01:  内置组件的变化:  移除Switch 新增 Routes  , Routes 相当于 Switch ,移除 Redirect替代产品为Navigate组件

02:  语法变化:  component = {Home}  变成  element={<Home/>}

03: 新增  hooks 如:  useNavigate() 主要用来实现编程式导航的 等



v6语法: 

### 01:引入 BrowserRouter

 在项目的入口文件 和v5一样, 使用browseerRouter或 HashRouter 包裹整个App组件

```ts
// index.ts
import { BrowserRouter as Router } from 'react-router-dom';
const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
    <Router>
      <App />
    </Router>
);

```

### 02:  定义声明式导航

```tsx
import React from 'react';
// 这里需要注意: 
// NavLink 和 Link 两个组件都可以使用, 区别在于一个有点击样式, 一个没有点击样式;
// NavLink选中的导航会有active 类名, 可以设置高亮
import { NavLink, Link } from 'react-router-dom';
import Rules from './Index';
type Props = {}

export default function Base({ }: Props) {
    return (
        <div>
            基础页
            {/*声明式导航*/}
            <p>
                <NavLink to='/home'>首页</NavLink>
                <NavLink to='/category'>分类页</NavLink>
            </p>
            {/* 路由坑 */}
            <Rules></Rules>
        </div>
    )
}
```

### 03:  定义路由规则组件Rules

```tsx
import React from 'react';
//v6中:  Routes  相当Switch. 必须使用该组件包裹Route 否则报错
//V6中: 还是使用 Route定义路由组件的坑及和路由规则
import { Routes, Route } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
    return (
        <div>
            <Routes>
                {/* 需要将component 改成 element 同时赋值也必须是标签形式的组件 */}
                <Route path='/home' element={<Home />}></Route>
                <Route path='/category' element={<Category />}></Route>
            </Routes>
        </div>
    );
}

export default Index;

```

### 04: 路由重定向

```tsx
import React from 'react';
// 路由重定向: 
// 去掉了 Redirect组件, 使用 Navigate导航组件如下: 
// Navigate组件的特点: 只要页面渲染该组件,就会根据Navigate的to属性切换页面
import { Routes, Route,Navigate } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
    return (
        <div>
            <Routes>
                <Route path='/home' element={<Home />}></Route>
                <Route path='/category' element={<Category />}></Route>
                <Route path='/detail' element={<Deatil/>}></Route>
                 {/* 路由重定向 */}
                <Route path='/' element={< Navigate to='/home' />}></Route>
            </Routes>
        </div>
    );
}

export default Index;

```

注意: Navigate 组件特性demo

实现效果:   在detail 详情页 当num值为2时,让其自动跳转到home页 

```tsx
// 在detail 详情页 当num值为2时,让其自动跳转到home页;

import React from 'react'
// 引入 Navigate 组件
import { Navigate } from 'react-router-dom'
import { useState } from 'react'
type Props = {}


export default function Detail({ }: Props) {
    const [num, setnum] = useState(0)
    return (
        <div>
            详情页
            <p>{num}</p>
            {/*当num值为2时,让其自动跳转到home页*/}
            {num == 2 ? <Navigate to='/home'></Navigate> : <></>}
            <button onClick={() => {
                setnum(num + 1)
            }}> +1</button>
        </div>
    )
}
```



### 05: 路由path 的大小写匹配

 注意 :当 NavLink的to属性和 Route组件中的path(变成大写) 可以正常匹配

 注意: 当  Route组件中的path(变成大写)  ,同时设置 caseSensitive属性,则会区分 path的大小写

```tsx

// 声明式导航
 <p>
     <NavLink to='/home'>首页</NavLink>
     {/* path='/CATEGORY' path 大写 与该处的to 属性不匹配*/}
     <NavLink to='/category'>分类页</NavLink>
 </p>
.......

// 如下是路由规则组件
const Index = () => {
    return (
        <div>
            <Routes>
                <Route path='/home' element={<Home />}></Route>
                {/* path='/CATEGORY' path 大写*/}
                <Route path='/CATEGORY' caseSensitive element={<Category />}></Route>
                <Route path='/detail' element={<Deatil />}></Route>
                {/* 路由重定向 */}
                <Route path='/' element={< Navigate to='/home' />}></Route>
            </Routes>
        </div>
    );
}

export default Index;
```



### 06:  定义路由表

第一步: 改造成路由表,修改路由规则组件如下: 
useRoutes:  用来生成路由规则表的钩子函数



```tsx
import React from 'react';
//01:  引入 useRoutes 这个钩子函数, 用来定义路由生成路由规则表
import { Routes, Route, Navigate, useRoutes } from 'react-router-dom';
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
const Index = () => {
   //02: 创建路由规则表 element
    const element = useRoutes([
        {
            path: '/home',
            element: <Home />
        },
        {
            path: '/category',
            element: <Category />
        },
        {
            path: '/detail',
            element: <Deatil />
        },
        {
            path: '/',
            element: <Navigate to='/home' />
        }
    ])
    return (
        <div>
            {/* 使用路由规则表*/}
            {element}
        </div>
    );
}

export default Index;

```

将以上代码改造, 将对应的路由规则表的数组单独导出去,在router目录下,新建 routes.tsx (相当于vue中的routes),代码如下: 

routes.tsx文件: 

```tsx
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
import { Navigate } from 'react-router-dom'
export default [
    {
        path: '/home',
        element: <Home />
    },
    {
        path: '/category',
        element: <Category />
    },
    {
        path: '/detail',
        element: <Deatil />
    },
    {
        path: '/',
        element: <Navigate to='/home' />
    }
]
```

 Index.tsx文件: 

```tsx
import React from 'react';
import { Routes, Route, useRoutes } from 'react-router-dom';
// 引入路由规则
import routes from './routes'
const Index = () => {
    // 使用路由规则
    const element = useRoutes(routes)
    return (
        <div>
            {/*使用element*/}
            {element}
        </div>
    );
}

export default Index;

```



### 07: 嵌套路由

在路由表routes.tsx 中定义二级路由规则 ,写法同vue 基本一致

```tsx
import Home from './Home';
import Category from './Category';
import Deatil from './Detail';
import { Navigate } from 'react-router-dom'
// 引入二级路由组件Cate1 和 Cate2
import Cate1 from './Cate1';
import Cate2 from './Cate2';
export default [
    {
        path: '/home',
        element: <Home />
    },
    {
        path: '/category',
        element: <Category />,
        // 定义二级路由规则
        children: [
            {
                path: '',  // 表示默认匹配的二级路由
                element: <Cate1></Cate1>
            },
            {
                path: 'cate1', // 不要加 / 
                element: <Cate1></Cate1>
            },
            {
                path: 'cate2',
                element: <Cate2></Cate2>
            }
        ]
    },
    {
        path: '/detail',
        element: <Deatil />
    },
    {
        path: '/',
        element: <Navigate to='/home' />
    }
]
```

在二级路由对应的一级路由组件中定义二级路由组件显示的坑

Catefory.tsx文件(一级路由组件)

```tsx
import { NavLink, Outlet } from "react-router-dom";


const Category = () => {
    return <div>
        分类
        <p>
            {/* 
				写法1: 二级路由的to属性可以直接写 to='cate1'
				写法2: 二级路由的to属性可以直接写 to='/category/cate2'
			*/}
            <NavLink to='cate1'>cate1</NavLink>
            <NavLink to='/category/cate2'>cate2</NavLink>
        </p>
        {/* 
           子路由组件匹配到的坑,二级路由的坑为Outlet 组件,这个和vue有区别
		*/}
        <Outlet></Outlet>
    </div >
}
export default Category;
```



### 08: 路由传参

案例: 首页 跳转到详情页

Home.tsx 组件

```tsx
import { useState } from 'react'
import { Link } from 'react-router-dom'
type Props = {};

export default function Home({ }: Props) {

    const [list, setlist] = useState([
        {
            id: 1,
            name: '张三'
        },
        {
            id: 2,
            name: '李四'
        },
        {
            id: 3,
            name: '王五'
        }
    ])


    return (
        <div>
            <div>首页</div>
            <ul>
                {list.map((item) => {
                    {/*第一种方式: 查询字符串方式*/}
                    {/* return <li key={item.id}><Link to=`/detail?id=${item.id}`>{item.name}</Link></li>*/}
                    
                    {/*第二种方式: restful风格 也有叫动态路由
                         这种方式需要修改路由配值表中的path/:id
					 */}
                    {/* return <li key={item.id}><Link to=`/detail/${item.id}`>{item.name}</Link></li>*/}
                    
                    {/*第三种方式; state 隐式传参
                     这里个v5有点区别, v5隐式传参都放在一个对象中 {pathname:url,state:{key:val}}
					*/}
                   return <li key={item.id}><Link to='/detail'  state={{id:item.id}}>{item.name}</Link></li>
                })}
            </ul>
        </div>

    )
}
```



### 09:编程式导航

Home.tsx 

```tsx

import { useState } from 'react'
// 01: 首先需要引入  useNavigate 这个钩子函数
import { Link, useNavigate } from 'react-router-dom'
type Props = {};

export default function Home({ }: Props) {

    const [list, setlist] = useState([
        {
            id: 1,
            name: '张三'
        },
        {
            id: 2,
            name: '李四'
        },
        {
            id: 3,
            name: '王五'
        }
    ])


    // 编程式导航函数
    // 02: 使用useNavigate 该函数创建一个对象
    const navigate = useNavigate()
    
    const gotoDetail = (item: any) => {
        // 03: 实现路由的跳转
        // 语法: navigate(path,配置对象) 配置对象配置的是state形式的传参,如果查询字符串参数直接拼接  			路径path 上
         navigate('/detail', {
            state: {
                id: item.id
            }
        })
    }
    return (
        <div>
            <div>首页</div>
            <ul>
                {list.map((item) => {
                    return <li key={item.id}><Link to='/detail?id=100'>{item.name}</Link>                    {/*通过点击button 实现编程式导航*/}
                   <button onClick={() => gotoDetail(item)}>查看详情</button></li >
                })}
            </ul >
        </div >

    )
}
```

Detail.tsx 页面

```tsx
import React from 'react'
// 01: 首先需要引入  useNavigate 这个钩子函数
import { Navigate, useParams, useLocation, useNavigate } from 'react-router-dom'
import { useState } from 'react'
type Props = {}


export default function Detail({ }: Props) {
    const [num, setnum] = useState(0);
    const par = useParams();
    
    // 02: 创建 navigate对象 
    const navigate = useNavigate();
    return (
        <div>
            详情页
            <button onClick={() => {
                //03:  返回上一页效果 下一页就传1
                navigate(-1) 
            }}>返回上一页</button>

            <p>{num}</p>
            {num == 2 ? <Navigate to='/home'></Navigate> : <></>}
            <button onClick={() => {
                setnum(num + 1)
            }}> +1</button>
        </div>
    )
}
```



























