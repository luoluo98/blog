## css字体属性
css fonts（字体）属性用于定义字体系列、大小、粗细和文字样式（比如斜体）。

### 1. 字体系列
css使用font-family属性定义文本的字体系列。
```
p {font-family:"微软雅黑"}
div {font-family:Arial,"Microsoft yahei","微软雅黑"}
```
* 各个字体之间必须用英文状态下的逗号隔开。
* 一般情况下，如果有空格隔开的多个单词组成的字体，加引号。
* 尽量使用系统默认自带字体，保证在任何用户的浏览器中都能正确显示。
* 最常见的几个字体：body {font-family:'Microsoft YaHei',tahoma,arial,'Hiragino Sans GB';}