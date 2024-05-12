# Less 必知必会

> Less 的使用：<https://less.bootcss.com/#-functions->

## 一、Less 的介绍

Less 是一个较为流行的 css 预处理语言。支持变量、混合(Mixin)、函数、计算、循环等特点。由于 Less 是预处理语言，所以不能直接运行，需要先进行编译。

凡是能用 css 编写的效果，都可以用 Less 编写。Less 中支持所有的 css 的语法，向下兼容。

常见预处理语言：LESS SASS stylus

## 二、Less 中变量的使用(Variables)

使用@符号声明一个变量

1. 变量作为 css 的属性值使用

```less
@width: 1200px;
@height: 50px;
.box1 {
  width: @width;
  height: @height;
}
```

编译后

```css
.box1 {
  width: 1200px;
  height: 50px;
}
```

2. 变量作为类名使用（需要将变量名加上大括号）

```less
@selector:box2;
.@{selector}{
width:@width;
height:@height;
}
```

编译后

```css
.box2 {
  width: 1200px;
  height: 100px;
}
```

3. 变量作为路径使用（需要将变量名加上大括号）

```less
@imgs: '../ES6/大转盘抽奖/img';

.box3 {
  width: @width;
  height: @height;
  background: url('@{imgs}/bg_lottery.png');
}
```

编译后

```css
.box3 {
  width: 1200px;
  height: 100px;
  background: url('../ES6/大转盘抽奖/img/bg_lottery.png');
}
```

4. 变量作为属性使用

```less
@bg: background;
.box4 {
  width: @width;
  height: @height;
  @{bg}: green;
}
```

编译后

```css
.box4 {
  width: 1200px;
  height: 100px;
  background: green;
}
```

5. 可变变量的使用（了解）

```less
@a: '测试';
@b: 'a';
.box4::after {
  content: @@b;
}
```

编译后

```css
.box4::after {
  content: '测试';
}
```

## 三、Less 的 extend 伪类

extend 是一个 Less 的伪类，他会合并它所在的选择和它匹配到的引用

示例一：

```less
.box5 ul li {
  //&代表父级元素
  &:extend(.inline);
  background: blue;
}

.inline {
  width: @width;
  height: @height;
}
```

编译后

```css
.box5 ul li {
  background: blue;
}
.inline,
.box5 ul li {
  width: 1200px;
  height: 100px;
}
```

示例二：

```less
.banner {
  color: red;
}

.nav:extend(.banner) {
  //background 是.nav 类名独有的 css 样式
  background: green;
}
```

编译后

```css
.banner,
.nav {
  color: red;
}

/* 单独生成一个.nav 的样式 他自己独有一个 background */
.nav {
  background: green;
}
```

注意：extend 会在选择器之间精准匹配

## 四、混合 Mixins

1. 混合'类'选择器或者'id'选择器（类选择器和 id 选择器的样式仍然能够正常显示）

```less
//混合‘类选择’和'id'选择器
.color,
#color {
  width: @width;
  height: @height;
  color: red;
}

.box6 {
  //直接使用'类选择器'
  .color();
}
.box7 {
  #color();
}
```

注意：当你调用混合集时，小括号可加可不加
编译后

```css
/* .color 和#color 设置的样式依然 */
.color,
#color {
  width: 1200px;
  height: 100px;
  color: red;
}
.box6 {
  width: 1200px;
  height: 100px;
  color: red;
}
.box7 {
  width: 1200px;
  height: 100px;
  color: red;
}
```

2. 创建混合集，但是不输出样式

方法：在混合集的名字后面加一个小括号

```less
.radius() {
  -webkit-border-radius: 30px;
  -moz-border-radius: 30px;
  -ms-border-radius: 30px;
  -o-border-radius: 30px;
  border-radius: 30px;
}

.box8 {
  .radius();
}
```

编译后

```css
.box8 {
  -webkit-border-radius: 30px;
  -moz-border-radius: 30px;
  -ms-border-radius: 30px;
  -o-border-radius: 30px;
  border-radius: 30px;
}
```

3. 混合集包含各种选择器

```less
.my-mixins() {
  &:hover {
    background: green;
  }
}
.box9 {
  .my-mixins();
}
```

编译后

```css
.box9:hover {
  background: green;
}
```

4. 混合集内若设置!important,则所有的属性都继承!important

```less
.foo() {
  width: 1200px;
  height: 100px;
  background: pink;
}

.box10 {
  .foo() !important;
}
```

编译后

```css
.box10 {
  width: 1200px !important;
  height: 100px !important;
  background: pink !important;
}
```

5. mixins 接收参数（将接收的参数传递给代码块）

```less
.border(@color) {
  border: 1px solid @color;
}

.box11 {
  .border(green);
}
```

编译后

```css
.box11 {
  border: 1px solid red;
}
```

6. mixins 接受多个参数,多个参数之间采用逗号连接

```less
.linear(@position,@start,@end) {
  background: -webkit-linear-gradient(@position, @start, @end);
  background: -moz-linear-gradient(@position, @start, @end);
  background: -o-linear-gradient(@position, @start, @end);
  background: -ms-linear-gradient(@position, @start, @end);
  background: linear-gradient(@position, @start, @end);
}
.box12 {
  .linear(top,red,orange);
}
```

编译后

```less
.box12 {
  background: -webkit-linear-gradient(top, red, orange);
  background: -moz-linear-gradient(top, red, orange);
  background: -o-linear-gradient(top, red, orange);
  background: -ms-linear-gradient(top, red, orange);
  background: linear-gradient(top, red, orange);
}
```

7. mixins 中的 when 的使用

```less
.mixin(@w) {
  width: @w;
}

//监听参数的情况
.mixin(@w) when (@w>=500px) {
  height: 600px;
}

.mixin(@w) when (@w<500px) {
  height: 300px;
}

.box16 {
  // .mixin(300px)
  .mixin(500px);
}
```

编译后

```css
.box16 {
  width: 500px;
  height: 600px;
}
```

8. 循环的使用

```less
.loop(@counter) when (@counter > 0) {
  .loop((@counter - 1)); // next iteration
  width: (10px \* @counter); // code for each iteration
}

div {
  .loop(5); // launch the loop
}
```

编译后

```css
div {
  width: 10px;
  width: 20px;
  width: 30px;
  width: 40px;
  width: 50px;
}
```

## 五、Less 的嵌套

嵌套：选择器之间的嵌套，可以很大程度的减少代码量

1. 选择器的嵌套

```less
header {
  width: @width;
  height: @height;
  ul {
    li {
      width: 300px;
      height: @height;
      background: lightblue;
      a {
        color: black;
      }
    }
  }
}
```

编译后

```css
header {
  width: 1200px;
  height: 100px;
}
header ul li {
  width: 300px;
  height: 100px;
  background: lightblue;
}
header ul li a {
  color: black;
}
```

2. 父级选择器

```less
header {
  ul {
    li {
      //&为父级选择器
      &:hover {
        background: pink;
      }
    }
  }
}
```

编译后

```css
header ul li:hover {
  background: pink;
}
```

3. 改变选择器的顺序(不推荐)

```less
ul {
  li {
    //此时的&代表的是 ul li
    .banner & {
      color: red;
    }
  }
}
```

编译后

```css
.banner ul li {
  color: red;
}
```

4. 组合所有的选择器列表（了解）

```less
div,
p {
  border: 1px solid black;
  & & {
    border-top: 0;
  }
}
```

编译后

```css
div,
p {
  border: 1px solid black;
}
div div,
div p,
p div,
p p {
  border-top: 0;
}
```

## 六、运算

算术运算符 +、-、\*、/ 可以对任何数字、颜色或变量进行运算

```less
ul {
  @width: 1200px;
  @margin: 5px;
  width: @width;
  li {
    width: @width / 4-2\* @margin;
    margin: @margin;
  }
}
```

编译后

```css
ul {
  width: 1200px;
}
ul li {
  width: 290px;
  margin: 5px;
}
```

注意：

1、运算时以最左侧的单位为准。运算时最好单位统一

2、若属性值之间有‘/’,则 less 默认当作除法运算

## 七、转义字符

转义前样式

```less
.box13 {
  border-radius: 30px/50px;
}
```

编译为

```css
.box13 {
  border-radius: 0.6px;
}
```

转义后的写法：在 less 中'/'会被当作除法

```less
.box13 {
  border-radius: ~'30px/50px';
  font: ~'30px/1.5 微软雅黑';
}
```

编译为

```css
.box13 {
  border-radius: 30px/50px;
  font: 30px/1.5 微软雅黑;
}
```

## 八、函数

Less 内置了多种函数用于转换颜色、处理字符串、算术运算等

<https://less.bootcss.com/functions/>

## 九、命名空间和访问符

为了提供一些封装的目的，你希望对混合（mixins）进行分组

将一些混合（mixins）和变量置于 #bundle 之下，为了以后方便重用或分发

```less
#bundle() {
  .border(@c) {
    border: 1px solid @c;
  }
  color: blue;
}

.box14 {
  //映射
  color: #bundle[color];
  //访问 mixins
  #bundle.border(black);
}
```

编译后

```css
.box14 {
  color: blue;
  border: 1px solid black;
}
```

## 十、作用域

Less 中的作用域与 CSS 中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

```less
@color: blue;
.box15 {
  color: @color;
  p {
    @color: red;
    color: @color;
  }
}
p {
  @color: red;
}
```

编译后

```css
.box15 {
  color: blue;
}
.box15 p {
  color: red;
}
```

## 十一、注释

方式一：
`//` 双斜杠注释（该注释内容不会被编译到 css 中）

方式二：
`/* */` 该注释的内容会解析到 css

## 十二、导入 import

在标准的 css 中，@import 必须放置在其他类型的规则之前

在 Less 中@import 语法可以放置在任意的位置

语法：@import "文件路径"

```less
/*导入的test.less的内容*/
@import './test';
```

注意：

1、less 的@import 的导入可以放置在任意位置

2、若导入的文件为.less 后缀，则可以省略(其他文件后缀不能省略)

3、同一个 less 文件只会被导入一次
