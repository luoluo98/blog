# Emmet 语法

emmet 语法前身是 Zen coding，它使用缩写，来提高 html/css 的编写速度，Vscode 内部已经集成该语法。

### 一. 快速生成 HTML 结构语法

1. 生成标签，直接输入标签名按 Tab 键即可。
2. 如果想要生成多个相同的标签，加上*就可以了。例，div*3，就可以快速生成三个 div。
3. 如果有父子级关系的标签，可以用>，例，ul> li 就可以。
4. 如果有兄弟关系的标签，用+，例 div+p。
5. 如果生成带有类名或者 id 名字的，直接写.demo 或者#two tab 键就可以。
6. 如果生成的 div 类名是有顺序的，可以用自增符号$。
7. 如果想要在生成标签内部写内容可以用{}表示。

### 二. 快速生成 css 样式语法

css 基本采取简写的形式即可。

1. 比如 w200 按 tab 可以生成 width: 200px;
2. 比如 lh26 按 tab 可以直接生成 line-height: 26px;

### 三. 快速格式化代码

1. shitf+alt+F 或者右键 格式化文档
2. 设置，当保存页面的时候自动格式化代码

- 文件----首选项----设置
- 搜索 emmet.include
- 在 setting.json 下【用户】中添加一下语句：
  `"editor.formatOneType":true,`
  `"editor.formatOneSave":true`
  `只需要设置一次即可`
