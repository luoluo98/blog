# Prettier

prettier 是一个包含大量固有规范的代码格式化工具，支持多种前端开发语言以及 markdown、yaml，它能够保证代码呈现的一致性。

## 一、安装

```js
// npm
npm install --save-dev --save-exact prettier

// yarn
yarn add --dev --exact prettier
```

## 二、文件准备

- 创建配置文件

  - 在根目录下创建 `.prettierrc.js`文件

```
{}
```

- 创建忽略文件
  - 项目根目录下创建`.prettierignore`忽略文件

```
Base your .prettierignore on .gitignore and .eslintignore (if you have one).
将 .prettierignore 建立在 .gitignore 和 .eslintignore 上（如果有的话）
```

示例：

```shell
# 忽略文件夹:
build
coverage

# 忽略某一类型文件:
*.html
```

在`.prettierignore`中写入以下内容：

```js
# Ignore artifacts:
build
coverage

# Ignore all HTML files:
*.html
```

## 三、创建配置

### 3.1 创建格式化参考规则

1. 在 `.prettierrc.js`中写入以下内容：

```js
//此处的规则供参考，其中多半其实都是默认值，可以根据个人习惯改写

module.exports = {
  printWidth: 80, //单行长度
  tabWidth: 2, //缩进长度
  useTabs: false, //使用空格代替tab缩进
  semi: true, //句末使用分号
  singleQuote: true, //使用单引号
  quoteProps: "as-needed", //仅在必需时为对象的key添加引号
  jsxSingleQuote: true, // jsx中使用单引号
  trailingComma: "all", //多行时尽可能打印尾随逗号
  bracketSpacing: true, //在对象前后添加空格-eg: { foo: bar }
  jsxBracketSameLine: true, //多属性html标签的‘>’折行放置
  arrowParens: "always", //单参数箭头函数参数周围使用圆括号-eg: (x) => x
  requirePragma: false, //无需顶部注释即可格式化
  insertPragma: false, //在已被preitter格式化的文件顶部加上标注
  proseWrap: "preserve", //不知道怎么翻译
  htmlWhitespaceSensitivity: "ignore", //对HTML全局空白不敏感
  vueIndentScriptAndStyle: false, //不对vue中的script及style标签缩进
  endOfLine: "lf", //结束行形式
  embeddedLanguageFormatting: "auto", //对引用代码进行格式化
};
```

格式化规则还可以写在如下文件中（按优先级由高至低排列）：

1. 项目的 `package.json` 文件中
2. `.prettierrc.json` 或 `.prettierrc.yml` 文件中

```js
//格式示例
{
  "trailingComma": "es5",
  "tabWidth": 4,
  "semi": false,
  "singleQuote": true
}
```

3. `.prettierrc.js` 或 `prettier.config.cjs` 文件中

```js
//格式示例
module.exports = {
  trailingComma: "es5",
  tabWidth: 4,
  semi: false,
  singleQuote: true,
};
```

## 四、格式化文档

### 4.1 命令行格式化

1. 格式化全部文档

```js
npx prettier --write .
//或
yarn prettier --write .
```

2. 格式化指定文档

```js
npx prettier --write src/components/Button.js
//或
yarn prettier --write src/components/Button.js
```

3. 检查文档是否已格式化

```js
npx prettier --check .
//或
yarn prettier --check .
//检查指定文件同上
```

- `--check` 命令只会检查已经格式化的文件，而不会重写它们。

- `--write` 则会自动检查并排版。

### 4.2 利用编辑器插件进行格式化

在编辑器中集成 Prettier 是最广泛的使用方式。具体集成方式请查看各编辑器的 扩展/插件 使用说明。

`注意 ! 需要在每个项目中都安装 Prettier 插件，从而保证它们使用正确的版本。`

VSCode 扩展下载 Prettier - Code formatter

在编辑器中搜索相应的 Prettier 安装，即可运用编辑器快捷键进行格式化。

| 常见的编辑器 | 对应插件名                |
| ------------ | ------------------------- |
| VS Code      | Prettier - Code formatter |
| Emacs        | prettier-emacs            |
| Vim          | vim-prettier              |
| Sublime Text | JsPrettier                |
| Atom         | prettier-atom             |

- 重要提示：编辑器中的 Prettier 格式化规则也可在设置中编写，但实际执行时会以本地规则为优先。

### `4.3 集成在 ESLint 中`

ESlint 与 Prettier 可能会冲突，故需做如下设置：

1. 安装插件

```js
//1. 安装 eslint-config-prettier 插件
npm i -D eslint-config-prettier
```

2. 修改 eslint 配置文件

```json
// .eslintrc.json
{
  "extends": ["plugin:prettier/recommended"]
}
```

到这里就已经配置好了。

如果出现错误提示：Delete ␍ eslint prettier/prettier ，只需要在 .prettier.config.js 中加入

```js
{
  endOfLine: "auto";
}
```

### 4.4 集成在 git 生命周期中
