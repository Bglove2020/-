# CSS

## 预备知识Flex布局

语法教程地址：https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
实战教程地址：https://www.ruanyifeng.com/blog/2015/07/flex-examples.html

## CSS中的小问题

### 图片垂直居中
让图片在一个设定了宽高的div中居中
1. 给img设置属性`vertical-align: middle` **(注意是给img标签设定属性)**
2. 外部div设置`display=flex`，并设置主轴和交叉轴居中
3. 使用外部div的`padding`+图片的`height`等于外部div的高

### html、body设置背景颜色后，背景色蔓延到整个视窗
1. 当html不设置背景时，body的背景将作为整个浏览器的背景色，因此会铺满全屏，而不是作为body标签自己的背景色。 当设置html背景色后，body的背景色就变成了自己的背景色，此时html的背景色将被浏览器获取成为浏览器的背景色
2. 虽然html或body的背景色占据了整个视窗，但是html或body的元素大小并不是整个视窗。在未设置元素高度且body中没内容时，html没有内外边距，body有默认的外边距，即整个html结构的高度仅为body的外边距宽度。

### 设置html标签的高度等于视窗高度
html标签是整个页面的根元素，也是最外层标签。当`height:100%`时，高度为父元素**内容宽度**的100%，当没有父元素时为整个浏览器视窗高度的100%。
```html
<style>
    html{
        height: 100%;
    }
</style>
```

### 一般H5端页面写法
一般我们书写的html元素都是在body中，我们如果想让整个页面背景大小铺满整个视窗，需要如下设置。
```html
<style>
    html,body{
        height: 100%;
        margin: 0;
    }
</style>
```
**注意：body自带margin，如果不将margin设置为0，会发现页面出现了滚动条，因为body的高度超过了html的高度**



### 
https://cloud.tencent.com/developer/article/1635235


### <i>标签
<i>标签类似于<span>标签，通过`font-size=10px`来设置宽高，同时设置`line-height`等于外部div宽度来实现居中





### 语法注意点

* 任何一个容器都可以指定为 Flex 布局
* 设为 Flex 布局以后，子元素的float、clear和vertical-align属性将失效
* 容器内元素称为项目。
* 容器属性，容器默认主轴排列方式为水平排列，从左开始，主轴不换行。**容器默认项目交叉轴对其为拉伸，会自动将项目的高度拉伸至和容器相同**
* 项目属性，主要设置flex，flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。flex-grow为1表示项目是否在主轴上进行拉伸(当设置为0时，项目在主轴的长度为元素长度)，flex-shrink为1表示是否在主轴空间不足时缩小。flex-basis可设置为固定数值、百分比和auto。在渲染一个容器时，优先计算其内部设置了flex-basis属性值和flex-grow为0的项目，再把剩余的主轴空间用其余项目填充。
* 容器的padding和margin要自己设置。
* 若要实现项目内部文字的水平垂直居中，将文字再包裹进一个容器内，将项目设置为flexbox再进行居中。
* flex容器的align-content是同时设置内部项目两个方向上的对齐方式，但是flex容器默认是项目高度拉伸至容器高度，横向排列，不换行。这样就只有一个轴线，无法直接设置align-content，必须先设置允许换行才可以设置align-content。也可以使用align-items直接垂直对齐。

## 预备知识Sass

Sass 是一款强化 CSS 的辅助工具，它在 CSS 语法的基础上增加了变量 (variables)、嵌套 (nested rules)、混合 (mixins)、导入 (inline imports) 等高级功能

### 引入变量

Sass可以将反复使用的css属性值定义成变量使用`$`标识一个变量，形如`$nav-color: #F90;`

1. **定义在css文件内部。当变量定义在某个选择器内部时，此变量仅在选择器内部有效。当变量定义在选择器外部时，此变量全局有效。**
2. **变量可以出现在任何css属性的标准值可存在的地方，变量的值可以嵌套引用变量**

### 嵌套

标签、类、id、伪类、组合、群组选择器都可以嵌套

1. 直接在一个选择器内部写入其子选择器即可嵌套
2. 在解析嵌套选择器时，父子选择器之间用空格隔开，如果嵌套的子选择器是伪类选择器就会出现错误，因为伪类选择器中间没空格。可以在嵌套的伪类选择器前写入&符号，这样在解析时会直接拼接。
3. 嵌套的选择器中，父选择器和组选择器都可以是群组选择器和组合选择器。
4. 属性嵌套，对于形如"padding-top,padding-bottom"的属性可以写成,嵌套时加上:。






```css
div{
    padding:{
    top: 1px;
    bottom: 2px;
}
}
```

---

## Bulma的实现依赖：

1. 所有 Bulma 组件都使用 CSS 变量（也称为 CSS 自定义属性）进行样式设置。CSS变量是用户在任何的CSS选择器内部以--开头声明的变量`-- variable_name=合法CSS样式值`，CSS变量具有作用域并受级联的约束，子标签集成父标签的所有CSS变量。当需要使用CSS变量时只需要使用`var(--variable_name)`函数即可获取CSS变量中的值。Bulma的CSS变量都定义在根伪类 :root选择器(相当于html{}元素选择器)中，并在对应的类选择器中重写CSS变量，这使得可以在任何标签中使用Bulma的CSS变量。
2.
