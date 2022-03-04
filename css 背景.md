# css 背景

背景属性可以设置背景颜色、背景图片、背景平铺、背景图片的位置、背景图像固定等。

## 背景颜色

- background-color 属性定义了元素的背景颜色。
- 语法格式：background-color: 颜色值（transparent 透明）；一般情况下元素背景颜色默认值是 transparent。

## 背景图片

- background-image 属性描述了元素的背景图像，实际开发常见于 logo 或者一些装饰性的小图片或者是超大的背景图片，优点是非常便于控制位置，（精灵图也是一种运用场景）。
- 语法格式：background-image: none/url

| 参数值 | 作用                           |
| ------ | ------------------------------ |
| none   | 无背景图（默认的）             |
| url    | 使用绝对或相对地址制定背景图像 |

## 背景平铺

- 使用 background-repeat 属性在 html 页面上对背景图像进行平铺。
- 语法格式：background-repeat: repeat/ no-repeat/ repeat-x/ repeat-y；

| 参数值    | 作用                                 |
| --------- | ------------------------------------ |
| repeat    | 背景图片在纵向和横向上平铺（默认的） |
| no-repeat | 背景图片不平铺                       |
| repeat-x  | 背景图片在横向上平铺                 |
| repeat-y  | 背景图片在纵向上平铺                 |

## 背景图片位置

- background-position 属性可以改变图片在背景中的位置。
- 语法格式：background-position: x y; （参数 x、y 代表坐标，可以使用**方位名词**或者**精确单位**）

| 参数值   | 作用                                         |
| -------- | -------------------------------------------- |
| length   | 百分数/由浮点数字和单位标识符组成的长度值    |
| position | top/center/bottom/left/center/right 方位名词 |

`参数是方位名词`

- 如果指定的两个值都是方位名词，则两个值前后顺序无关，比如 left top 和 top left 效果一致。
- 如果只制定了一个方位名词，另一个值省略，则第二个值默认居中对齐。
