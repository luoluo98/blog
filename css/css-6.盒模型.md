# css 盒子模型

页面布局的三大核心：盒子模型、浮动、定位。

`看透网页布局的本质`：利用 css 摆盒子。

网页布局过程

1. 先准备好相关网页元素，网页元素基本都是盒子。
2. 利用 css 设置盒子样式，摆放到相应位置。
3. 往盒子里面装内容。

# 盒子模型的组成部分

所谓盒子模型，就是把 html 页面中的元素看做是一个矩形的盒子，也就是一个盛装内容的容器。
css 盒子模型本质上是一个盒子，封装周围的 html 元素，他包括**border 边框、margin 外边距、padding 内边距、和实际内容**。

## 一、边框 border

border 可以设置元素的边框。由三部分组成：边框宽度、边框样式、边框颜色。

| 属性         | 作用                  |
| ------------ | --------------------- |
| border-width | 定义边框粗细，单位 px |
| border-style | 边框的样式            |
| border-color | 边框颜色              |

边框简写：

border: 1px solid red;没有顺序

边框分开写：

border-top: 3px solid red;只设定上边框
border-bottom: 8px dashed pink;下边框

## 表格的细线边框

border-collapse 属性控制浏览器绘制表格边框的方式。他控制相邻单元格的边框。collapse 合并

`语法`

border-collapse: collapse;

**边框会影响盒子的实际大小**

解决方法：

1. 测量盒子大小的时候，不测量边框。
2. 如果测量的时候包含了边框，需要 width/height 减去边框的宽度。

## 二、内边距 padding

padding 属性用于设置内边距，即**边框与内容之间的距离**。

| 属性           | 作用     |
| -------------- | -------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

### padding 的复合属性

    padding 简写属性可以有1~4个值。

| 值的个数                     | 表达意思                                                                |
| ---------------------------- | ----------------------------------------------------------------------- |
| padding: 5px;                | 1 个值，代表上下左右都有 5 像素内边距                                   |
| padding: 5px 10px;           | 2 个值，代表上下内边距都是 5 像素，左右内边距是 10 像素                 |
| padding: 5px 10px 20px       | 3 个值，代表上内边距是 5 像素，左右内边距是 10 像素，下内边距是 20 像素 |
| padding: 5px 10px 20px 30px; | 4 个值，上是 5 像素，右是 10 像素，下是 20 像素，左是 30 像素。顺时针   |

- 内容和边框有了距离，添加了内边框
- padding 影响盒子的实际大小。  
  解决方法：让 width/height 减去多出来的内边距大小即可。

`padding不会撑开盒子的情况`

如果盒子本身没有指定 width/height 属性，则此时 padding 不会撑开盒子大小。

## 三、外边距 margin

外边距控制盒子与盒子之间的距离。

| 属性          | 作用     |
| ------------- | -------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

**margin 简写方式代表的意义和 padding 完全一致。**

外边距可以让**块级**盒子**水平居中**，但是必须满足两个条件：

1. 盒子必须指定了宽度 width
2. 盒子左右的外边距都设置为 auto

```
.header {width: 960px; margin: 0 auto;}
```

常见写法：

- margin-left: auto; margin-right: auto;
- margin: auto;
- margin:0 auto;

注意：以上方式是让块级元素水平居中，行内元素或者行内块元素水平居中给其父元素添加 text-align 即可。

## 外边距合并

使用 margin 定义块内元素的垂直外边距时，可能会出现外边距的合并。

1. 相邻块元素垂直外边距的合并
   当上下相邻的两个块元素（兄弟关系）相遇时，如果上面的元素有下外边距 margin-bottom，下面的元素有上外边距 margin-top，则他们之间的垂直间距不是 margin-bottom 和 margin-top 之和。**取两个值中的较大者这种现象被称为相邻块元素垂直外边距的合并**。
   （此时，尽量只给一个盒子添加 margin 值）
2. 嵌套块元素垂直外边距的塌陷
   对于两个嵌套关系（父子关系）的块级元素，父元素有上外边距的同时子元素上也有上外边距，此时父元素会塌陷较大的外边距值。取较大的值。

解决方案：

A.可以为父元素定义上边框。

B.可以为父元素定义上内边距。

C.可以为父元素添加 overflow：hidden。

## 清除内外边距

网页元素很多都带有默认的内外边距，而且不同浏览器默认的也不一致，因此我们在布局前，首先要清除下网页元素的内外边距。

```
* {
  padding: 0;   清除内边距
  margin: 0;    清除外边距
}
```

css 的第一行代码
`注意：行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距。但是转换为块级和行内块元素就可以了。`

## ps 基本操作

- 打开文件
- Ctrl+R：打开标尺，或者视图 → 标尺
- 右击标尺，单位改成像素
- ctrl+/- 放大缩小视图
- 按住空格，可以拖动图片
- 用选区拖动，可以测量大小
- Ctrl+d 取消选区，或者空白处点击一下

`去掉li前面的小圆点`

```
list-style: none;
```

## 四、圆角边框 \*

border-radius 属性用于设置元素的边框圆角。

`语法`

```
border-radius: length;
```

- radius 半径原理：圆与边框的交集形成圆角效果。
- 参数值可以为数值或百分比的形式。
- 如果是正方形，想要设置为一个圆，把数值修改为高度或宽度的一半即可，或者直接写 50%。
- 如果是矩形，设置为高度的一半即可。
- 该属性是一个简写属性，可以跟四个值，分别代表左上角、右上角、右下角、左下角。
- 分开写：border-top-left-radius、border-top-right-radius、border-bottom-right-radius、border-bottom-left-radius。

## 五、盒子阴影 \*

box-shadow 属性为盒子添加阴影。

`语法`

```
box-shadow: h-shadow blur spread color inset;
```

| 值       | 描述                                 |
| -------- | ------------------------------------ |
| h-shadow | 必需，水平阴影的位置，允许负值       |
| v-shadow | 必需，垂直阴影的位置，允许负值       |
| blur     | 可选，模糊距离                       |
| spread   | 可选，阴影的尺寸                     |
| color    | 可选，阴影的颜色，参考 css 颜色值 l  |
| inset    | 可选，将外部阴影 outset 改为内部阴影 |

- 默认的是外阴影 outset，但是不可以写这个单词，否则导致阴影无效。
- 盒子阴影不占用空间，不会影响其他盒子排列。

### 文字阴影

text-shadow 属性将阴影应用于文本。

`语法`

```
text-shadow: h-shadow v-shadow blur color;
```
