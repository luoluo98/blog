# 一、精灵图 sprites

一个网页中往往会应用很多小的背景图像作为修饰，当网页中图像过多时，服务器就会频繁的接收和发送请求图片，造成服务器请求压力过大，会降低页面加载速度。

`目的：`_为了有效的接收和发送请求的次数，提高页面加载速度_，出现了 css 精灵技术，也称为 css sprites、css 雪碧。

`核心原理：将网页中的一些小背景图像整合到一张大图中，这样服务器只需要请求一次就行了。`

### 1.1 精灵图的使用（原理）

1. 主要针对于背景图片的使用。整合到一张大的图片中。
2. 移动图片位置，此时可以使用 background-position。
3. 移动距离就是这个目标图片的 x 和 y 坐标。注意网页中的坐标有所不同。
4. 往上往左移动是负值。
5. 使用精灵图的时候需要精确测量，每个背景图片的大小和位置。

### 1.2 精灵图的使用（代码）

```
 background: url(./images/sprites.png) no-repeat -182px 0;
```

### 1.3 精灵图缺点

1. 图片文件较大。
2. 图片本身放大和缩小会失真。
3. 一旦图片制作完想要更换非常复杂。

# 二、字体图标---iconfont

主要用于显示网页中通用、常用的一些小图标。

字体图标可以为前端工程师提供一种方便高效的图标使用方式，**展示的是图标，本质属于字体**。

### 2.1 字体图标优点

```
· 轻量级：一个图标字体要比一系列的图像小。一旦字体加载了，图标马上就会渲染出来，减少了服务器请求。
· 灵活性：本质是文字，可以随意更改颜色、大小、产生阴影、透明效果、旋转等。
· 兼容性：几乎支持所有浏览器。
```

- 字体图标不能替代精灵技术，只是对工作中图标部分技术的提升和优化。
- 如果遇到一些结构样式比较简单的小图标，就用字体图标。反之用精灵图。

### 2.2 字体图标的使用

1. 字体图标下载

- 推荐网站：iconmoon 字库：http://iconmoon.io（外网）
- 阿里 iconfont 字库：http://www.iconfont.cn

2. 字体图标引入

- 把下载包里面的 fonts 文件夹放入页面根目录下面。
- 在 Css 样式中全局声明字体：把这些字体文件通过 css 引入到页面。一定注意文件路径问题。

```
@font-face {
  font-family: 'icomoon';
  src:  url('fonts/icomoon.eot?hy17px');
  src:  url('fonts/icomoon.eot?hy17px#iefix') format('embedded-opentype'),
    url('fonts/icomoon.ttf?hy17px') format('truetype'),
    url('fonts/icomoon.woff?hy17px') format('woff'),
    url('fonts/icomoon.svg?hy17px#icomoon') format('svg');
  font-weight: normal;
  font-style: normal;
  font-display: block;
}
```

- html 标签内添加小图标。

3. 字体图标追加

- 如果工作中 ，原来的字体图标不够用，需要添加新的字体图标到原来的字体文件中。
  _把压缩包里面的 selection.json 重新上传，选中自己想要的新图标，重新下载并替换原来的文件即可。_

# 三、css 三角

```
div {
  width: 0;
  height: 0;
  line-height: 0;
  font-size: 0;
  border: 50px solid transparent;
  border-left-color: blue;
}
```

# 四、css 用户界面样式

所谓界面样式，就是更改一些用户操作样式，以便于提高更好的用户体验。

## 4.1 更改用户 【鼠标样式-cursor】

`li {cursor: poninter;}`

**设置或检索在对象上移动的鼠标指针采用何种系统预定义的光标形状。**

| 属性值      | 描述      |
| ----------- | --------- |
| default     | 小白 默认 |
| pointer     | 小手      |
| move        | 移动      |
| text        | 文本      |
| not-allowed | 禁止      |

## 4.2 取消表单轮廓--outline: 0;

`给表单添加 outline: 0; 或者 outline: none;样式之后就可以去掉默认边框。`

## 4.3 防止拖拽文本域--resize

`textarea {resize: none;}`

# 五、vertical-align 属性应用

使用场景：经常用于设置图片或表单（行内块元素）和文字垂直对齐。

用于设置一个元素的垂直对齐方式，**只针对于行内元素或者行内块元素有效。**
`vertical-align: baseline/top/middle/bottom`

| 值       | 描述                                 |
| -------- | ------------------------------------ |
| baseline | 默认。元素放置在父元素的基线上       |
| top      | 把元素的顶端与行中最高元素的顶端对齐 |
| middle   | 把此元素放置在父元素的中部           |
| bottom   | 把元素的顶端与行中最低的元素顶端对齐 |

- 图片、表单都属于行内块元素，默认的 vertical-align 是基线对齐。

## 5.1 解决图片底部默认空白缝隙问题

图片底侧经常会有一个空白缝隙，原因是行内块元素会和文字基线对齐。

`解决方案：`

1. 给图片添加 vertical-align: middle/top/bottom 等。（提倡）
2. 把图片转换为块级元素 display: block;

# 六、溢出的文字省略号显示

## 6.1 单行文本溢出显示省略号

必须满足三个条件

1. 先强制一行内文本显示

   `white-space: nowrap; 默认normal自动换行`

2. 超出的部分隐藏

   `overflow: hidden;`

3. 文字用省略号替代超出的部分

   `text-overflow: ellipsis;`

## 6.2 多行文本溢出显示省略号

多行文本溢出显示省略号，有较大兼容性问题，适合于 webkit 浏览器或者移动端。

```
overflow: hidden;
text-overflow: ellipsis;
（弹性伸缩盒子模型显示）
display：-webkit-box；
（限制在一个块元素显示的文本的行数）
-webkit-line-clamp：2；
（设置或检索伸缩盒对象的子元素的排列方式）
-webkit-box-orient：vertical；
```

- 推荐后台人员做此效果。

# 七、常见布局技巧

## 7.1 margin 负值的运用--练习 101

1. 让每个盒子 margin 王左侧移动-1px 正好压住相邻盒子边框。
2. 鼠标经过某个盒子的时候，提高当前盒子的层级即可（如果没有定位，则加相对定位（保留位置），如果有定位，则加 z-index）。

## 7.2 文字围绕浮动元素--练习 102

## 7.3 行内块巧妙运用--练习 103

## 7.4 css 三角巧妙运用

`1.css三角强化`--练习 104

```
      .box1 {
        width: 0;
        height: 0;
        /* 1.只保留右边的边框有颜色 */
        border-color: transparent red transparent transparent;
        /* 2.样式都是solid */
        border-style: solid;
        /* 3.上边框宽度要大，右边框宽度稍小，其余边框改为0 */
        border-width: 100px 50px 0 0;
      }
```

# 八、css 初始化

不同浏览器对有些标签的默认值是不同的，为了消除不同浏览器对 HTML 文本呈现的差异，照顾浏览器的兼容，需要对 css 进行初始化。

- CSS 初始化是指重设浏览器的样式。（也称为 CSS reset）
- 每个网页都必须先进性 css 初始化。

```
· Unicode 编码字体：
把中文字体的名称用相应的Unicode编码来代替，这样就可以有效的避免浏览器解释css代码的时候出现乱码的问题。

例如：
黑体--\9ED1\4F53
宋体--\5B8B\4F53
微软雅黑--\5FAE\8F6F\96C5\9ED1
```
