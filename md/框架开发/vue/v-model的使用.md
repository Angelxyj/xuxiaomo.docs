# v-model 的使用

### vue2.x 中 v-model

```html
<ChildComponent v-model = "title />
<!-- 实际上是这样的 -->
<ChildComponent :value="title" @input="title = $event" />
```

也就是传递一个属性 value，然后接收一个 input 事件。

问题： 那就是传递下去的必须是 value 值，接收的也必须是 input 事件

解决： Vue2.2 中，引入了 model 组件选项，也即是说你可以通过 model 来指定 v-model 绑定的值和属性

```html
<ChildComponent v-model="title" />
```

子组件这样设置的：

```js
export default {
  model: {
    prop: "title", // v-model绑定的属性名称
    event: "change", // v-model绑定的事件
  },
  props: {
    value: String, // value跟v-model无关
    title: {
      // title是跟v-model绑定的属性
      type: String,
      default: "Default title",
    },
  },
};
```

### vue3.x 中 v-model

vue3 中就实现了可以直接使用另外的属性和方法，而不需要去通过 model 进行定义功能

v-model 绑定的不再是 value，而是 modelValue，接收的方法也不再是 input，而是 update:modelValue。使用方法如下：

```html
<!-- 简写 -->
<ChildComponent v-model="title">
  <!-- 实际上是这样的 -->
  <ChildComponent
    :modelValue="title"
    @update:modelValue="title = $event"
  ></ChildComponent
></ChildComponent>
```

```js
export default defineComponent({
    name:"ValidateInput",
    props:{
        modelValue:String,   // v-model绑定的属性值
    },
    setup(){
        const updateValue = (e: KeyboardEvent) => {
          context.emit("update:modelValue",targetValue);   // 传递的方法
        }
    }
}

```

也就是说 vue3 中，value 改成了 modelValue，input 方法了改成 update:modelValue。

### 更换 v-model 的参数

vue3 中使用了 modelValue 来替代 value，但是 modelValue 不太具备可读性，在子组件的 props 中看到这个都不知道是什么

```html
<ChildComponent v-model:title="title" />
```

```js
export default defineComponent({
    name:"ValidateInput",
    props:{
        // modelValue:String,
        title:String,   // title替代了modelValue
    },
    setup(){
        const updateValue = (e: KeyboardEvent) => {
        //   context.emit("update:modelValue",targetValue);   // 传递的方法
          context.emit("update:title",targetValue);   // 传递的方法
        }
    }
}

```

### 思考

v-model 是双向绑定吗？

v-model 是单向数据流吗？

什么是单项数据流？
父组件可以向值组件传递数据,并且改变值组件的值,而子组件不能改变父组件传递给它的 prop 属性，官方推荐的做法是它抛出事件，通知父组件自行改变绑定的值。
