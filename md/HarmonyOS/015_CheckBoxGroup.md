# CheckBoxGroup

> 鸿蒙开发以身入局
> 声明式 UI，内有大乾坤
> 组件学习开始了
> 掌握 CheckBoxGroup 组件

- 复选框群组
- 用于控制多个复选框全选或者不全选状态

## 参数

> 参数形式 ： CheckboxGroup( options?: { group?: string } )
> 创建复选框群组，可以用于控制群组内的 CheckBox 成员 全选 或者 不全选
> 相同 group 的 CheckBox 和 CheckBoxGroup 为同一群组

| 参数名 | 参数类型 | 是否必填 | 参数描述 |
| ------ | -------- | -------- | -------- |
| group  | string   | 否       | 群组名称 |

## 相关属性

| 属性名         | 属性类型      | 默认值 | 描述             |
| -------------- | ------------- | ------ | ---------------- |
| selectAll      | boolean       | false  | 设置组内是否全选 |
| selectAllColor | ResourceColor | -      | 设置全选按钮颜色 |

## 事件

| 名称                                                         | 描述                                                             |
| ------------------------------------------------------------ | ---------------------------------------------------------------- |
| onChange( callback: ( event: CheckBoxGroupResult ) => void ) | CheckBoxGroup 的选中或者群组内 CheckBox 的选中状态发生改变时触发 |

### CheckboxGroupResult

| 名称   | 参数类型        | 描述                               |
| ------ | --------------- | ---------------------------------- |
| name   | `Array<string>` | 群组内 CheckBox 名称，数组形式返回 |
| status | SelectStatus    | 选中状态                           |

### SelectStatus

| 名称      | 描述                            |
| --------- | ------------------------------- |
| All（0）  | 群组内复选框 全部 为 选中状态   |
| Part（1） | 群组内复选框 部分 为 选中状态   |
| None（2） | 群组内复选框 全部 为 未选中状态 |

## 示例 1

> 只要 CheckBoxGroup 配置的 group 属性和 CheckBox 配置的 group 属性一致
> 此时就可以控制

```ts
@Entry
@Component
struct Index {
  @State count: number = 0

  build() {
    Column() {
      Row() {
        CheckboxGroup({ group: 'hobby' })

        Text('全选')
      }.margin(30)

      Row() {
        Checkbox({ name: 'a', group: 'hobby' })

        Text('篮球')

        Checkbox({ name: 'b', group: 'hobby' })

        Text('足球')

        Checkbox({ name: 'c', group: 'hobby' })

        Text('羽毛球')
      }
    }
  }
}
```

## 示例 2

> 给 CheckBoxGroup 绑定了 onChange 事件
> 那么 全选复选框 和 每一个选项复选框 发生变化的时候都会触发
> 回调函数内事件对象得到的信息
> name 属性为选中的复选框配置的 name 属性
> status 属性的值为选中状态

```ts
@Entry
@Component
struct Index {
  @State count: number = 0

  build() {
    Column() {
      Row() {
        CheckboxGroup({ group: 'hobby' })
          .onChange(ev => {
            console.log(JSON.stringify(ev))
          })

        Text('全选')
      }.margin(30)

      Row() {
        Checkbox({ name: 'a', group: 'hobby' })

        Text('篮球')

        Checkbox({ name: 'b', group: 'hobby' })

        Text('足球')

        Checkbox({ name: 'c', group: 'hobby' })

        Text('羽毛球')
      }
    }
  }
}
```
