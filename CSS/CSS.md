#CSS

### 清除浮动？

- 给需清除浮动元素添加如下属性

```css
.clearfix:after{
  content:'';
  display:block;
  clear:both;
}
```

 - 修改父元素的 owerflow 属性

```js
 owerflow:auto|hidden
```

### CSS选择器的优先级

```js
!import          infinity
行间样式           1000
ID                 100
class|属性|伪类     10
标签|伪元素         1
通配符              0
```

### BFC是什么？

Block Formating Context 块级格式化上下文

下列方式会创建块级格式化上下文：
1. 浮动元素 float:left;
2. 绝对定位元素 position:absolute|fixed;
3. 行内块元素 display:inline-block;
4. overflow的值不为visible时。

### 弹性盒子

display:flex

主轴 main axis

交叉轴 cross axis 

**容器**

- flex-direction 决定主轴的方向
 row|row-reserve|column|column-reverse
- flex-wrap 是否换行
 nowrap|wrap|wrap-reverse
- justify-content 定义了项目在主轴的对齐方式
 flex-start|flex-end|space-between|space-around|center
- align-items 定义了项目在交叉轴的对齐方式
 stretch|flex-start|flex-end|center|baseline
- align-content 定义了项目在多根轴线的对齐方式 

**项目**

- order 定义了项目在容器中的排列顺序，数值越小排列越前。
- flex `<flex-grow><flex-shrink><flex-basis>`
- flex-grow 定义项目放大的比例。
- flex-shrink 定义项目缩小的比例。
- flex-basis 在分配多余空间之前，项目占主轴的空间。
- align-self 允许单个项目有与其他不一样的对齐方式。

flex: auto  1 1 auto

flex: 1  1 1 0%

flex: 0  0 1 0%

### 移动端 border:1px

```css
:after{
content: ''; 
position: absolute;
top: 0; 
left: 0; 
border: 1px solid #000; 
-webkit-box-sizing: border-box;
 box-sizing: border-box;
 width: 200%; 
height: 200%; 
-webkit-transform: scale(0.5); 
transform: scale(0.5); 
-webkit-transform-origin: left top;
 transform-origin: left top;
}
```