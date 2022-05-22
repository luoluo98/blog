# React 中使用 TS

概述：

掌握 TS 中的基础类型、高级类型的使用后，要在前端项目中使用 TS，还要掌握 React、vue、angular 等这些库或者框架中提供的 API 类型，以及在 TS 中是如何使用的。

以 react 为例，学习如何使用：

1. 使用 CRA 创建支持 TS 项目
2. TS 配置文件 tsconfig.json
3. react 中常用的类型

## 1. 使用 CRA 创建支持 TS 的项目

React 脚手架工具 create-react-app 默认支持 typescript。

创建支持 TS 项目命令： npx create-react-app 项目名称` --template typescript`。

当看到以下提示，表示支持 TS 的项目创建成功。

```xml
We suggest that you begin by typing:

    cd react-ts-basic
    yarn start

Happy hacking!
```

相对于非 TS 项目，目录结构主要有以下三个变化：

1. 项目根目录中增加了 `tsconfig.json` 配置文件：`指定TS的编译选项`（比如，编译时是否移除注释）
2. React 组件的文件扩展名变为：`.tsx`
3. src 目录中增加了 react-app-env.d.ts：`react项目默认的类型声明文件`
   - `三斜线指令`：指定依赖的其他类型声明文件，types 表示依赖的类型声明文件包的名称。
   ```xml
   /// <reference types= "react-scripts" />
   ```
   解释：告诉 TS 帮助加载 react-scripts 这个包提供的类型声明。

react-scripts 的类型声明文件包含了两部分类型:

1.  react、react-dom、node 的类型
2.  图片、样式等模块的类型，以允许在代码中导入图片、SVG 等文件

TS 会自动加载该 .d.ts 文件，以提供类型声明（通过修改 tsconfig.json 中的 include 配置来验证）

## 2. TS 配置文件 tsconfig.json

tsconfig.json 指定：`项目文件和项目编译所需的配置项`。

注意：TS 的配置项非常多（100+），以 CAR 项目中的配置为例来学习，其他的配置项用到时查文档即可。

1. tsconfig.json 文件所在目录为项目根目录（与 package.json 同级）
2. tsconfig.json 可以自动生成，命令： `ts --init`

### **tsconfig 的解释说明**

- 说明：所有的配置项都可以通过鼠标移入的方式，来查看配置项的解释说明。
- [ tsconfig 文档链接]（https://www.typescriptlang.org/tsconfig)

```json
{
  // 编译选项
  "complierOptions": {
    //生成代码的语言版本
    "target": "es5",
    //指定包含在编译中的library
    "lib": ["dom", "dom.iterable", "esnext"],
    //允许 ts 编辑器编译 js 文件
    "allowJs": true,
    //跳过声明文件的类型检查
    "skipLibCheck": true,
    // es 模块 互操作，屏蔽ESModule 和CommonJs 之间的差异
    "esModuleInterop": true,
    //允许通过 import x from 'y' 及时模块没有显示指定 default 导出
    "allowSyntheticDefaultImports": true,
    // 开启严格模式
    "strict": true,
    // 对文件名称强制区分大小写
    "forceConsistentCasingInFileNames": true,
    // 为 switch 语句启用错误报告
    "noFallthoughCasesInSwitch": true,
    // 生成代码的模块化标准
    "module": "esnext",
    // 模块解析（查找）策略
    "moduleResolution": "node",
    // 允许导入扩展名为 .json 的模块
    "resolveJsonModule": true,
    // 是否将没有import/export 的文件视为旧（全局而非模块化）脚本文件
    "isolatedModules": true,
    // 编译时不生成任何文件（只进行类型检查）
    "noEmit": true,
    // 指定将JSX 编译成什么形式
    "jsx": "react-jsx"
  },
  // 指定允许 ts 处理的目录
  "include": ["src"]
}
```

除了在 tsconfig.json 文件中使用编译配置外，`还可以通过命令行来使用`。

使用演示：tsc hello.ts --target es6

注意：

1. tsc 后`带有传输文件`时，（比如，tsc hello.ts），将忽略 tsconfig.json 文件
2. tsc 后`不带输入文件`时，（比如，tsc），才会启用 tsconfig.json

**推荐使用：tsconfig.json 配置文件**

## 3. React 中的常用类型

前提说明：现在基于 class 组件来讲解 React + TS 的使用（最新的 React Hooks，后面讲解）

在不使用 TS 时，可以使用 prop-types 库，为 React 组件提供类型检查。

说明：`TS项目中，推荐使用typescript实现组件类型校验（代替PropTypes）`

不管是 React 还是 Vue，只要是支持 TS 的库，都提供了很多类型，来满足该库对类型的需求。

注意：

1. react 项目时通过 @types/react、@types/react-dom 类型声明包，来提供类型的。
2. 这些包 CRA 已帮我们安装好（react-app-env.d.ts）直接使用即可。

参考资料：react 文档-静态类型检查、react+TS 备忘单

react 是组件化开发模式，react 开发主要任务就是`写组件`，两种组件：函数组件、class 组件。

- **`函数组件：`**
  - 组件的类型
  - 组件的属性（props）
  - 组件属性的默认值（defaultProps）
  - 事件绑定和事件对象

**1. 函数组件的类型以及组件的属性**

```ts
type Props = { name: string; age?: number }
const Hello: FC<Props> = ({ name, age }) => (
  <div>你好，我叫：{name}, 我 {age} 岁了</div>
)
<Hello name="jack" age={18} />

// 实际上，还可以直接简化为（完全按照函数在TS中的写法）
const Hello = ({ name, age }: Props) => (
  <div>你好，我叫：{name}, 我 {age} 岁了</div>
)
```

**2. 函数组件属性的默认值（defaultProps）**

```ts
const Hello: FC<Props> = ({ name, age }) => (
  <div>
    你好，我叫：{name}, 我 {age} 岁了
  </div>
);
Hello.defaultProps = {
  age: 18,
};

// 实际上，还可以直接简化为（完全按照TS中的写法）：
const Hello = ({ name, age = 18 }: Props) => (
  <div>
    你好，我叫：{name}, 我 {age} 岁了
  </div>
);
```

**3. 事件绑定和事件对象**

```ts
<button onClick={onClick}>点赞</button>;

const onClick = () => {};
const onClick = (e: React.MouseEvent<HTMLButtonElement>) => {};

//再比如，文本框：
<input onChange={onChange} />;
const onChange = (e: React.ChangeEvent<HTMLInputElement>) => {};
```

`技巧：在JS中写事件处理程序 (e => {})，然后把鼠标放在e上，利用TS的类型推论来查看事件对象类型`。

```ts
<input onChange={(e) => {}} />
```

- **`class组件：`**

  - 组件的类型、属性、事件
  - 组件状态（state）

**1. class 组件的类型**

```ts
type State = { count: number };
type Props = { message?: string };

class C1 extends React.Component {} // 无props、state
class C2 extends React.Component<Props> {} // 有props、无state
class C3 extends React.Component<{}, State> {} // 无props、有state
class C4 extends React.Component<Props, State> {} // 有props、state
```

**2. class 组件的属性和属性默认值**

```ts
type Props = { name: string; age?: number }

class Hello entends React.Component<Props> {
  static defaultProps: Partial<Props> = {
    age: 18
  }
  render() {
    const { name, age } = this.props
    return <div> 你好，我叫：{name}, 我{age}岁了 </div>
  }
}

// 或者这种方式
const { name, age = 18 } = this.props
```

**3. class 组件状态（state）和事件**

```ts
type State = { count: number }

// 1.
class Counter entends React.Component<{}, State> {
  state: State = {
    count: 0
  }
}

// 2.
onIncrement = () => {
  this.setState({
    count: this.state.count + 1
  })
}

// 3.
<button onClick={this.onIncrement}>+1</button>
```
