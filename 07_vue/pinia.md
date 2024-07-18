# 介绍

[https://www.cnblogs.com/yaopengfei/p/17053053.html](https://www.cnblogs.com/yaopengfei/p/17053053.html)

Pinia 是 Vue 的存储库，它允许您跨组件/页面共享状态

* dev-tools 支持
    * 跟踪动作、突变的时间线
    * Store 出现在使用它们的组件中
    * time travel 和 更容易的调试
* 热模块更换
    * 在不重新加载页面的情况下修改您的 Store
    * 在开发时保持任何现有状态
* 插件：使用插件扩展 Pinia 功能
* 为 JS 用户提供适当的 TypeScript 支持或 autocompletion
* 服务器端渲染支持
## 安装

1.vue3

```javascript
yarn add pinia
npm install pinia
```
2.Vue CLI
创建一个 pinia（根存储）并将其传递给应用程序：

```javascript
import { createPinia } from 'pinia'
app.use(createPinia())
```
## 什么是store

托管全局状态，state，getters，actions（数据，计算，方法）

## 使用场景

整个应用程序中都可以访问的数据

# 应用store

引入

```javascript
// main.js
import { createApp } from 'vue'
import App from './App.vue'
import { createPinia } from 'pinia'
createApp(App).use(createPinia()).mount('#app')
```
定义store
```javascript
// store/counter.js
import { defineStore } from 'pinia'
import { ref } from 'vue'

export const useCounterStore = defineStore('counter', {
    state: () => {
        return { count: ref(2) }
    },
    getters:{
        doubleCount(state){
            return state.count*2
        }
    },
    actions: {
        increment(a) {
            this.count += a
        },
    },
})
```
使用
```javascript
<template>
<div class="App">
  {{ counter.count }}
  {{ count }}
  <!-- pinia 中store 的 getter -->
  {{ counter.doubleCount }}
  <button @click="add">add2</button>
</div>
</template>

<script>
import { defineComponent } from 'vue'
import {useCounterStore} from './store/counter'
import { storeToRefs } from 'pinia'

export default defineComponent({
  setup() {
    // pinia 中store 的 state
    const counter = useCounterStore()
    // const count = counter.count // 不会出现响应式，只能读取第一次值
    const { count } = storeToRefs(counter) // 此时读取是响应式
    
    // pinia 中store 的 actions
    function add(){
      counter.increment(1)
      console.log(counter.count)
    }
    
    return {
      count,
      counter,
      add
    }
  },
})
</script>
```
# store

## 定义store

Store 是使用 defineStore() 定义的，并且它需要一个唯一名称，作为第一个参数传递：

```javascript
import { defineStore } from 'pinia'
// useStore 可以是 useUser、useCart 之类的任何东西
// 第一个参数是应用程序中 store 的唯一 id
export const useStore = defineStore('main', {
  // other options...
})
```
## 使用 store

在setup() 中调用 useStore() ，因为之前不会创建 store

```javascript
import { useStore } from '@/stores/counter'
export default {
  setup() {
    const store = useStore()
    return {
      // 您可以返回整个 store 实例以在模板中使用它
      store,
    }
  },
}
```
* 可以定义任意数量的 store 
* 并且一旦 store 被实例化，可以直接在 store 上访问 state、getters 和 actions 中定义的任何属性
* store 是一个用reactive 包裹的对象，不需要在getter 之后写.value，但是不能对store进行解构。（就像setup 中的props 一样）
* 为了从 Store 中提取属性同时保持其响应式，需要使用storeToRefs()
```javascript
import { storeToRefs } from 'pinia'
export default defineComponent({
  setup() {
    const store = useStore()
    // `name` 和 `doubleCount` 是响应式引用
    // 这也会为插件添加的属性创建引用
    // 但跳过任何 action 或 非响应式（不是 ref/reactive）的属性
    const { name, doubleCount } = storeToRefs(store)
    return {
      name,
      doubleCount
    }
  },
})
```
# state

* state 是 store 的核心部分，定义应用程序的状态。
* 在 Pinia 中，状态被定义为返回初始状态的函数。 Pinia 在服务器端和客户端都可以工作。
```javascript
import { defineStore } from 'pinia'
const useStore = defineStore('storeId', {
  // 推荐使用 完整类型推断的箭头函数
  state: () => {
    return {
      // 所有这些属性都将自动推断其类型
      counter: 0,
      name: 'Eduardo',
      isAdmin: true,
    }
  },
})
```
## 访问state

```javascript
app.vue
const store = useStore()
store.counter++

// 重置状态
store.$reset()
```
## 选项api

先创建store

```javascript
// Example File Path:
// ./src/stores/counterStore.js
import { defineStore } from 'pinia',
const useCounterStore = defineStore('counterStore', {
  state: () => ({
    counter: 0
  })
})
```
setup 与 computed
```javascript
import { useCounterStore } from '../stores/counterStore'
export default {
  setup() {
    const counterStore = useCounterStore()
    return { counterStore }
  },
  computed: {
    tripleCounter() {
      return counterStore.counter * 3
    },
  },
}
```
不使用setup ，只使用computed
使用的是 computed、methods、...，则可以使用 mapState() 帮助器将状态属性映射为只读计算属性：

```javascript
import { mapState } from 'pinia'
import { useCounterStore } from '../stores/counterStore'
export default {
  computed: {
    // 允许访问组件内部的 this.counter
    // 与从 store.counter 读取相同
    ...mapState(useCounterStore, {
      myOwnName: 'counter',
      // 您还可以编写一个访问 store 的函数
      double: store => store.counter * 2,
      // 它可以正常读取“this”，但无法正常写入...
      magicValue(store) {
        return store.someGetter + this.counter + this.double
      },
    }),
  },
}
```
写入这些状态属性（例如，如果您有一个表单），可以使用 mapWritableState() 代替
```javascript
import { mapWritableState } from 'pinia'
import { useCounterStore } from '../stores/counterStore'
export default {
  computed: {
    // 允许访问组件内的 this.counter 并允许设置它
    // this.counter++
    // 与从 store.counter 读取相同
    ...mapWritableState(useCounterStore, ['counter'])
    // 与上面相同，但将其注册为 this.myOwnName
    ...mapWritableState(useCounterStore, {
      myOwnName: 'counter',
    }),
  },
}
```
对于像数组这样的集合，不需要 mapWritableState()，除非用 cartItems = [] 替换整个数组，mapState() 仍然允许调用集合上的方法。
## 改变状态

* 直接用 store.counter++ 修改 store
* 可以调用 $patch 方法。 允许使用部分“state”对象，同时应用多个更改
```javascript
store.$patch({
  counter: store.counter + 1,
  name: 'Abalam',
})
```
任何集合修改（例如，从数组中推送、删除、拼接元素）都需要创建一个新集合，所以$patch 方法也接受一个函数来批量修改集合内部分对象的情况：
```javascript
cartStore.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```
## *替换state

将 $state 属性设置为新对象来替换 Store 的整个状态：

```javascript
store.$state = { counter: 666, name: 'Paimon' }
```
还可以通过更改 pinia 实例的 state 来替换应用程序的整个状态。 这在 SSR for hydration 期间使用。
## 订阅状态

# getters

* Getter 完全等同于 Store 状态的 计算值。 
* 用 defineStore() 中的 getters 属性定义。 
* 接收“状态”作为第一个参数以箭头函数的使用：
```javascript
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    doubleCount: (state) => state.counter * 2,
  },
})
```
* 大多数，getter 只会依赖状态
* 需要使用其他 getter时，可以定义常规函数时，通过 this 访问到 整个 store 的实例
* 在 TypeScript 中，需要定义返回类型
```javascript
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    // 自动将返回类型推断为数字
    doubleCount(state) {
      return state.counter * 2
    },
    // 返回类型必须明确设置
    doublePlusOne(): number {
      return this.counter * 2 + 1
    },
  },
})
```
可以直接在 store 实例上访问 getter
```javascript
<template>
  <p>Double count is {{ store.doubleCount }}</p>
</template>
<script>
export default {
  setup() {
    const store = useStore()
    return { store }
  },
}
</script>
```
## 访问其他 getter

通过 this 访问任何其他 getter 

```javascript
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  getters: {
    // 类型是自动推断的，因为我们没有使用 `this`
    doubleCount: (state) => state.counter * 2,
    // 这里需要我们自己添加类型（在 JS 中使用 JSDoc）。 我们还可以
    // 使用它来记录 getter
    /**
     * 返回计数器值乘以二加一。
     *
     * @returns {number}
     */
    doubleCountPlusOne() {
      // 自动完成 ✨
      return this.doubleCount + 1
    },
  },
})
```
## 将参数传递给 getter

* *Getters* 只是幕后的 *computed* 属性，因此无法向它们传递任何参数。 
* 但是，可以从 *getter* 返回一个函数以接受任何参数：
```javascript
// counter.js
import { defineStore } from 'pinia'
export const useCounterStore = defineStore('counter', {
    state: () => {
        return { 
            users: [{id:1,name:'qq'},{id:2,name:'ss'},{id:3,name:'tt'}]
        }
    },
    getters:{   
        getUserById: (state) => {
            return (userId) => state.users.find((user) => user.id === userId)
          },
    },
})
```

```javascript
// app.vue
<template>
<div class="App">
  <p>User 2: {{ getUserById(2) }} {{ getUserById(2).name }}</p>
</div>
</template>

<script>
import { defineComponent } from 'vue'
import {useCounterStore} from './store/counter'

export default defineComponent({
  setup() {
    return { 
      getUserById: counter.getUserById
    }
  },
})
</script>
```
在执行此操作时，getter 不再缓存，它们只是调用的函数
## 访问其他 Store 的getter

要使用其他存储 getter，可以直接在 *better* 内部使用它：

```javascript
import { useOtherStore } from './other-store'
export const useStore = defineStore('main', {
  state: () => ({
    // ...
  }),
  getters: {
    otherGetter(state) {
      const otherStore = useOtherStore()
      return state.localData + otherStore.data
    },
  },
})
```
## 与 `setup()` 一起使用

 可以直接访问任何 getter 作为 store 的属性（与 state 属性完全一样）

```javascript
export default {
  setup() {
    const store = useStore()
    store.counter = 3
    store.doubleCount // 6
  },
}
```
## 使用选项 API

```javascript
// Example File Path:
// ./src/stores/counterStore.js
import { defineStore } from 'pinia',
const useCounterStore = defineStore('counterStore', {
  state: () => ({
    counter: 0
  }),
  getters: {
    doubleCounter() {
      return this.counter * 2
    }
  }
})
```
### 使用`setup()`

 setup() 钩子可以使在 Options API 中使用 Pinia 更容易。 不需要额外的 map helpers 功能

```javascript
import { useCounterStore } from '../stores/counterStore'
export default {
  setup() {
    const counterStore = useCounterStore()
    return { counterStore }
  },
  computed: {
    quadrupleCounter() {
      return counterStore.doubleCounter * 2
    },
  },
}
```
# Actions

* Actions 相当于组件中的 methods
* 它们可以使用 defineStore() 中的 actions 属性定义
* 非常适合定义业务逻辑
```javascript
export const useStore = defineStore('main', {
  state: () => ({
    counter: 0,
  }),
  actions: {
    increment() {
      this.counter++
    },
    randomizeCounter() {
      this.counter = Math.round(100 * Math.random())
    },
  },
})
```
* 与 getters 一样，操作可以通过 this 访问 whole store instance 并提供完整类型（和自动完成✨）支持
* actions 可以是异步的，您可以在其中await 任何 API 调用甚至其他操作
## 访问其他 store 操作

使用另一个 store ，可以直接在操作内部使用它

```javascript
import { useAuthStore } from './auth-store'
export const useSettingsStore = defineStore('settings', {
  state: () => ({
    // ...
  }),
  actions: {
    async fetchUserPreferences(preferences) {
      const auth = useAuthStore()
      if (auth.isAuthenticated) {
        this.preferences = await fetchPreferences()
      } else {
        throw new Error('User must be authenticated')
      }
    },
  },
})
```
## 订阅 Actions

* 可以使用 store.$onAction() 订阅 action 及其结果。 
* 传递给它的回调在 action 之前执行。 
* after 处理 Promise 并允许您在 action 完成后执行函数。 
* 以类似的方式，onError 允许您在处理中抛出错误。
*  这些对于在运行时跟踪错误很有用，类似于 Vue 文档中的这个提示。
```javascript
const unsubscribe = someStore.$onAction(
  ({
    name, // action 的名字
    store, // store 实例
    args, // 调用这个 action 的参数
    after, // 在这个 action 执行完毕之后，执行这个函数
    onError, // 在这个 action 抛出异常的时候，执行这个函数
  }) => {
    // 记录开始的时间变量
    const startTime = Date.now()
    // 这将在 `store` 上的操作执行之前触发
    console.log(`Start "${name}" with params [${args.join(', ')}].`)
    // 如果 action 成功并且完全运行后，after 将触发。
    // 它将等待任何返回的 promise
    after((result) => {
      console.log(
        `Finished "${name}" after ${
          Date.now() - startTime
        }ms.\nResult: ${result}.`
      )
    })
    // 如果 action 抛出或返回 Promise.reject ，onError 将触发
    onError((error) => {
      console.warn(
        `Failed "${name}" after ${Date.now() - startTime}ms.\nError: ${error}.`
      )
    })
  }
)
// 手动移除订阅
unsubscribe()
```
* 默认情况下，action subscriptions 绑定到添加它们的组件（如果 store 位于组件的 setup() 内）
* 当组件被卸载时，它们将被自动删除
* 要在卸载组件后保留它们，请将 `true` 作为第二个参数传递给当前组件的 *detach* *action subscription*： 
```javascript
export default {
  setup() {
    const someStore = useSomeStore()
    // 此订阅将在组件卸载后保留
    someStore.$onAction(callback, true)
    // ...
  },
}
```
# plugins 插件

由于是底层 API，Pania Store可以完全扩展。 以下是可以执行的操作列表：

* 向 Store 添加新属性
* 定义 Store 时添加新选项
* 为 Store 添加新方法
* 包装现有方法
* 更改甚至取消操作
* 实现本地存储等副作用
* 仅适用于特定 Store
# 在组件外使用存储













.

