# **`指令、过滤器、侦听器、计算属性、axios、Vue-cli、组件`**

# 一、指令（Directives)

是 vue 为开发者提供的**模板语法**，用于**辅助开发者渲染页面的基本结构**。

### **`按照不同的用途`**进行分类

1. **内容渲染** 指令
2. **属性绑定** 指令
3. **事件绑定** 指令
4. **双向绑定** 指令
5. **条件渲染** 指令
6. **列表渲染** 指令

## 1. 内容渲染 指令

用来辅助开发渲染 dom 元素的文本内容。

**1. `v-text`**

- 会覆盖元素内部原有的内容（默认的值）

**2. `{{}} 语法--插值表达式（Mustache）`**

- 用来专门解决 v-text 会覆盖默认文本内容的问题。

- `注意：`插值表达式只能用在元素的**内容节点**中，不能用在元素的**属性节点**中。

- 在实际开发中用的最多，只是内容的占位符，不会覆盖原有的内容

**3. `v-html`**

- v-text 和插值表达式只能渲染纯文本内容。

- v-html 可以把包含 html 标签的字符串渲染为真正的 DOM 元素

## 2. 属性绑定 指令--- v-bind

- 使用 v-bind 指令可以为元素属性动态绑定值
- v-bind: 指令可以简写为 :
- 在使用 v-bind 期间，如果绑定的内容需要进行动态拼接，则字符串的外面应该包裹单引号，例如:

`<div :title="'box' + index">这是一个 div</div>`

### 2.1 在插值和属性绑定中编写 JS 语句

在 vue 提供的模板渲染语法中，除了支持绑定简单的数据值外，还支持 JavaScript 表达式的运算。

## 3. 事件绑定 指令--- v-on

- 辅助程序员为 DOM 元素绑定事件监听
- 在创建的 vue 实例对象里面使用 methods 来定义事件的处理函数
- 在绑定事件处理函数的时候，可以使用 () 传递参数
- v-on: 指令可以被简写为 @
- @click='show'和@click='show'(传参)
- 注意：原生 DOM 对象 onclick、oninput、onkeyup 等原生事件，替换为 vue 的事件绑定形式后，分别为：v-on:click、v-on:input、v-on:keyup

1. 语法格式为：

   ```xml
   <button @click="add"></button>

   methods: {
      add() {
   			// 如果在方法中要修改 data 中的数据，可以通过 this 访问到
   			this.count += 1
      }
   }
   ```

### 3.1. 事件对象 $event(不常用)

- @click='show'(传参)，传参过程中，e 会被覆盖
- vue 提供了内置变量，名字叫做 $event，它就是原生 DOM 的事件对象 e。@click='show'(3,$event)

- `$event` 的应用场景：如果默认的事件对象 e 被覆盖了，则可以手动传递一个 $event。例如：

  ```xml
  <button @click="add(3, $event)"></button>

  methods: {
     add(n, e) {
  			// 如果在方法中要修改 data 中的数据，可以通过 this 访问到
  			this.count += 1
     }
  }
  ```

### 3.2 事件修饰符

在事件处理函数中调用 event.preventDefault()或者 event.stopPropagation()是非常常见的需求。因此，vue 提供了事件修饰符的概念，来辅助程序员更方便的对事件触发进行控制。

| 事件修饰符      | 说明                                                      |
| --------------- | --------------------------------------------------------- |
| .prevent (重点) | 阻止默认行为（例如：阻止 a 链接的跳转、阻止表单的提交等） |
| .stop (重点)    | 阻止事件冒泡                                              |
| .capture        | 以捕获模式触发当前的事件处理函数                          |
| .once           | 绑定是事件只触发 1 次                                     |
| .self           | 只有在 event.target 是当前元素自身时触发事件处理函数      |

- `.prevent`

  ```xml
  <a @click.prevent="xxx">链接</a>
  ```

- `.stop`

  ```xml
  <button @click.stop="xxx">按钮</button>
  ```

### 3.3 按键修饰符(不常用)

在监听键盘事件时，经常需要判断详细的按键，此时，可以为键盘相关事件添加按键修饰符。esc 和 enter 键常用。

```xml
<div id="app">
    <input type="text" @keyup.esc="clearInput" @keyup.enter="commitAjax">
  </div>
```

## 4. 双向绑定 指令--- v-model

`只能用在表单元素上。`

1. input 输入框
   - type = "radio"
   - type = "checkbox"
   - type = "xxxx"
2. textarea
3. select

### 4.1 v-model 指令的修饰符

为了方便对用户输入的内容进行处理，vue 为 v-model 指令提供了 3 个修饰符：

| 修饰符  | 作用                             | 示例                             |
| ------- | -------------------------------- | -------------------------------- |
| .number | 自动将用户输入的值转换为数值类型 | `<input v-model.number='age'/> ` |
| .trim   | 自动过滤用户输入的首位空白字符   | `<input v-model.trim='msg'/>`    |
| .lazy   | 在"change"时而非"input"时更新    | `<input v-model.lazy='msg'/>`    |

## 5. 条件渲染 指令--- v-if

来赋值开发者按需控制 DOM 的显示与隐藏。

- `v-if（常用）`
- `v-show`

1. v-show 的原理是：动态为元素添加或移除 display：none 样式，来实现元素的显示和隐藏
   - 如果要实现频繁的切换元素的显示状态，用 v-show 性能会更好
2. v-if 的原理是：每次动态创建或移除元素，实现元素的显示和隐藏
   - 如果刚进入页面的时候，某些元素默认不需要被展示，而且后期这个元素很可能也不需要被展示出来，此时 v-if 性能更好
3. 在实际开发中，绝大多数不用考虑性能问题，直接使用 v-if

v-if 指令在使用的时候，有两种方式：

1. 直接给定一个布尔值 true 或 false

   ```xml
   <p v-if="true">被 v-if 控制的元素</p>
   ```

2. 给 v-if 提供一个判断条件，根据判断的结果是 true 或 false，来控制元素的显示和隐藏

   ```xml
   <p v-if="type === 'A'">良好</p>
   ```

- `v-else-if`(很少使用)

充当 v-if 的"else-if"块，可以连续使用

注意：v-else-if 指令必须配合 v-if 指令一起使用，否则将不会被识别。

```xml
 <div v-if="type === 'A'">优秀</div>
    <div v-else-if="type === 'B'">良好</div>
    <div v-else-if="type === 'C'">一般</div>
    <div v-else>差</div>
```

## 6. 列表渲染 指令--- v-for

来辅助开发者**基于一个数组来循环渲染一个列表结构**，v-for 指令需要用**items in item**形式的特殊语法，其中：

- items 是 待循环的数组
- item 是被循环的每一项
- v-for 指令还支持一个可选的第二个参数，即当前项的索引，语法格式为(item,index) in items，示例代码如下：

```js
data: {
  list: [// 列表数据
  {id: 1, name: 'zs'}
  {id: 2, name: 'ls'}
  ]
}
//----------分割线--------
<ul>
   <li v-for="(item, index) in list">索引是：{{ index }}, 姓名是: {{ item.name }}
</ul>
```

注意：v-for 指令中的 item 项和 index 索引都是形参，可以根据需要重新命名。例如(user, i) in uselist

```
只要用到了 v-for 指令，那么一定要绑定一个 :key 属性

1. key的值只能是字符串或数字类型
2. key的值必须具有唯一性
3. 建议把数据项的ID属性的值作为key的值
4. 使用index的值当做key的值没有任何意义（因为index的值不具有唯一性）
5. 建议使用v-for指令时一定要指定key的值（既提升性能，又防止列表状态紊乱）
```

## vue 指令总结

1. vue 的使用步骤：

- 导入 vue.js 文件
- new Vue()构造函数，得到 vm 实例对象
- 声明 el 和 data 数据节点
- MVVM 的对应关系

2. vue 常见指令用法

- 插值表达式、v-bind、v-on、v-if 和 v-else
- v-for 和：key、v-model

# 二、过滤器（vue2.0 才使用）（Filter）

过滤器（Filters）是 vue 为开发者提供的功能，**常用于文本的格式化**。过滤器还可以用在两个地方：**插值表达式和 v-bind 属性绑定**。

1. 要定义到 filters 节点下，**本质是一个函数**
2. 在过滤器函数中，**一定要有 return 值**
3. 在过滤器的形参中，可以获取到“管道符”前面待处理的那个值
4. 过滤器应该被添加在 JS 表达式的尾部，由“管道符”进行调用。
5. 如果全局过滤器和私有过滤器名字一致，此时按照“**就近原则**”，调用的是”私有过滤器“
6. 全局过滤器是 Filter；私有过滤器是 Filters。

```js
 <script>
    // 使用 Vue.filter() 定义全局过滤器
    Vue.filter('capi', function (str) {
      const first = str.charAt(0).toUpperCase()
      const other = str.slice(1)
      return first + other + '~~~'
    })
```

# 三、侦听器（watch）

watch 侦听器允许开发者监视数据的变化，从而**针对数据的变化做特定的操作**。

1. 所有的侦听器，都应该被定义到 watch 节点下
2. 侦听器本质上是一个函数，要监视哪个数据的变化，就把数据名作为方法名即可。
3. 新值在前，旧值在后

### 侦听器的格式

1. 方法 function 格式的侦听器
   - 缺点 1：无法在刚进入页面的时候，自动触发！！！
   - 缺点 2：如果侦听的是一个对象，如果对象中的属性发生了变化，不会触发侦听器！！！
2. 对象 immediate 格式的侦听器
   - 好处 1：可以通过 **immediate** 选项，让侦听器自动触发！！！
   - 好处 2：可以通过 **deep** 选项，让侦听器深度监听对象中每个属性的变化！！！

- immediate 选项的默认值是 false
- immediate 的作用是：控制侦听器是否自动触发一次！

# 四、计算属性（computed）

是指通过一系列运算之后，最终得到一个属性值。

这个动态计算出来的属性值可以被 template 模板结构或者 methods 方法（this 计算属性的名字）使用。

特点：

1. 定义的时候，要被定义为“方法”
2. 在使用计算属性的时候，当普通的属性使用即可

好处：

1. 实现了代码的复用
2. 只要计算属性中依赖的数据源变化了，则计算属性会自动重新求值！

# 五、axios

axios 是一个**专注于网络数据请求的库**！

### axios 的基本使用

1. 发起 GET 请求：

   ```js
   axios({
     // 请求方式
     method: "GET",
     // 请求的地址
     url: "http://www.liulongbin.top:3006/api/getbooks",
     // URL 中的查询参数
     params: {
       id: 1,
     },
   }).then(function (result) {
     console.log(result);
   });
   ```

- 调用 axios 方法得到的返回值是 Promise 对象

2. 发起 POST 请求：

   ```js
   document
     .querySelector("#btnPost")
     .addEventListener("click", async function () {
       // 如果调用某个方法的返回值是 Promise 实例，则前面可以添加 await！
       // await 只能用在被 async “修饰”的方法中
       const { data: res } = await axios({
         method: "POST",
         url: "http://www.liulongbin.top:3006/api/post",
         data: {
           name: "zs",
           age: 20,
         },
       });

       console.log(res);
     });
   ```

# 六、vue-cli

`单页面应用程序`：（Single Page Application）SPA，**一个 web 网站中只有唯一一个 html 页面，所有的功能与交互都在这唯一的一个页面完成**。

`Vue-cli`：是 vue.js 开发的标准工具。它**简化了程序员基于 webpack 创建工程化的 vue 项目的过程**。

- vue-cli 是 npm 上的一个全局包，使用 npm install 命令，`npm install -g@vue/cli`；vue -V 查看是否安装成功。
- 基于 vue-cli 快速生成工程化的 vue 项目：`vue create项目的名称`

## vue-cli 的使用

1. 在终端下运行如下的命令，创建指定名称的项目：

   ```bash
   vue cerate 项目的名称
   ```

2. vue 项目中 src 目录的构成：

   ```
   1. assets 文件夹：存放项目中用到的静态资源文件，例如：css 样式表、图片资源
   2. components 文件夹：程序员封装的、可复用的组件，都要放到 components 目录下
   3. main.js 是项目的入口文件。整个项目的运行，要先执行 main.js
   4. App.vue 是项目的根组件。定义所看见的UI结构。
   ```

### vue 项目的运行流程

在工程化项目中，vue 通过 main.js 把 App.vue 渲染到 index.html 指定的区域中。

1. App.vue 用来编写待渲染的模板结构
2. index.html 中需要预留一个 el 区域
3. main.js 把 App.vue 渲染到了 index.html 所预留的区域中

# 七、Vue 组件

**组件化开发**：根据封装的思想，把页面上重用的 UI 结构封装为组件，从而方便项目的开发和维护。

### **`1. vue项目中的组件化开发`**

- vue 是一个**支持组件化开发**的前端框架
- vue 中规定：组件的后缀名是.vue。例如 App.vue。

### **`2. vue组件的三个组成部分`**

- template -> 组件的模板结构
- script -> 组件的 js 行为
- style -> 组件的样式

```xml
<script>
// 默认导出。固定写法！
export default {
  // data 数据源
  // 注意： .vue组件中的data不能和之前一样，不能指向对象
  // 组件中的 data 必须是一个函数
  data() {
    // 这个return出去的{}中，可以定义数据
    return {
      username: 'admin'
    }
  }
}
</script>
```

- template 模板结构中只能有一个 div
- 在 style 中 必须加上一个 `<style lang='less'>`，不加的话只支持 css 语法

### **`3. 组件之间的父子关系`**

组件在被封装好之后，彼此之间是相互独立的，不存在父子关系。

在使用组件的时候，根据彼此的嵌套关系，形成了父子关系、兄弟关系。

### **`4. 使用组件的三个步骤`**

1. 使用 import 语法**导入需要的组件**

```js
import Left from `@/components/Left.vue`
```

2. 使用 **components** 节点注册组件

```js
export default {
  components: {
    Left,
  },
};
```

3. 以**标签形式**使用刚才注册的组件

```js
<div class="box">
  <Left></Left>
</div>
```

`通过components注册的是私有子组件`

- 例如：在组件 A 的 components 节点下，注册了组件 F。则组件 F 只能用在组件 A 中，不能被用在组件 C 中。

### **`5. 注册全局组件`**

在 vue 项目的 main.js 入口文件中，通过 Vue.component()方法，可以注册全局组件。

```js
// 导入需要被全局注册的那个组件
import Count from "@/components/Count.vue";
// 参数1，字符串格式，表示组件的注册名称
// 参数2，需要被全局注册的那个组件
Vue.component("MyCount", Count);

Vue.config.productionTip = false;
```

### **`6. 组件的props`**

props 是组件的自定义属性，在**封装通用组件**的时候，合理的使用 props 可以极大的**提高组件的复用性**。

- props 是"自定义属性"，允许使用者通过自定义属性，为当前组件指定初始值
- 自定义属性的名字，是封装者自定义的（只要名称合法即可）
- props 中的数据，可以直接在模板结构中被使用
- 注意：props 是**只读的**，不要直接修改 props 的值，否则终端会报错！
- 要想修改 props 的值，可以把 props 的值**转存到 data 中**，因为 data 中的数据都是可读写的。

### 1. 数组格式

`props: ['init']`
`1.props - default 默认值`

### 2. 对象格式

```js
export default {
  props: {
    // 自定义属性A : { /* 配置选项 */ },
    // 自定义属性B : { /* 配置选项 */ },
    // 自定义属性C : { /* 配置选项 */ },
    init: {
      // 如果外界使用 Count 组件的时候，没有传递 init 属性，则默认值生效
      default: 0,
    },
  },
};
```

`2.props - type 值类型` -- 必须是 number 数字

```js
export default {
  props: {
    init: {
      default: 0,
      // init 的值类型必须是 Number 数字
      type: Number,
    },
  },
};
```

`3.props - required 必填项`

```js
export default {
  props: {
    init: {
      default: 0,
      type: Number,
      // 必填项校验
      required: true,
    },
  },
};
```

### **`7. 组件之间的样式冲突问题 --- scoped`**

默写情况下，**写在.vue 组件中的样式会全局生效**，因此很容易造成多个组件之间的样式冲突问题。

导致组件之间样式冲突的根本原因是：

- 单页面应用程序中，所有组件的 DOM 结构，都是**基于唯一的 index.html 页面**进行呈现的
- 每个组件中的样式，都会影响整个 index.html 页面中的 DOM 元素

`1.使用 deep 深度选择器 修改子组件中的样式`

```js
// 当使用第三方组件库的时候，如果有修改第三方组件默认样式的需求，需要用到 /deep/

/deep/ h5 {
  color: pink;
}
```
