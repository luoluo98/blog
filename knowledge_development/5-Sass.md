# 一、SASS 语法

### 简介

Sass 是一款成熟、稳定、强大的专业级 CSS 扩展语言，他是一款强化 CSS 的辅助工具，在 css 语法的基础上增加了变量（variables）、嵌套（nestedrules）、混合（mixins）、导入（inline imports）等高级功能，让 css 更加强大和优雅。使用 sass 以及 sass 的样式库（如 compass）有助于更好的组织管理样式文件，以及更高效的开发项目。

Sass 的优势：

1. Sass 完全兼容所有版本的 css。
2. 特性丰富，sass 拥有比其他任何 css 廓镇语言更多的功能和特性。
3. 技术成熟、功能强大。
4. 行业认可，越来越多的人使用。
5. 社区庞大、大多数科技企业和成百上千开发者为 sass 提供支持。
6. 有无数框架使用 sass 建构，如 Compass、Bootstrap、Boerbon 和 Susy。

此外，Sass 为 css 引入了变量的概念，在 sass 中编写样式代码时，可以把反复使用的 css 属性值定义成一个变量，这样就不用重复的书写此属性值，在使用此属性值时只需通过变量名在不同的代码位置来引用它。或者，对于仅使用过一次的属性值，可以赋予其一个易懂的变量名，让人很直观的看出这个属性值的用途。

### 什么是预处理器

是在程序源文件被编译之前根据预处理指令对程序源文件进行处理的程序。说白了，预处理器只不过是一个文本替换工具而已。CSS 预处理器则是通过将有特殊语法和指令的源代码处理成浏览器可使用的 CSS 文件的程序。

## 1. 安装

npm install --save-dev sass-loader

npm install --save-dev node-sass

安装完以后，在 vue 页面的 style 中添加：

```xml
<style lang="scss" scoped></style>
```

添加后，先执行写一段嵌套的 css，如果有效果就 OK 了。

如果报错，在 package.json 检查下 sass-loader 版本，可能是版本过高，可以进行低版本安装即可。

安装前，先卸载 sass-loader：

npm uninstall sass-loader

卸载完成，重新安装低版本：

npm install sass-loader@7.3.1

## 2. 特性

## 2.1 变量

变量用来存储需要在 css 中复用的信息，例如颜色和字体。

`sass通过 $ 符号去声明一个变量`

```js
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

// 等价于
// body {
//   font: 100% Helvetica, sans-serif;
//   color: #333;
// }
```

## 2.2 嵌套

sass 允许开发人员以嵌套的方式使用 css，但是**过度的使用嵌套会让产生的 css 难以维护，因此是一种不好的实践**。

例如：

```css
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li {
    display: inline-block;
  }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

上面的 ul、li、a 选择器都被嵌套在 nav 选择器当中使用，这是一种书写更高可读性 css 良好的方式，编译后：

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

## 2.3 引入

SASS 能够将代码分割为多个片段，并以 underscore 风格的下划线作为其命名前缀（\_partial.scss），SASS 会通过这些下划线来辨别哪些文件是 SASS 片段，并且不让片段内容直接生成为 CSS 文件，`从而只是在使用@import 指令的位置被导入`。CSS 原生的@import 会通过额外的 HTTP 请求获取引入的样式片段，而 SASS 的@import 则会直接将这些引入的片段合并至当前 CSS 文件，并且不会产生新的 HTTP 请求。下面例子中的代码，将会在 base.scss 文件当中引入\_reset.scss 片断。

```js
// _reset.scss
html,
body,
ul,
ol {
  margin: 0;
  padding: 0;
}

// base.scss
@import "reset";
body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

SASS 中引入片段时，可以**缺省使用文件扩展名**，因此上面的代码中通过 @import 'reset' 引入，编译后生成的代码如下：

```js
html,
body,
ul,
ol {
  margin: 0;
  padding: 0;
}

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

SASS 片断使用下划线前缀命名，主要用于 SASS 命令行工具 watch 指定目录源码的场景；如果使用 Webpack 等打包工具则毋须顾虑该问题，CSS 样式将会通过 Webpack 加载器，按照 ES6 风格的 import 或 Webpack 插件 extract-text-webpack-plugin 进行打包和模块化。

## 2.4 混合

混合（Mixin）用来分`组那些需要在页面中复用的 css 声明`，开发人员可以通过向 mixin 传递变量参数来让代码更加灵活，改特效在添加浏览器兼容性前缀的时候非常有用，SASS 目前使用@mixin name 指令来进行混合操作。

```js
@mixin border-radius($radius) {
  border-radius: $radius;
  -ms-border-radius: $radius;
  -moz-border-radius: $radius;
  -webkit-border-radius: $radius;
}

.box {
  @include border-radius(10px);
}
```

上面的代码建立了一个为 border-radius 的 Mixin，并传递了一个变量$radius 作为参数，然后在后续代码中通过@include border-radius(10px)使用该 Mixin，最终编译结果如下：

```js
.box {
  border-radius: 10px;
  -ms-border-radius: 10px;
  -moz-border-radius: 10px;
  -webkit-border-radius: 10px;
}
```

## 2.5 继承

继承是 SASS 中一个非常重要的特性，可以通过`@extend`指令在选择器之间复用 css 属性，并且不会产生冗余的代码。

下面例子通过 SASS 提供的继承机制建议一系列样式：

```js
// 这段代码不会被输出到最终生成的CSS文件，因为它没有被任何代码所继承。
%other-styles {
  display: flex;
  flex-wrap: wrap;
}

// 下面代码会正常输出到生成的CSS文件，因为它被其接下来的代码所继承。
%message-common {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-common;
}

.success {
  @extend %message-common;
  border-color: green;
}

.error {
  @extend %message-common;
  border-color: red;
}

.warning {
  @extend %message-common;
  border-color: yellow;
}
```

上面代码 .message 中的 css 属性应用到了 .success、 .error、 .warning 上面，魔法将会发生在最终生成的 css 当中。这种方式能够避免在 HTML 元素上书写多个 class 选择器，最终生成的 css 样式如下：

```js
.message,
.success,
.error,
.warning {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

## 2.6 操作符

SASS 提供了标准的算数运算符，例如 +、 -、 \*、 /、 %。

在下面的例子中，尝试在 aside 和 article 选择器当中对宽度进行简单的计算。

```js
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

上面代码以 960px 为基准建立了简单的流式网格布局，SASS 提供的算术运算符让开发人员可以更容易的将像素值转换为百分比，最终生成的 css 样式如下：

```js
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complementary"] {
  float: right;
  width: 31.25%;
}
```

## 3. CSS 扩展

SASS 当中有两种可用的语法，一种是 Scss（即 SASS 化的 CSS），这是一种类 CSS 的语法，以.scss 作为源码文件后缀。另一种是 Sass，提供了比较简明的 CSS 书写方式，通过缩进而非括号来标识嵌套的选择器，以及使用换行而非分号来分隔各个属性，并使用.sass 作为代码文件的后缀。

Sass 和 Scss 两种风格可以通过 `sass-convert` 命令行工具进行相互转换：

```
➜  sass-convert test.scss test.sass

➜  sass-convert test.sass test.scss
```

日常开发环境下，出于前端开发的使用习惯，更多会选择 Scss 语法。

## 4. 嵌套规则

Scss 允许 css 规则嵌套使用，父子规则将会呈现`包含选择器`的关系。

例如:

```js
/*===== SCSS =====*/
#main p {
  color: #00ff00;
  width: 97%;

  .redbox {
    background-color: #ff0000;
    color: #000000;
  }
}

/*===== CSS =====*/
#main p {
  color: #00ff00;
  width: 97%;
}
#main p .redbox {
  background-color: #ff0000;
  color: #000000;
}
```

这样可以避免重复的使用父级选择器，从而达到简化 css 代码结构的目的。

例如：

```js
/*===== SCSS =====*/
#main {
  width: 97%;

  p,
  div {
    font-size: 2em;
    a {
      font-weight: bold;
    }
  }

  pre {
    font-size: 3em;
  }
}

/*===== CSS =====*/
#main {
  width: 97%;
}
#main p,
#main div {
  font-size: 2em;
}
#main p a,
#main div a {
  font-weight: bold;
}
#main pre {
  font-size: 3em;
}
```

## 5. 引用父级选择器 &

Scss 使用 `&`关键字在 css 规则中引用`父级选择器`。

例如在嵌套使用伪类选择器的场景下：

```js
/*===== SCSS =====*/
a {
  font-weight: bold;
  text-decoration: none;
  &:hover {
    text-decoration: underline;
  }
  body.firefox & {
    font-weight: normal;
  }
}

/*===== CSS =====*/
a {
  font-weight: bold;
  text-decoration: none;
}
a:hover {
  text-decoration: underline;
}
body.firefox a {
  font-weight: normal;
}
```

无论 css 规则嵌套的深度如何，关键字`&`都会使用父级选择器`级联替换`全部其出现的位置：

```js
/*===== SCSS =====*/
#main {
  color: black;
  a {
    font-weight: bold;
    &:hover {
      color: red;
    }
  }
}

/*===== CSS =====*/
#main {
  color: black;
}
#main a {
  font-weight: bold;
}
#main a:hover {
  color: red;
}
```

`&`必须出现在复合选择器开头的位置，后面再连接自定义的后缀。

例如：

```js
/*===== SCSS =====*/
#main {
  color: black;
  &-sidebar {
    border: 1px solid;
  }
}

/*===== CSS =====*/
#main {
  color: black;
}
#main-sidebar {
  border: 1px solid;
}
```

如果在父级选择器不存在的场景使用&，Scss 预处理器会报出错误信息。

## 6. 嵌套属性

CSS 许多属性都位于相同的命名空间（例如 font-family、font-size、font-weight 都位于 font 命名空间下），`Scss 当中只需要编写命名空间一次，后续嵌套的子属性都将会位于该命名空间之下`。

请看下面的代码：

```js
/*===== SCSS =====*/
.demo {
  // 命令空间后带有冒号:
  font: {
    family: fantasy;
    size: 30em;
    weight: bold;
  }
}

/*===== CSS =====*/
.demo {
  font-family: fantasy;
  font-size: 30em;
  font-weight: bold;
}
```

命令空间上可以直接书写 CSS 简写属性，但是日常开发中建议直接书写 CSS 简写属性，尽量保持 CSS 语法的一致性。

```js
.demo {
  font: 20px/24px fantasy {
    weight: bold;
  }
}

.demo {
  font: 20px/24px fantasy;
  font-weight: bold;
}
```

## 7. 占位符选择器 %

占位符选择器与 id 或者 class 选择器的用法类似，只是 `#` 和 `.` 需要替换成为 `%`，占位符选择器必须通过 `@extend`指令进行调用。

详情参考 @extend 章节。

## 8. 注释

SASS 支持标准 css 的多行注释 /\* \*/ 和单行注释 //，其中，多行注释会完整输出到编译后的 css 文件，而单行注释不会。

```js
/* 多行注释
 * 会原样输出到CSS文件 */
body {
  color: black;
}

// 单行注释不会出现在输出的CSS当中
a {
  color: green;
}
```

编译成为 css 的结果如下：

```js
@charset "UTF-8";
/* 多行注释
 * 会原样输出到CSS文件 */
body {
  color: black;
}

a {
  color: green;
}
```

- 插值语句 `#{$variable}`可以应用在多行注释当中。

```js
$version: "3.5.5";
/* 这段CSS是通过SASS #{$version}生成的。*/
```

编译成为 css 结果：

```js
@charset "UTF-8";
/* 这段CSS是通过SASS 3.5.5生成的。*/
```

当注释中出现中文时，SASS 会自动在生成的 CSS 文件头部添加@charset "UTF-8";。

# 二、SassScript

SassScript 是 Sass 提供的一系列 CSS 语法扩展，允许在任意 CSS 属性上书写变量、计算以及函数。SassScript 还可以通过 interpolation 插值生成选择器和属性名，这在编写 mixins 时非常有用。

## 1. 变量

sassscript 通过 `$` 关键字声明和使用一个变量。

```js
/*===== SCSS =====*/
$width: 8rem; // 声明变量

#main {
  width: $width; // 使用变量
}

/*===== CSS =====*/
#main {
  width: 8rem;
}
```

SassScript 变量支持块级作用域，嵌套规则当中声明的变量都是`局部变量`，可以通过`!global` 将局部变量声明为`全局变量`。

```js
/*===== SCSS =====*/
#menu {
  $width: 5rem !global;
  width: $width;
}

#sidebar {
  width: $width;
}

/*===== CSS =====*/
#menu {
  width: 5rem;
}

#sidebar {
  width: 5rem;
}
```

## 2. 数据类型

SassScript 支持 8 种数据类型：

1. **数值 number**：1.2,13,10px

2. **颜色 color**：blue、#04a3f9、rgba(255, 0, 0, 0.5)
3. **布尔值 bollean**：true、false
4. **空值 null**：null
5. **字符串 string**：有或没有引号，"menu"、'sidebar'、navbar
6. **数组 list**：由空格或逗号分隔，1.5em 1em 0 2em, Helvetica, Arial, sans-serif
7. **对象 map**：(key1: value1、key2: value2)
8. **函数 function**。

Scss 支持所有其它类型的 CSS 属性值，例如 Unicode 字符或者!important 声明。`Scss 并没有对这些类型进行特殊处理，仅仅将其视为没有使用引号的字符串。`

## 2.1 String

CSS 支持两种类型的字符串：

1. 使用单双引号的："Lucida Grande"， 'http://sass-lang.com';
2. 没有使用引号的：sans-serif，bold;

SassScript 能够自动识别这 2 种字符串，Scss 当中使用了哪种类型字符串，就会在生成的 CSS 文件原样输出该类型字符串。

但是这里存在一个意外，当使用#{}插值语法的时候，使用引号的字符串会失去引号，这样可以便于 mixin 中使用选择器的名称。

```js
/*===== SCSS =====*/
@mixin firefox-message($selector) {
  body.firefox #{$selector}:before {
    content: "Hi, Firefox users!";
  }
}

@include firefox-message(".header");

/*===== CSS =====*/
body.firefox .header:before {
  content: "Hi, Firefox users!";
}
```

## 3. List

sass 当中可以通过 list 来表达 css 声明的值，例如：margin: 10px 15px 0 0 或 font-face: Helvetica, Arial, sans-serif。SASS 中的 List 是一系列由空格或者逗号分隔的值，实际上单个值也可以作为一个 List，即只拥有一项元素的 List。

List 本身的作用不多，但是通过 SassScript 的 List 函数可以让它发挥更大的作用。`nth 函数可以操作 List 中的每一项元素，join 函数能够合并多个 List 在一起，append 函数能够向 List 添加一项元素，而@each 指令可以为 List 当中每项元素添加样式。`

除了包含简单的值，List 还可以包含其它的 List，例如拥有 2 项元素的 List1px 2px, 5px 6px 包含了 1px 2px 和 5px 6px 两个 List，如果内部的 List 和外部 List 拥有相同的分隔符，那么必须使用圆括号()标识内部 List 的起始和结束位置，例如(1px 2px) (5px 6px)。

当 List 被转换为普通 CSS 的时候，SASS 不会添加其它圆括号，这意味(1px 2px) (5px 6px)和 1px 2px 5px 6px 变成 CSS 的时候结果是相同的。但是无论如何，它们在 SASS 里是不相同的，(1px 2px) (5px 6px)是一个拥有 2 个 List 元素的 List，1px 2px 5px 6px 则是拥有 4 个普通元素的 List。

List 里同样可以不包含任何元素，这样的 List 会被表现为()（同样可以认为其是一个空的 map），它们并不能直接输出为 CSS。例如 SASS 编译 font-family: ()时将会发生错误。如果 List 包含一个空的 List()或者其本身为空值 null，例如对于 1px 2px () 3px 或 1px 2px null 3px 当中的 null 和()将会在输出的 CSS 当中被移除。

逗号分隔的 List 尾部可能还跟着一个逗号，这在一些特殊的情况下比较有用，例如展现一个只有 1 个元素的 List，(1,)就是一个包含了元素 1 的 List，而(1 2 3,)是一个逗号分隔的 List，这个 List 又包含着一个空格分隔的 List，空格分隔的 List 拥有着 1、2、3 三个元素。

`List 也可以通过方括号[]编写，方括号编写的 List 用来在 CSS Grid 布局中表达行的名称，但是它同样也可以用于纯 SASS 代码，同样也可以通过逗号或者空格进行分隔。`

## 4. Map

**Map 提供了 key 与 value 之间的关联关系，并且能够通过 key 找到相应的 value。**他们可以容易的搜集 value 到一个被命名的分组，然后动态的操作这些分组。它们在 CSS 中没有直接并行的概念，尽管在句法上与媒体查询表达式相似：

```js
$map: (key1: value1, key2: value2, key3: value3);
```

不同与前面提到的 List，`Map 必须总是被圆括号()包裹，并且必须使用逗号,进行分隔`。Map 中的 key 与 value 可以是任意的 SassScript 对象，一个 Map 可以只拥有 1 个给定的 key 和对应的 value（可以是一个 List），但是一个给定的 value 可能会关联到多个 key。

和 List 一样，Map 大部分情况下也需要通过 SassScript 函数对其进行操作。其中 `map-get 函数负责查询 Map 中的 value，map-merge 函数添加 value 至 Map，指令@each 可以被用来为 Map 中的每个 key 和 value 添加样式`。Map 中键值对的顺序总是与其被建立的时候相同。

**List 能够使用的位置，Map 也能正常使用**。当通过一个 List 函数进行使用的时候，Map 被视为拥有键值对的 List。例如：(key1: value1, key2: value2)在 List 函数当中将被视为一个嵌套的 List（即 key1 value1, key2 value2）。但是，值得注意的是，List 并不能相反的被视为一个 Map，尽管空的 List 将会抛出错误。()即可以被视为一个没有 key 和 value 的 Map，也可以视为没有元素项的 List.

注意 **Map 的 key 可以是任意的 Sass 数据类型（即使是 Map），并且声明 Map 的语法允许任意的 SassScript 表达式**，该表达式将会被解析以决定 key 的值。

Map 并不能被转换为普通 CSS，在 CSS 函数中使用它作为变量和参数的值将会导致错误。这种情况下，建议使用 inspect($value)函数为 Map 的调试生成一个有用的字符串输出。

## 5. Color

任何 CSS 颜色表达式都返回一个 SassScript 色彩值，这包括大量被命名的颜色，它们与未引用的字符串是不可区分的。

在压缩输出模式下，Sass 将输出颜色的最小的 CSS 表示。例如，在压缩模式下，#FF0000 将输出为红色 red，而#FFEBCD 将会输出为白杏色 blanchedalmond。

用户使用被命名的颜色时，经常会遇到一个问题：在。。。SASS 喜欢在其他输出模式中键入相同的输出格式。。。之后，一个颜色会被插值到一个选择器，代码压缩时这将会变成不合法的语法。要避免这个问题，在被命名颜色用于构建选择器的时候，总是需要为它们添加引号。

## 6. First-Class Function

`get-function($function-name)`将会返回一个函数引用，该函数能够被传递到 `call($function, $args...)`并得到调用。First-Class 函数不能直接用于 CSS 输出，任何这样的尝试都将会引发错误。

## 7. 运算

SassScript 所有数据类型都支持`== 或 !=`逻辑运算，但是每种数据类型还拥有自己的运算方式。

### 7.1 数值运算

数值类型支持常见的数值运算+、-、_、/、%，并在必要时在不同的`绝对`数值单位之间转换。SASS 数学函数会在计算过程中保留单位，这意味不同单位不能在一起进行运算（例如 px 和 em），两个相同单位的数值相乘后可能会造成运算结果的单位重复，例如：10px _ 10px == 100px _ px，显然这里 px _ px 是一个无效单位，此时 SASS 会由于使用非法单位而报错。

`关系运算符<、 >、<=、>=同样支持数值类型，而相等运算符==, !=则可以被 SASS 所有类型所支持`。

### 7.2 除法与标识符 /

CSS 允许在属性值中使用/作为分隔数字的手段，SassScript 为 CSS 属性语法提供了一个扩展，允许/作为除法运算符来使用。如果 SassScript 当中 2 个数值通过 / 进行分隔，那么它们的计算结果将会出现在生成的 CSS 当中。

/被解释为除法的情况主要有以下 3 种：

1. 如果值及其部分被保存到一个变量，或者通过一个函数返回。
2. 值被圆括号()包裹，除非这些圆括号在一个 List 的外面并且该地，
3. 如果指定值被用于其它算术表达式。

```js
/*===== SCSS =====*/
p {
  font: 10px/8px; // 普通CSS，未进行除法运算。
  $width: 1000px;
  width: $width/2; // 使用一个变量，进行了除法运算。
  width: round(1.5) / 2; // 使用一个函数，进行了除法运算。
  height: (500px/2); // 使用圆括号, 进行了除法运算。
  margin-left: 5px + 8px/2px; // 使用了加号+, 进行了除法运算。
  font: (italic bold 10px/8px); // 在List当中，圆括号不会进行计算。
}

/*===== CSS =====*/
p {
  font: 10px/8px;
  width: 500px;
  height: 250px;
  margin-left: 9px;
}
```

如果需要在普通 CSS 当中使用变量，可以通过 **#{}** 去插入它们，例如:

```js
/*===== SCSS =====*/
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}

/*===== CSS =====*/
p {
  font: 12px/30px;
}

```

### 7.3 减法、负数与标识符 -

中划线-在 CSS 和 SASS 中有许多不同的意义，`可以表达一个减法运算符（5px - 3px），一个负数的开始（-23px），一元负值运算符（-$var），或者一个标识符的组成部分（font-weight）`，大部分情况下何时何地使用是比较清晰的，但也存在一些例外情况，为了避免这些例外需要**遵循如下原则**：

1. 进行减法运算的时候，符号两则尽量保留空格。

2. 作为负值标识或一元负值运算符时，需要在数值或变量前包含一个空格。

3. 如果是在空格分隔的 List 中，就用括号将一元负值运算符括起来，就像 10px (-$var)一样。

不同含义的表达遵循如下的优先次序：

- 表达式中使用字母时，a-1 表示一个没有引号的字符串，其值为"a-1"。但是唯一的例外在于单位的使用，比如 5px-3px 与 5px - 3px 是等效的，其执行结果为 2px。
- 两个数字之间没有空格意味着这是一个减法，所以 1-2 与 1 - 2 作用相同。
- 将-放置在数字开头表示这是一个负数，所以 1 -2 是一个包含了 1 和-2 两个元素的 List。
- 两个数字之间不考虑空格表示这是减法，所以 1 -$var 和 1 - $var 相同。
- 放置在一个值的前面表示这是一元负值运算符，该运算符会返回当前数值的负值。

```js
➜  hank ✗ sass -i
>> a-1
"a-1"
>> 5px-3px
2px
>> 1 -2
(1 -2)
>> 1-2
-1
```

### 7.4 颜色运算

SASS 中所有的数学运算都分段的支持颜色值，即分别对红、绿、蓝依次进行计算。

```js
/*===== SCSS =====*/
p {
  color: #010203 + #040506;
}
```

在执行完 01 + 04 = 05，02 + 05 = 07，03 + 06 = 09 运算后编译为下面的 CSS：

```js
/*===== CSS =====*/
p {
  color: #050709;
}
```

通常来说，使用**颜色函数**比使用**颜色运算**能更加有效的达到相同目的。颜色运算同样也可以作用于数值与颜色值，并且依然是分段进行的，例如：

```js
p {
  color: #010203 * 2;
}
```

进行 01 \* 2 = 02，02 \* 2 = 04，03 \* 2 = 06 运算后编译为下面 CSS：

```js
p {
  color: #020406;
}
```

注意，带有 alpha 通道的颜色（使用 rgba 或 hsla 函数创建的颜色）必须具有相同的 alpha 值单位，以便与其执行颜色运算。

```js
/*===== SCSS =====*/
p {
  color: rgba(255, 0, 0, 0.75) + rgba(0, 255, 0, 0.75);
}

/*===== CSS =====*/
p {
  color: rgba(255, 255, 0, 0.75);
}
```

颜色的 alpha 通道可以通过 `opacify` 和 `transparentize` 函数进行修改，例如：

```js
/*===== SCSS =====*/
$translucent-red: rgba(255, 0, 0, 0.5);
p {
  color: opacify($translucent-red, 0.3); // 增加alpha通道值
  background-color: transparentize($translucent-red, 0.25); // 减少alpha通道值
}

/*===== CSS =====*/
p {
  color: rgba(255, 0, 0, 0.8);
  background-color: rgba(255, 0, 0, 0.25);
}
```

IE 的 filter 滤镜要求所有颜色都包括 alpha 层，并且严格采用#AABBCCDD 格式，SASS 提供了 `ie_hex_str` 函数能够更加简便的对颜色值进行转换，例如：

```js
/*===== SCSS =====*/
$translucent-red: rgba(255, 0, 0, 0.5);
$green: #00ff00;
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#{ie-hex-str($green)}', endColorstr='#{ie-hex-str($translucent-red)}');
}

/*===== CSS =====*/
div {
  filter: progid:DXImageTransform.Microsoft.gradient(enabled='false', startColorstr='#FF00FF00', endColorstr='#80FF0000');
}
```

### 7.5 字符串运算

SassScript 使用 `+` 运算符执行**字符串连接**操作。

```js
/*===== SCSS =====*/
p {
  cursor: e + -resize;
}

/*===== CSS =====*/
p {
  cursor: e-resize;
}
```

注意:

如果有引号的字符串被添加到没有引号的字符串（即带引号的字符串在+的左侧），生成的字符串将会是有引号的。同理，如果将没有引号的字符串添加到有引号的字符串（即没有引号的字符串在+的左侧），则结果为没有引号的字符串。

```js
/*===== SCSS =====*/
p:before {
  content: "Foo " + Bar; // 有引号字符串 + 无引号字符串 = 有引号字符串
  font-family: sans- + "serif"; // 无引号字符串 + 有引号字符串 = 无引号字符串
}

/*===== CSS =====*/
p:before {
  content: "Foo Bar";
  font-family: sans-serif;
}
```

默认情况下，临近的两个 CSS 属性值可以通过空格进行连接。

```js
/*===== SCSS =====*/
p {
  margin: 3px + 4px auto;
}

/*===== CSS =====*/
p {
  margin: 7px auto;
}
```

如果字符串插值 `#{}` 中的变量是空值，则插值表达式的结果将被视为空字符串。

```js
/*===== SCSS =====*/
$value: null; // 空值
p:before {
  content: "I ate #{$value} pies!";
}

/*===== CSS =====*/
p:before {
  content: "I ate  pies!";
}
```

### 7.6 布尔运算

SassScript 当中布尔类型的值支持`与 and、或 or、非 not `运算

```js
/*===== SCSS =====*/
$year: 2018;
$moth: 8;
$day: 6;
$name: Hank;
#app {
  @if ($year > 2010 and $moth==8 or $day==6 not $name==Hank) {
    color: blue;
  } @else {
    color: red;
  }
}

/*===== CSS =====*/
#app {
  color: blue;
}
```

### 7.7 List 运算

数组不支持任何特定的计算方式，只能通过 list 函数进行操作，下面代码是一个使用了 `hsl($hue, $saturation, $lightness)`函数的例子：

```js
/*===== SCSS =====*/
a {
  color: hsl(120deg, 100%, 50%);
}

/*===== CSS =====*/
a {
  color: lime;
}
```

### 7.8 圆括号

SASS 中可以`通过圆括号{}来调整运算的优先级。`

```js
/*===== SCSS =====*/
p {
  width: 1em + (2em * 3);
}

/*===== CSS =====*/
p {
  width: 7em;
}
```

### 7.9 函数

SassScript 内置了许多有用的函数，可以通过普通的 CSS 语句进行调用。

```js
/*===== SCSS =====*/
p {
  color: hsl(0, 100%, 30%);
}

/*===== CSS =====*/
p {
  color: #990000;
}
```

### 7.10 关键词参数

Sass 函数还允许通过关键词参数(即带有键名的参数)进行调用，上面的例子也可以改写为下面代码：

```js
/*===== SCSS =====*/
p {
  color: hsl($hue: 0, $saturation: 100%, $lightness: 30%);
}

/*===== CSS =====*/
p {
  color: #990000;
}
```

虽然这不是很简洁，但可以使样式表更加容易阅读。同时还能够让函数提供更灵活的接口，提供多个参数的同时，并不会变得难以调用。

命名参数能够以任何顺序进行传递，而带有默认值的参数可以省略掉。如果命名参数是变量名，那么下划线与破折号可以互换使用。

可以通过 `Sass::Script::Functions` 查看 SASS 函数及其参数名称的完整清单，也可以通过 Ruby 进行自定义。

### 7.11 插值 #{ }

开发人员可以通过`插值（interpolation）#{}`在选择器和属性名称中使用 SassScript 变量。

```js
/*===== SCSS =====*/
$name: uinika;
$attr: border;
p.#{$name} {
  #{$attr}-color: blue;
}

/*===== CSS =====*/
p.uinika {
  border-color: blue;
}
```

还可以使用#{}将 SassScript 放入属性值，大多数情况下这种方式并不优于变量，但是使用#{}意味着其附近的任意操作都将被视为普通 CSS，例如：

```js
/*===== SCSS =====*/
p {
  $font-size: 12px;
  $line-height: 30px;
  font: #{$font-size}/#{$line-height};
}

/*===== CSS =====*/
p {
  font: 12px/30px;
}
```

### 7.12 SassScript 中的 &

如同使用选择器一样，SassScript 中的`&引用着当前的父选择器列表`（一个由逗号分隔 List 作为元素再通过空格进行分隔的 List）。

```js
.navbar.text .sidebar.word,
.menu.item {
  $selector: &;
}
```

上面代码中$selector 的值为`((".navbar.text" ".sidebar.word"), ".menu.item")`，这里的复合选择器使用了引号"标识它是一个字符串，但是真实情况它可能是没有引号的。即使其父选择器并没有包含逗号和空格，&依然会拥有两层嵌套，因此它能够被持续的访问。

当父级选择器列表不存在的时候，&运算符的值为 null，使用 mixin 当中可以通过该特性判断父选择器列表是否存在。

```js
@mixin does-parent-exist {
  @if & {
    &:hover {
      color: red;
    }
  } @else {
    a {
      color: red;
    }
  }
}
```

### 7.13 变量默认值 !default

可以在变量后添加`!default`声明，如果变量已被赋值就不会再被重新进行赋值，如果变量未被赋值，则会
被赋予新值。

```js
/*===== SCSS =====*/
$content: "First content";
$content: "Second content?" !default;
$new_content: "First time reference" !default;
#main {
  content: $content;
  new-content: $new_content;
}

/*===== CSS =====*/
#main {
  content: "First content";
  new-content: "First time reference";
}
```

如果变量的值是 null，则会被!default 视为没有赋值。

```js
/*===== SCSS =====*/
$content: null;
$content: "Non-null content" !default;
#main {
  content: $content;
}

/*===== CSS =====*/
#main {
  content: "Non-null content";
}
```

### 7.14 @ 规则与指令

sass 支持所有 css3 的 @-Rules 规则，以及 sass 特有的 #Directive 指令。

### 7.15 @import / #import

sass 拓展了 css 的 `@import` 允许其导入.scss 或.sass 文件，导入的文件将合并、编译到一个 css 文件，文件中的变量和 mixin 肚饿可以在导入的主文件当中使用。

sass 会基于当前目录查找其他文件，以及 Rack、Rails 或者 Merb 下的 sass 文件目录。可以通过 `:load_paths` 或 `--load-path`选项指定额外的搜索目录。

`@import`通过指定路径以及文件名来引入 sass 文件，但是在下面 4 中情况当中，`@import`仅被编译为普通的 css 语句：

1. 如果文件拓展名为`.css`
2. 如果文件名以 `http://`开头
3. 如果文件名是`url()`
4. 如果`@import`包含媒体查询语句

```js
/*===== SCSS =====*/
@import "app.css";
@import "pp" screen;
@import "https://uinika.github.io/app";
@import url(app);

/*===== CSS =====*/
@import url(app.css);
@import "pp" screen;
@import "https://uinika.github.io/app";
@import url(app);
```

除上述四种情况外，文件拓展名为 `.scss`或`.sass`的情况都会进行预编译处理，即使**文件扩展名缺省**依然能够正确的进行导入。

```js
// 下面2种导入方式等效
@import 'base';
@import 'base.scss;
```

sass 允许在一个@import 语句内同时导入读个文件。

```js
@import "base", "reset", "app";
```

@import 语句内可以有限制的使用`#{}`插值，即不能动态的导入基于变量的 sass 文件，只能用于标准 css 的`@import url("");`导入方式。

```js
/*===== SCSS =====*/
$family: unquote("Droid+Sans");
@import url("http://fonts.googleapis.com/css?family=#{$family}");

/*===== CSS =====*/
@import url("http://fonts.googleapis.com/css?family=Droid+Sans");
```

### 7.16 Partial

如果你引入一个 SCSS 或 Sass 文件，但并不希望它们编译成 CSS 文件，那么可以在文件名开头添加一个`下划线_`，这样 Sass 将不会把该文件编译成 CSS 文件，`然后引入文件的时候不需要添加下划线`。例如：你拥有一个\_colors.scss，但是 Sass 并不会创建\_colors.css 文件，然后，不过仍然可以通过`@import "colors";`语句引入`_colors.scss`文件。

注意，不能在同一目录中包含相同名称的 Partial 和非 Partial；例如：**\_colors.scss 不能与 colors.scss 在相同目录下并存**。

### 7.17 嵌套的 @import

大多数情况下，在代码的顶层使用 `@import`导入是最为常用，但是我们依然可以在`css规则`和`@media规则`中使用；其效用如同基本的`@import`，仍然会去包含引入文件的内容。

```js
/*===== SCSS =====*/
// 文件demo.scss包含如下代码
.demo {
  color: red;
}

#app {
  @import "demo"; // 在文件app.scss中引入demo.scss
}

/*===== CSS =====*/
#app .demo {
  color: red;
}
```

- 指令只能用于文档的基本级别，在一个嵌套上下文当中被@import 引入的文件里，诸如@mixin 或者@charset 是不被允许的。

- 不能在 mixin 或者 控制指令 当中嵌套使用@import。

### 7.18 @media / #media
