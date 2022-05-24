# Vuex

## 1. vuex 概述

### 1.1 组件之间数据共享的方式（小范围）

- 父向子传值：v-bind 属性绑定

- 子向父传值：v-on 事件绑定

- 兄弟之间共享数据： EventBus
  - $on 接收数据的那个组件
  - $emit 发送数据的那个组件

### 1.2 vuex 是什么

Vuex 是实现组件全局状态（数据）管理的一种机制，可以方便的实现组件之间数据的共享。

### 1.3 使用 vuex 统一管理的好处

1. 能够在 vuex 中集中管理共享的数据，易于开发和后期维护

2. 能够高效的实现组件之间的数据共享，提高开发效率
3. 存储在 vuex 中的数据都是响应式的，能实时保持数据与页面的同步
4. 一般情况下，只有组件之间共享的数据，才有必要存储到 vuex 中；对于组件中的私有数据，依旧存储在组件自身的 data 中即可

## 2. Vuex 的基本使用

- 1.安装 Vuex 依赖包

```
npm install vuex --save
```

- 2 导入 vuex 包

```js
import Vuex from "vuex";
Vue.use(Vuex);
```

- 3.创建 store 对象

```js
const store = new Vuex.Store({
  // state 中存放的就是全局共享的数据
  state: { count: 0 },
});
```

- 4.将 store 对象挂载到 vue 实例中

```js
new Vue({
  el: "#app",
  render: (h) => h(app),
  router,
  // 将创建的共享数据对象，挂载到vue实例中
  // 所有的组件，就可以直接从store中获取全局的数据了
  store,
});
```

## 3. Vuex 的核心概念

### 3.1 核心概念概述

- State
- Mutation
- Action
- Getter

### `1. State`

state `提供唯一的公共数据源`，所有共享的数据都要统一放到 store 的 state 中进行存储。

```js
// 创建store数据源，提供唯一公共数据
const store = new Vuex.Store({
  state: {
    count: 0,
  },
});
```

- 组件访问 state 中数据的 第一种 方式：

```js
this.$store.state.全局数据名称;
```

- 组件访问 state 中数据的 第二种 方式：

```js
// 1. 从vuex中按需导入mapState函数
import { mapState } from "vuex";

// 通过刚才导入的mapState函数，将当前组件需要的全局数据，映射为当前组件的computed计算属性：

// 2. 将全局数据，映射为当前组件的计算属性
computed: {
  ...mapState(["count"]);
}
```

### `2. Mutation`

mutation 用于`变更 store 中的数据`。

- 只能通过 mutation 变更 store 数据，`不可以直接操作 store 中的数据`

- 通过这种方式虽然操作起来稍微繁琐一些，但是可以集中健康所以数的变化

1. 触发 mutations 的 第一种 方式

```js
// 定义Mutation
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  mutations: {
    add(state) {
      // 变更状态
      state.count++;
    },
  },
});
```

组件中访问 mutations

```js
// 触发mutation
methods: {
  handle1() {
    // 触发mutations的第一种方式
    this.$store.commit('add')
  }
}
```

- 可以在触发 mutations 时传递参数：

```js
// 定义mutation
const store = new Vuex.State({
  state: {
    count: 0,
  },
  mutations: {
    addN(state, step) {
      //变更状态
      state.count += step;
    },
  },
});
```

```js
methods: {
  handle2() {
    // 在调用 commit 函数，
    // 触发 mutations 时携带参数
    this.$store.commit('addN'， 3)
  }
}
```

2. 触发 mutations 的 第二种 方式

```js
//1. 从vuex中按需导入mapMutations 函数
import { mapMutations } from "vuex";
```

通过刚才导入的 mapMutations 函数，将需要的 mutations 函数映射为当前`组件`的 methods 方法：

```js
// 2.将指定的mutations 函数，映射为当前组件中的methods函数
methods: {
  ...mapMutations (['add', 'addN'])
}
```

- mutation 中不能执行异步操作代码

### `3.Action `

action 用于`处理异步任务`。

如果通过异步操作变更数据，必须通过 action，而不能用 mutation，但是在 action 中还是要通过触发 mutation 的方式间接变更数据。

```js
// 定义Action
const store = new Vuex.Store({
  // ...省略其他代码
  mutations: {
    add(state) {
      state.count++;
    },
  },
  actions: {
    addAsync(context) {
      setTimeout(() => {
        context.commit("add");
      }, 1000);
    },
  },
});
```

1. 触发 actions 的第一种方式

```js
// 触发Action
methods: {
  handle() {
    //触发actions的第一种方式
    this.$store.dispatch('addAsync')
  }
}
```

- 只有 mutations 中定义的函数，才有权利修改 state 中的数据

- 在 actions 中，不能直接修改 state 中的数据，必须通过 context.commit() 触发某个 mutation 才可以

2. 触发 actions 的第二种方式：`触发异步任务时携带参数`

```js
// 定义Action
const store = new Vuex.Store({
  // ...省略其他代码
  mutations: {
    addN(state, step) {
      state.count += step;
    }, // mutations 是第四步
  },
  actions: {
    addAsync(context, step) {
      // step 是第二步
      setTimeout(() => {
        context.commit("addN", step); // ("addN", step)是第三步
      }, 1000);
    },
  },
});
```

```js
// 触发Action
methods: {
  handle() {
    //在调用dispatch函数
    // 触发actions时携带参数
    this.$store.dispatch('addAsync'， 5)// 5是第一步
  }
}
```

2. this.$store.dispatch() 是 actions 的第一种方式，触发 Actions 的第二种方式：

```js
// 1. 从 vuex 中按需导入 mapActions 函数
import { mapActions } from "vuex";
```

通过刚才指定 actions 函数，将需要的 actions 函数，映射为当前组件的 methods 方法：

```js
// 2. 将指定的 actions 函数， 映射为当前组件的 methods 函数
methods: {
  ...mapActions(['addAsync', 'addNAsync'])
}
```

### `4.Getter`

Getter 用于对 store 中的数据进行`加工处理形成新的数据`。（包装数据）

1. Getter 可以对 store 中已有的数据加工处理之后形成新的数据，类似 vue 的计算属性。
2. store 中的数据发生变化，Getter 的数据也会跟着变化。

```js
// 定义Getter
const store = new Vuex.Store({
  state: {
    count: 0,
  },
  getters: {
    showNum: (state) => {
      return "当前最新的数量是【" + state.count + "】";
    },
  },
});
```

- 使用 getters 的第一种方式：

```js
this.$store.getters.名称;
```

- 使用 getters 的第二种方式：

```js
import { mapGetters } from 'vuex'

computed: {
  ...mapGetters(['showNu'])
}
```
