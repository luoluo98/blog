# Eslint

1. 什么是 eslint

2. 为什么使用 eslint？

3. 快速上手

4. 使用 eslint 检查代码

   - 4.1 在命令行中使用
   - 4.2 在 vscode 中使用

5. eslint 配置

- 5.1 语言环境配置

  - 5.1.1 环境 env
  - 5.1.2 全局变量 globals
  - 5.1.3 解析选项 parserOptions
  - 5.1.4 使用其他解析器 parser

- 5.2 规则配置 rules

- 5.3 使用插件 plugins

  - 5.3.1 插件的命名
  - 5.3.2 插件能带来什么

- 5.4 继承配置 extends
- 5.5 通过 Glob 匹配应用配置 overrides
- 5.6 忽略文件 ignorePatterns
- 5.7 使用注释进行配置
- 5.8 配置层级与规则合并

  - 5.8.1 配置文件层级
  - 5.8.2 配置的优先级
  - 5.8.3 规则的覆盖

6. 其他资源

7. 常用规则

## 1. 什么是 eslint

Lint（Linter）是一种静态代码分析工具，用于标记代码中某些编码错误】风格问题和不具结构性（易导致 bug）的代码。简单理解就是一个**代码检查器**，检查目标代码是否符合语法和规定的风格习惯。ESLint 是基于 ECMAScript/JavaScript 语法的 Lint，能够：

- 查出 JavaScript 代码语法问题
- 根据配置的规则，标记不符合规范的代码
- 自动修复一些结构、风格问题

ESLint 的特点是：`灵活、高度自定义`。用户可以通过多种方式配置在项目中应用的规则和其他配置，也可以自定义自己的规则，还可以通过插件的方式拓展功能。

## 2. 为什么使用 eslint

- 对个人：

  - 避免代码中的语法 bug，结构性问题
  - 格式化代码、自动美化代码

- 对团队：
  - 统一团队、项目中不同人的代码风格，降低维护成本
  - （已经有既定的代码规范的团队）约束团队成员使用统一的规范

现在，即时团队内没有形成统一的规范，在工程化项目中大部分会使用 eslint 来帮助检查、统一项目中的代码。

## 3. 快速上手

**前提：Node.js（>12.0.0)且项目目录中存在 package.json 文件。**

1. 安装（不推荐全局安装）

```shell
npm install --save-install eslint
```

2. 初始化

```shell
npx eslint --init

# 注：npx表示从当前路径下查找命令，即./node_modules/.bin/eslint --init
```

ESLint 会询问一系列问题，根据你的回答生成对应的 `.eslintrc.*`配置文件。

```json
/* 
$ npx eslint --init
✔ How would you like to use ESLint? · style
✔ What type of modules does your project use? · esm
✔ Which framework does your project use? · react
✔ Does your project use TypeScript? · No / Yes
✔ Where does your code run? · browser
✔ How would you like to define a style for your project? · guide
✔ Which style guide do you want to follow? · standard
✔ What format do you want your config file to be in? · JSON
*/

// 上面的回答对应下面的 .eslint.json 文件

{
  "env": {
    "browser": true,
    "es2021": true
  },
  "extends": ["plugin:react/recommended", "standard"],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaFeatures": {
      "jsx": true
    },
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["react", "@typescript-eslint"],
  "rules": {}
}
```

3. 配置你的规则（参照后面的配置部分）
4. 使用

（配置内容较多，而且简单上手无需太深入配置，后面先说 eslint 的使用，再总结配置内容）

## 4. 使用 ESLint 检查代码

### 4.1 在命令行中使用

**注：现在很少在命令行使用 eslint，一般都通过编辑器插件的方式与编辑器配合使用，实现在编辑的过程中实时检查。了解即可。**

- 检查并打印发现的问题：`npx eslint [file|dir|glob]*`，如:

```shell
# 检查多个文件
npx eslint file1.js file2.js

# 使用 glob 正则，检查目录下所有文件
npx eslint lib/**
```

- 使用 `--fix`选项自动 fix 柯秀芳问题，如：

```shell
# index.js 中可自动修复的问题会被修复并忽略
npx eslint --fix index.js
```

### 4.2 在 vscode 中使用

**注：除了 vscode，其他编辑器也有对应的 eslint 集成方式，参看 ESLint Docs：Intergrations。**

首先，安装 vscode eslint 插件。需要注意，`插件并不内置eslint核心代码`，而是自动查找项目中的 eslint 库，在用户每次输入时调用 `lint` 命令，并在编辑器中标记代码问题。文档这样说明：

```
The extension uses the ESLint library installed in the opened workspace folder. If the folder doesn't provide one the extension looks for a global install version.
（该扩展使用安装在打开的工作区文件夹中的 ESLint 库。如果该文件夹没有提供一个，则扩展程序会查找全局安装版本。）
```

这样的好处是，编辑器能兼容不同项目中不同版本的 eslint，同时，能保证同一项目中，团队成员的 eslint 版本一致。

如果希望在每次保存时，自动 fix 可修复问题，可以在 vscode 中设置保存时 fix：

```json
/*
 * VSCode Settings JSON File
 */
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
```

## `5. ESLint 配置 *`

配置文件是 eslint 最主要的配置方式。eslint 配置文件支持多种格式，同一目录下，eslint 按照 `.eslintrc.js`，`.eslintrc.cjs`，`.eslintrc.yaml`，`.eslintrc.yml`，`.eslintrc.json`，`package.json 下的 eslintConfig字段` 的顺序查找配置，相同目录下只有一个配置文件会生效。其中，常用的是 `eslintrc.[js|json]`格式，下面采用 eslint.json 格式。

eslint 还支持使用注释更灵活、细粒度地对特定文件、局部代码进行配置，常用于腹泻特殊代码片段的规则。

eslint 配置的主要内容包括 **执行环境 env**、**全局变量 globals**、**规则 rules**。

## 5.1 语言环境配置

不同的执行环境会向 JavaScript 注入不同的全局变量，如浏览器中的 window，Node.js 中的 process。另外摸不透的 ECMAScript 语法版本支持的语法和特性有所不同，因此，代码的合法性是与执行环境和环境支持的 ES 版本相关的。

为了 eslint 能正确失败代码是否符合规范，碧玺配置代码的执行环境和支持的语法选项。

### **`5.1.1 环境 env`**

eslint 默认不开启任何环境，可以在 env 字段下使用 `"env_name": true`的方式开启，环境之间不会互斥，可以同时开启多个环境，如：

```json
{
  "env": {
    "browser": true,
    "node": true,
    "es6": true
  }
}
```

开启环境后，eslint 能正常解析代码中的全局变量，可能还会开启对应的语法解析选项。常见的环境有：

- `browser`：浏览器全局变量
- `node`：Node.js 全局变量和作用域
- `es6`：支持 es6 语法（不含 ES module），并开启 ES6 语法解析选项。

完整的环境列表：参看： ESLint Docs: Specifying Environments。

### **`5.1.2 全部变量 globals`**

env 可以方便的支持特定环境下的全局变量，但 JavaScript 的执行环境要复杂得多，每个模块和外部依赖都有可能注入自己的全部变量，为了避免这被识别成错误，需要再配置中指出代码中用到的外部全局变量。

globals 字段用于设置代码中的全局变量，语法为`{"var_name": "off"|"readonly"|"writable"}`，如：

```json
{
  "globals": {
    "$": "readonly",
    "globalState": "writable"
  }
}
```

**"off" 的作用在于关闭 env 带来的全局变量**，如在支持大部分 ES2015 语法，但不支持 Promise 的浏览器中，可以设置：

```json
{
  "env": {
    "es6": true
  },
  "globals": {
    "Promise": "off"
  }
}
```

### **`5.1.3 解析选项 parserOptions`**

在 eslint 解析的时候提供一些语言特性的支持，如 ES 语法、JSX。eslint 默认支持 es5 语法。

配置语法如下：

```json
{
  "parserOptions": {
    /* ecmaVersion: 指定 ECMA 语法版本
     *  取值：
     *      - "latest": 使用最新版本，现在 (2021) 等同于 12
     *      - 版本号：3, 5（默认）, 6, 7, 8, 9, 10, 11, 12
     *      - 年份命名法：2015(=6), 2016(=7), 2017(8) ...
     */
    "ecmaVersion": "latest",

    /* sourceType: 脚本类型，普通脚本 或 ES 模块脚本
     *  取值："script"（默认） | "module"(ES 模块脚本）
     */
    "sourceType": "module",

    /* ecmaFeatures: 支持的特性语法*/
    "ecmaFeatures": {
      // 支持在全局使用 return
      "globalReturn": true,

      // 默认使用严格模式（ES 5 或 以上）
      "impliedStrict": true,

      // 支持 JSX 语法
      "jsx": true
    }
  }
}
```

需要注意的是，**开启跟高的 ES 解析选项并不会自动支持对应的 ES 全局变量**。所以，为了支持最新的 ES 语法：

```json
{
  "env": {
    // 支持 es 语法全局变量识别，这是必须的
    "es2021": true
  }
  /* 下面是可选的，因为 es2021 会自动设置 "ecmaVersion": 12
  "parserOptions": {
    "ecmaVersion": 12
  }
  */
}
```

### **`5.1.4 使用其他解析器 parser`**

eslint 默认使用的是 `Espree`解析器。可以通过 parser 字段使用其他解析器。解析器需要满足两个条件：

- 必须是一个 Node 模块，并且在配置文件所在目录下能找到，比如同目录下的 npm 包，一个由路径指定的 js 文件。
- 满足 ESLint 解析器接口。

**如果项目中用到一下语言增强工具（TypeScript、Babel）或者框架（React、Vue），就需要使用预制对应的解析器**。以 typescript 为例，为了正确解析代码，需要：

1. 在 `.eslintrc.json`对应的目录下安装解析器

```shell
npm install --save-dev @typescript-eslint/parser
```

2. 在配置文件中配置

```json
{
  "parser": "@typescript-eslint/parser"
}
```

注：使用其他插件时，parserOptions 仍然有效，他会作为参数传递给解析器。

## 5.2 规则配置 rules

rules 是 eslint 中最重要的配置，里面规定了将采用哪些规则去约束代码。

eslint 提供了大量的`内置规则`。此外，使用插件可以添加适合特定场景下的规则集。

规则的配置语法是 `{"rules": {"rule_name": state | [state, ...options] }}`，其中，state 代表枚举值：

- `"off"`或者 `0` ：关闭规则，常用于关闭某个来自 extends 中的规则
- `"warn"`或者 `1`：规则校验不通过时发出 warning 提示
- `"error"`或者 `2`：规则校验不通过时发出 error 提示，返回 1，表示 lint 检查不通过。

下面的配置方式都是合法的：

```json
{
  "rules": {
    // 使用 "off", "warn", "error"
    "no-console": "warn",
    // 使用数字（不推荐，语义不明确）
    "for-direction": 1,
    // 数组语法，但没有额外配置项
    "no-else-return": ["error"],
    // 数组语法，一个配置项
    "eqeqeq": ["error", "always"],
    // 数组语法，多个配置项
    "quotes": ["error", "double", { "avoidEscape": true }]
  }
}
```

## 5.3 使用插件 plugins

eslint 支持第三方插件的使用，使用前，需要先在配置文件的目录上安装对应的 npm 包。

安装插件之后，就可以在 eslint 的 plugins 数组中添加该配置文件需要使用的插件。不过，`插件的各项规则配置都是默认关闭的，所以 plugins 只是使用插件功能的前提，必须在 rules、extends、env 中开启所需要的规则特性`。

```json
{
  "plugins": ["jest"],

  "extends": ["plugin:jest/recommended"],

  "env": {
    "jest/global": true
  },

  "rules": {
    "jest/valid-expect": "error"
  }
}
```

### 5.3.1 插件的命名

ESLint 对插件的命名做了规定：

- `eslint-plugin-<plugin-name>` 缩写 `<plugin-name>`: 如 `eslint-plugin-jquery` 缩写 `jquery`。
- `@<scope>/eslint-plugin-<plugin-name>` 缩写 `@<scope>/<plugin-name>`: 如 `@jquery/eslint-plugin-jquery` 缩写 `@jquery/jquery`。
- `@<scope>/eslint-plugin` 缩写 `@<scope>`: 如 `@jquery/eslint-plugin` 缩写 `@jquery`。

**一般使用不含 eslint-plugin 的简写形式。**

### 5.3.2 插件能带来什么

ESLint 使用 `<plugin-name>/XXX` 的方式指定插件内的规则或其他配置。根据 ESLint Docs: Working with Plugins, 一个插件能带来：

- 额外的规则，如`{"rules": {"react/boolean-prop-naming": "warning"}}`。

- 环境，如`{"env": {"jest/global": true}}`。

- 配置，如`{"extends": ["plugin:react/recommended"]}`。

- 预处理器，如`{"process": "a-plugin/a-processor"}`。

## 5.4 继承配置 extends

1. 使用 extends 能很方便地继承已有配置的**全部特性**，包括 `rules, env, extends` 等。

2. 配置继承与面向对象中“类的继承”类似。如果称被继承配置为“父配置”，继承配置为“子配置”，那么子配置具有父配置的所有特性，并且可以在子配置中的规则会覆盖父配置的同一规则。配置继承也具有传递性。

3. 配置的继承极大方便了项目之间通用配置的共享，也避免了每次繁琐的大量规则配置。

4. extends 数组包含了配置中继承的所有父配置，当只有一个父配置时，也可以使用{"extend": "config-name"}的形式。

5. 父配置可以通过几种方式指定：

- ESLint 内置配置 `"eslint:recommended"` 和 `"eslint:all"`。
- 共享配置的包名称 `"eslint-config-<config-name>"`, 命名及其缩写和插件命名类似。如：

```json
{
  "extends": ["standard"] // 同 eslint-config-standard
}
```

- 插件内导出的配置 `"plugin:a-plugin/config", plugin:`前缀为了区分简写下的配置和插件包名。如：

```json
{
  // 别忘了 plugins
  "plugins": ["react"],
  "extends": ["plugin:react/recommended"]
}
```

- ESLint 配置文件的路径。如：`{"extends": "./.my-eslintrc.json"}`

## 5.5 通过 Glob 匹配应用配置 overrides

overrides 支持通过 Glob 模式 匹配特定文件集合，额外应用不同的配置。比如，我们经常需要在项目中根据文件类型应用不同的规则。

`overrides` 是一个配置对象数组，里面的对象支持大部分的 ESLint 配置，而且多了用于匹配文件的 `files` 数组和 `excludedFiles` 数组。当对文件进行 lint 检查时，目标文件相对于配置文件的相对路径会与这两组 glob 模式进行匹配，如果路径满足 `files` 中任何一个，且不满足 `excludedFiles` 中任何一个模式，则会被应用该配置。

```json
/* ./src 目录下除了 a.js 下的 js 文件应用 no-alert 规则 */
{
  "overrides": [
    {
      "files": ["src/*.js"],
      "excludedFiles": "a.js",
      "rules": {
        "no-alert": "warn"
      }
    }
  ]
}
```

## 5.6 忽略文件 ignorePatterns

ignorePatterns 数组包含了一组 glob 模式，当文件路径匹配其中任意一个时被忽略，作用类似`.gitignore`。

一般都不希望对打包产物进行 lint 检查：

```json
{
  "ignorePatterns": ["**/dist/**", "**/output/**"]
}
```

## 5.7 使用注释进行配置

除了文件配置，ESLint 还支持使用注释进行文件内的配置，这种方式更灵活，一般作为补充。

注：如果通过命令行执行，还能通过命令参数注入配置，这里不讨论。

ESLint 注释区分// 和/\*\*/, 可以在注释的同时说明原因，原因放在配置内容之后，用两个或两个以上 - 隔开。

常用的注释：

- 大部分时候，你只是想偷懒，关掉烦人的规则：

```js
/*
 *  单行关闭
 */
alert("foo"); // eslint-disable-line

// eslint-disable-next-line -- I don't want eslint
alert("foo");

/* eslint-disable-next-line */
alert("foo");

alert("foo"); /* eslint-disable-line */

// eslint-disable-next-line no-alert, quotes, semi
alert("foo");

foo(); // eslint-disable-line plugin/rule-name

/*
 *  块关闭
 */
/* eslint-disable */
console.log("bar");
alert("foo");
/* eslint-enable */

/* eslint-disable no-alert, no-console */
alert("foo");
console.log("bar");
/* eslint-enable no-alert, no-console */

/*
 *  整个文件关闭，在第一行使用  eslint-disable
 */
/* eslint-disable */
alert("foo");
//...

/* eslint-disable no-alert */
alert("foo");
//...
```

- 可以直接进行规则配置：

```js
/* eslint quotes: ["error", "double"], curly: 2 
  -----
  字符串内容含有'', 不想用转移和模版字符串
*/
const foo = "'bar'";
```

- 全局变量：

```js
// var1 只读，var2 读写
/* global var1, var2: writable */
```

- 环境：

```js
/* eslint-env node, mocha */
```

## 5.8 配置层级与规则合并

## **`5.8.1 配置文件层级`**

尽管一个目录下只有一个配置文件会生效，但 `ESLint 支持在不同目录层级下放置多个配置文件`。执行文件 lint 时，会从当前文件的目录开始，逐级往上查找配置文件，直到根目录或 遇到{"root": true}的配置，并合并不同层级的配置。

注：应该在项目根目录下设置 `{"root": true}` 避免不必要的查找和影响。

## **`5.8.2 配置的优先级`**

由于 ESLint 支持多处配置，配置之间可能会冲突，需要规定优先级：

- 配置类型上： 注释 > 命令行参数 > 配置文件
- 文件层级上：（相对目标文件）近 > 远
- 同一目录内：（只会采用一个配置文件）js > cjs > yaml > yml > json > package.json
- 同一配置内：overrides > rule > extends
- 同一选项内：后者 > 前者

## **`5.8.3 规则的覆盖`**

当多个地方的 rules 配置出现相同规则时，

- 如果该规则没有配置选项（只支持错误等级），则简单应用最高优先级。

- 如果该规则支持额外配置选项，且高优先级的规则配置没提供选项，则继承低优先级的选项；否则，只应用高优先级的配置和选项。如：

  - `"eqeqeq": ["error", "allow-null"]` + `"eqeqeq": "warn"`（高优先级） = `"eqeqeq": ["warn", "allow-null"]`
  - `"quotes": ["error", "single", "avoid-escape"]` + `"quotes": ["error", "single"]`（高优先级） = `"quotes": ["error", "single"]`

## 6. 其他资源

- ESLint 官网 VS ESLint 中文
  - https://eslint.org/
  - https://cn.eslint.org/
- Awesome ESLint
  - https://github.com/dustinspecker/awesome-eslint
- 代码风格自动化最佳实践：ESLint+Prettier+lint-staged
  - https://juejin.cn/post/7018488201295691789/

## 7. 常用规则

1. 与 JavaScript 代码中可能的错误或逻辑错误有关的规则：

```js
no-cond-assign          // 禁止条件表达式中出现模棱两可的赋值操作符
no-console              // 禁用console
no-constant-condition   // 禁止在条件中使用常量表达式
no-debugger             // 禁用 debugger
no-dupe-args            // 禁止 function 定义中出现重名参数
no-dupe-keys            // 禁止对象字面量中出现重复的 key
no-duplicate-case       // 禁止出现重复的 case 标签
no-empty                // 禁止出现空语句块
no-ex-assign            // 禁止对 catch 子句的参数重新赋值
no-extra-boolean-cast   // 禁止不必要的布尔转换
no-extra-parens         // 禁止不必要的括号
no-extra-semi           // 禁止不必要的分号
no-func-assign          // 禁止对 function 声明重新赋值
no-inner-declarations   // 禁止在嵌套的块中出现变量声明或 function 声明
no-irregular-whitespace // 禁止在字符串和注释之外不规则的空白
no-obj-calls            // 禁止把全局对象作为函数调用
no-sparse-arrays        // 禁用稀疏数组
no-prototype-builtins   // 禁止直接使用Object.prototypes 的内置属性
no-unexpected-multiline // 禁止出现令人困惑的多行表达式
no-unreachable          // 禁止在return、throw、continue 和 break语句之后出现不可达代码
use-isnan               // 要求使用 isNaN() 检查 NaN
valid-typeof            // 强制 typeof 表达式与有效的字符串进行比较
```

2. 下面这些规则是关于最佳实践的，帮助避免一些问题：

```js
array-callback-return   // 强制数组方法的回调函数中有 return 语句
block-scoped-var        // 强制把变量的使用限制在其定义的作用域范围内
complexity              // 指定程序中允许的最大环路复杂度
consistent-return       // 要求 return 语句要么总是指定返回的值，要么不指定
curly                   // 强制所有控制语句使用一致的括号风格
default-case            // 要求 switch 语句中有 default 分支
dot-location            // 强制在点号之前和之后一致的换行
dot-notation            // 强制在任何允许的时候使用点号
eqeqeq                  // 要求使用 === 和 !==
guard-for-in            // 要求 for-in 循环中有一个 if 语句
no-alert                // 禁用 alert、confirm 和 prompt
no-case-declarations    // 不允许在 case 子句中使用词法声明
no-else-return          // 禁止 if 语句中有 return 之后有 else
no-empty-function       // 禁止出现空函数
no-eq-null              // 禁止在没有类型检查操作符的情况下与 null 进行比较
no-eval                 // 禁用 eval()
no-extra-bind           // 禁止不必要的 .bind() 调用
no-fallthrough          // 禁止 case 语句落空
no-floating-decimal     // 禁止数字字面量中使用前导和末尾小数点
no-implicit-coercion    // 禁止使用短符号进行类型转换
no-implicit-globals     // 禁止在全局范围内使用 var 和命名的 function 声明
no-invalid-this         // 禁止 this 关键字出现在类和类对象之外
no-lone-blocks          // 禁用不必要的嵌套块
no-loop-func            // 禁止在循环中出现 function 声明和表达式
no-magic-numbers        // 禁用魔术数字
no-multi-spaces         // 禁止使用多个空格
no-multi-str            // 禁止使用多行字符串
no-new                  // 禁止在非赋值或条件语句中使用 new 操作符
no-new-func             // 禁止对 Function 对象使用 new 操作符
no-new-wrappers         // 禁止对 String，Number 和 Boolean 使用 new 操作符
no-param-reassign       // 不允许对 function 的参数进行重新赋值
no-redeclare            // 禁止使用 var 多次声明同一变量
no-return-assign        // 禁止在 return 语句中使用赋值语句
no-script-url           // 禁止使用 javascript: url
no-self-assign          // 禁止自我赋值
no-self-compare         // 禁止自身比较
no-sequences            // 禁用逗号操作符
no-unmodified-loop-condition   // 禁用一成不变的循环条件
no-unused-expressions   // 禁止出现未使用过的表达式
no-useless-call         // 禁止不必要的 .call() 和 .apply()
no-useless-concat       // 禁止不必要的字符串字面量或模板字面量的连接
vars-on-top             // 要求所有的 var 声明出现在它们所在的作用域顶部
```

3. 与使用严格模式和严格模式指令有关规则：

```js
strict; // 要求或禁止使用严格模式指令
```

4. 与变量声明有关规则：

```js
init-declarations     // 要求或禁止 var 声明中的初始化
no-catch-shadow       // 不允许 catch 子句的参数与外层作用域中的变量同名
no-restricted-globals // 禁用特定的全局变量
no-shadow             // 禁止 var 声明 与外层作用域的变量同名
no-undef              // 禁用未声明的变量，除非它们在 /global / 注释中被提到
no-undef-init         // 禁止将变量初始化为 undefined
no-unused-vars        // 禁止出现未使用过的变量
no-use-before-define  // 不允许在变量定义之前使用它们
```

5. 关于 Node.js 或 在浏览器中使用 CommonJS 的规则：

```js
global-require        // 要求 require() 出现在顶层模块作用域中
handle-callback-err   // 要求回调函数中有容错处理
no-mixed-requires     // 禁止混合常规 var 声明和 require 调用
no-new-require        // 禁止调用 require 时使用 new 操作符
no-path-concat        // 禁止对 dirname 和 filename进行字符串连接
no-restricted-modules // 禁用指定的通过 require 加载的模块
```

6. 下面的规则是关于风格指南的，而且是非常主观的

```js
array-bracket-spacing           // 强制数组方括号中使用一致的空格
block-spacing                   // 强制在单行代码块中使用一致的空格
brace-style                     // 强制在代码块中使用一致的大括号风格
camelcase                       // 强制使用骆驼拼写法命名约定
comma-spacing                   // 强制在逗号前后使用一致的空格
comma-style                     // 强制使用一致的逗号风格
computed-property-spacing       // 强制在计算的属性的方括号中使用一致的空格
eol-last                        // 强制文件末尾至少保留一行空行
func-names                      // 强制使用命名的 function 表达式
func-style                      // 强制一致地使用函数声明或函数表达式
indent                          // 强制使用一致的缩进
jsx-quotes                      // 强制在 JSX 属性中一致地使用双引号或单引号
key-spacing                     // 强制在对象字面量的属性中键和值之间使用一致的间距
keyword-spacing                 // 强制在关键字前后使用一致的空格
linebreak-style                 // 强制使用一致的换行风格
lines-around-comment            // 要求在注释周围有空行
max-depth                       // 强制可嵌套的块的最大深度
max-len                         // 强制一行的最大长度
max-lines                       // 强制最大行数
max-nested-callbacks            // 强制回调函数最大嵌套深度
max-params                      // 强制 function 定义中最多允许的参数数量
max-statements                  // 强制 function 块最多允许的的语句数量
max-statements-per-line         // 强制每一行中所允许的最大语句数量
new-cap                         // 要求构造函数首字母大写
new-parens                      // 要求调用无参构造函数时有圆括号
newline-after-var               // 要求或禁止 var 声明语句后有一行空行
newline-before-return           // 要求 return 语句之前有一空行
newline-per-chained-call        // 要求方法链中每个调用都有一个换行符
no-array-constructor            // 禁止使用 Array 构造函数
no-continue                     // 禁用 continue 语句
no-inline-comments              // 禁止在代码行后使用内联注释
no-lonely-if                    // 禁止 if 作为唯一的语句出现在 else 语句中
no-mixed-spaces-and-tabs        // 不允许空格和 tab 混合缩进
no-multiple-empty-lines         // 不允许多个空行
no-negated-condition            // 不允许否定的表达式
no-plusplus                     // 禁止使用一元操作符 ++ 和 –
no-spaced-func                  // 禁止 function 标识符和括号之间出现空格
no-trailing-spaces              // 禁用行尾空格
no-whitespace-before-property   // 禁止属性前有空白
object-curly-newline            // 强制花括号内换行符的一致性
object-curly-spacing            // 强制在花括号中使用一致的空格
object-property-newline         // 强制将对象的属性放在不同的行上
one-var                         // 强制函数中的变量要么一起声明要么分开声明
one-var-declaration-per-line    // 要求或禁止在 var 声明周围换行
operator-assignment             // 要求或禁止在可能的情况下要求使用简化的赋值操作符
operator-linebreak              // 强制操作符使用一致的换行符
quote-props                     // 要求对象字面量属性名称用引号括起来
quotes                          // 强制使用一致的反勾号、双引号或单引号
require-jsdoc                   // 要求使用 JSDoc 注释
semi                            // 要求或禁止使用分号而不是 ASI
semi-spacing                    // 强制分号之前和之后使用一致的空格
sort-vars                       // 要求同一个声明块中的变量按顺序排列
space-before-blocks             // 强制在块之前使用一致的空格
space-before-function-paren     // 强制在 function的左括号之前使用一致的空格
space-in-parens                 // 强制在圆括号内使用一致的空格
space-infix-ops                 // 要求操作符周围有空格
space-unary-ops                 // 强制在一元操作符前后使用一致的空格
spaced-comment                  // 强制在注释中 // 或 /* 使用一致的空格
```
