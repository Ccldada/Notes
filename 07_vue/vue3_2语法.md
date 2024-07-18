# 前言

* 起初 Vue3.0 暴露变量必须 return 出来，template 中才能使用；
* Vue3.2 中 只需要在 script 标签上加上 setup 属性，组件在编译的过程中代码运行的上下文是在 setup() 函数中，无需 return，template 可直接使用。
* vue-cli
* [https://baijiahao.baidu.com/s?id=1669007905967987781&wfr=spider&for=pc](https://baijiahao.baidu.com/s?id=1669007905967987781&wfr=spider&for=pc)
1. 文件结构
Vue2中，<template> 标签中只能有一个根元素，在Vue3中没有此限制

2. data
* 导入不变
* ref声明响应式数据，用于声明基本数据类型
* reactive声明响应式数据，用于声明引用数据类型
* 使用toRefs解构
```javascript
<script setup>
  import { reactive, ref, toRefs } from 'vue'

  // ref声明响应式数据，用于声明基本数据类型
  const name = ref('Jerry')
  // 修改
  name.value = 'Tom'

  // reactive声明响应式数据，用于声明引用数据类型
  const state = reactive({
    name: 'Jerry',
    sex: '男'
  })
  // 修改
  state.name = 'Tom'
  
  // 使用toRefs解构
  const {name, sex} = toRefs(state)
  // template可直接使用{{name}}、{{sex}}
</script>
```
3. method
```javascript
<template>
  // 调用方法
  <button @click='changeName'>按钮</button>  
</template>

<script setup>
  import { reactive } from 'vue'

  const state = reactive({
    name: 'Jery'
  })

  // 声明method方法
  const changeName = () => {
    state.name = 'Tom'
  }  
</script>
```
4. computed
通过computed获得doubleCount

```javascript
<script setup>
  import { computed, ref } from 'vue'

  const count = ref(1)

  // 通过computed获得doubleCount
  const doubleCount = computed(() => {
    return count.value * 2
  })
  // 获取
  console.log(doubleCount.value)
</script>
```
5. watch
```javascript
<script setup>
  import { watch, reactive } from 'vue'

  const state = reactive({
    count: 1
  })

  // 声明方法
  const changeCount = () => {
    state.count = state.count * 2
  }

  // 监听count
  watch(
    () => state.count,
    (newVal, oldVal) => {
      console.log(state.count)
      console.log(`watch监听变化前的数据：${oldVal}`)
      console.log(`watch监听变化后的数据：${newVal}`)
    },
    {
      immediate: true, // 立即执行
      deep: true // 深度监听
    }
  )
</script>
```
6. 传值
## 父传子

**父组件**

引入子组件，组件会自动注册

```javascript
<template>
  <child name='Jerry'/>  
</template>

<script setup>
  // 引入子组件
  import child from './child.vue'
</script>
```
**子组件**
* defineProps在<script setup>中自动可用，无需导入
* 需在.eslintrc.js文件中【globals】下配置【defineProps: true】
```javascript
<template>
  <span>{{props.name}}</span>
  // 可省略【props.】
  <span>{{name}}</span>
</template>

<script setup>
  // import { defineProps } from 'vue'
  // defineProps在<script setup>中自动可用，无需导入
  // 需在.eslintrc.js文件中【globals】下配置【defineProps: true】

  // 声明props
  const props = defineProps({
    name: {
      type: String,
      default: ''
    }
  })  
</script>
```
## emit子传父

**子组件**

* defineEmits和defineProps在<script setup>中自动可用，无需导入
* 需在.eslintrc.js文件中【globals】下配置【defineEmits: true】、【defineProps: true】
```javascript
<template>
  <span>{{props.name}}</span>
  // 可省略【props.】
  <span>{{name}}</span>
  <button @click='changeName'>更名</button>
</template>

<script setup>
  // import { defineEmits, defineProps } from 'vue'
  // defineEmits和defineProps在<script setup>中自动可用，无需导入
  // 需在.eslintrc.js文件中【globals】下配置【defineEmits: true】、【defineProps: true】
	
  // 声明props
  const props = defineProps({
    name: {
      type: String,
      default: ''
    }
  }) 
  // 声明事件
  const emit = defineEmits(['updateName'])
  
  const changeName = () => {
    // 执行
    emit('updateName', 'Tom')
  }
</script>
```
**父组件**
```javascript
<template>
  <child :name='state.name' @updateName='updateName'/>  
</template>

<script setup>
  import { reactive } from 'vue'
  // 引入子组件
  import child from './child.vue'

  const state = reactive({
    name: 'Jerry'
  })
  
  // 接收子组件触发的方法
  const updateName = (name) => {
    state.name = name
  }
</script>
```
## 兄弟传值 mitt

### 局部引入

>npm install [mitt](https://so.csdn.net/so/search?q=mitt&spm=1001.2101.3001.7020)
定义一个bus.js

```javascript
import  mitt  from 'mitt';
const  mitts=mitt();
export default mitts;
```
**组件A：**
```javascript
<template> 
    <div style="width：300px;height:200px;background:blue">
        <button @click="child">兄弟传参</button>
    </div>
</template>
<script setup lang="ts">
import  mitts  from '../../util/bus.js'; 
let  flag=false;
const  child=()=>{
    mitts.emit('event',flag)
}
</script>
```
**组件B：**
```javascript
<template>
    <div style="width：300px;height:200px;background:red"> 
      B组件接受的值：  {{flag}}
    </div>
</template>

<script setup lang="ts">
import {onBeforeMount,ref} from "vue";
import  mitts  from '../../util/bus.js';
 let  flag=ref(null);
onBeforeMount(()=>{
  mitts.on('event',(e:boolean)=>{
      flag.value=e
  })
})
</script>
```
### 全局引入

>npm install mitt
**main.ts**

```javascript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import mitt from 'mitt'
const  app=createApp(App);
app.config.globalProperties.$mitt=mitt();
app.mount('#app')

```
**兄弟A**
```javascript
<template> 
    <div style="width：300px;height:200px;background:blue"> 
        <button @click="child">兄弟传参</button>
    </div>
</template>
<script setup lang="ts">
import {  getCurrentInstance,ComponentInternalInstance  } from "vue" 
const {appContext }=getCurrentInstance() as ComponentInternalInstance
let  flag=false;
 const  child=()=>{
    flag=!flag; 
    appContext.config.globalProperties.$mitt.emit('event',flag) 
}
</script>
```
**兄弟B**
```javascript
<template>
    <div style="width：300px;height:200px;background:red"> 
     {{flag}}
    </div>
</template>

<script setup lang="ts">
import { ref ,onBeforeMount,getCurrentInstance,ComponentInternalInstance} from "vue";
type Props={
    flag:boolean
} 
const flag<Props>=ref(null);
const {appContext}=getCurrentInstance() as ComponentInternalInstance 
onBeforeMount(()=>{
    appContext.config.globalProperties.$mitt.on('event',(e:boolean)=>{ 
      flag.value=e;

  }) 
</script>
```
7. v-model
* 支持绑定多个v-model
* v-model 是 v-model:modelValue 的简写
* 绑定其他字段，如：v-model:name
## 子组件

```javascript
<template>
  <span @click="changeInfo">我叫{{ modelValue }}，今年{{ age }}岁</span>
</template>

<script setup>
  // import { defineEmits, defineProps } from 'vue'
  // defineEmits和defineProps在<script setup>中自动可用，无需导入
  // 需在.eslintrc.js文件中【globals】下配置【defineEmits: true】、【defineProps: true】

  defineProps({
    modelValue: String,
    age: Number
  })

  const emit = defineEmits(['update:modelValue', 'update:age'])
  const changeInfo = () => {
    // 触发父组件值更新
    emit('update:modelValue', 'Tom')
    emit('update:age', 30)
  }
</script>
```
## 父组件

```javascript
<template>
  // v-model:modelValue简写为v-model
  // 可绑定多个v-model
  <child
    v-model="state.name"
    v-model:age="state.age"
  />
</template>

<script setup>
  import { reactive } from 'vue'
  // 引入子组件
  import child from './child.vue'

  const state = reactive({
    name: 'Jerry',
    age: 20
  })
</script>
```
8. nextTick
```javascript
<script setup>
  import { nextTick } from 'vue'
	
  nextTick(() => {
    // ...
  })
</script>
```
9. ref 子组件实例和defineExpose
* 在标准组件写法里，子组件的数据都是默认隐式暴露给父组件的
* 但在 script-setup 模式下，所有数据只是默认 return 给 template 使用，不会暴露到组件外，所以父组件是无法直接通过挂载 ref 变量获取子组件的数据
* 如果要调用子组件的数据，需要先在子组件显示的暴露出来，才能够正确的拿到，这个操作，就是由 defineExpose 来完
## **子组件**

```javascript
<template>
  <span>{{state.name}}</span>
</template>

<script setup>
  import { reactive, toRefs } from 'vue'
  // defineExpose无需引入
  // import { defineExpose, reactive, toRefs } from 'vue'

  // 声明state
  const state = reactive({
    name: 'Jerry'
  }) 
	
  // 将方法、变量暴露给父组件使用，父组件才可通过ref API拿到子组件暴露的数据
  defineExpose({
    // 解构state
    ...toRefs(state),
    // 声明方法
    changeName () {
      state.name = 'Tom'
    }
  })
</script>
```
## 父组件

### 获取一个子组件实例

```typescript
<template>
  <child ref='childRef'/>
</template>

<script setup>
  import { ref, nextTick } from 'vue'
  // 引入子组件
  import child from './child.vue'

  // 子组件ref（TypeScript语法）
  const childRef = ref<InstanceType<typeof child>>()
  
  // nextTick
  nextTick(() => {
    // 获取子组件name
    console.log(childRef.value.name)
    // 执行子组件方法
    childRef.value.changeName()
  })
</script>
```
### 获取多个子组件实例：在 v-for 中获取子组件实例

这种情况仅适用于 v-for 循环数是固定的情况 ，因为如果 v-for 循环数 在初始化之后发生改变，那么就会导致 childRefs 再一次重复添加，childRefs 中会出现重复的子组件实例

```typescript
<template>
  <div v-for="item in 3" :key="item">
    <child :ref='addChildRef'/>
  </div>
</template>

<script setup>
  // 省略...
  
  // 子组件实例数组
  const childRefs = ref([])
  // 通过 addChildRef 方法向 childRefs 添加子组件实例
  const addChildRef = (el) => {
    childRefs.value.push(el)
  }
</script>
```
### 获取多个子组件实例：动态 v-for 获取子组件实例

通过下标来向 childRefs 添加/修改，初始化之后，动态修改 v-for 循环数，会自动根据下标重新修改该下标对应的数据

```typescript
<template>
  <button @click='childNums++'></button>
  <div v-for="(item, i) in childNums" :key="item">
    // 通过下标向 childRefs 动态添加子组件实例
    <child :ref='(el) => childRefs[i] = el'/>
  </div>
  <button @click='childNums--'></button>
</template>

<script setup>
  // 省略...
  
  // 子组件数量
  const childNums = ref(1)
  // 子组件实例数组
  const childRefs = ref([])
</script>
```
10. 插槽
## 子组件

```javascript
<template>
  // 匿名插槽
  <slot/>
  // 具名插槽
  <slot name='title'/>
  // 作用域插槽
  <slot name="footer" :scope="state" />
</template>

<script setup>
  import { useSlots, reactive } from 'vue'
  const state = reactive({
    name: '张三',
    age: '25岁'
  })
  
  const slots = useSlots()
  // 匿名插槽使用情况
  const defaultSlot = reactive(slots.default && slots.default().length)
  console.log(defaultSlot) // 1
  // 具名插槽使用情况
  const titleSlot = reactive(slots.title && slots.title().length)
  console.log(titleSlot) // 3
</script>
```
## 父组件

```javascript
<template>
  <child>
    // 匿名插槽
    <span>我是默认插槽</span>
    // 具名插槽
    <template #title>
      <h1>我是具名插槽</h1>
      <h1>我是具名插槽</h1>
      <h1>我是具名插槽</h1>
    </template>
    // 作用域插槽
    <template #footer="{ scope }">
      <footer>作用域插槽——姓名：{{ scope.name }}，年龄{{ scope.age }}</footer>
    </template>
  </child> 
</template>

<script setup>
  // 引入子组件
  import child from './child.vue'
</script>
```
12. 路由useRoute和useRouter
```javascript
<script setup>
  import { useRoute, useRouter } from 'vue-router'
	
  // 必须先声明调用
  const route = useRoute()
  const router = useRouter()
	
  // 路由信息
  console.log(route.query)

  // 路由跳转
  router.push('/newPage')
</script>
```
13. 路由导航守卫
```javascript
<script setup>
  import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'
	
  // 添加一个导航守卫，在当前组件将要离开时触发。
  onBeforeRouteLeave((to, from, next) => {
    next()
  })

  // 添加一个导航守卫，在当前组件更新时触发。
  // 在当前路由改变，但是该组件被复用时调用。
  onBeforeRouteUpdate((to, from, next) => {
    next()
  })
</script>
```
14. pinia
* 同时支持 Composition Api 和 Options api 的语法；
* 去掉 mutations ，只有 state 、getters 和 actions ；
* 不支持嵌套的模块，通过组合 store 来代替；
* 更完善的 Typescript 支持；
* 清晰、显式的代码拆分；
15. 生命周期
![图片](data:image/png;base64,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)


16. 原型绑定与组件内使用
main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
const app = createApp(App)

// 获取原型
const prototype = app.config.globalProperties

// 绑定参数
prototype.name = 'Jerry'
```
组件内使用
```javascript
<script setup>
  import { getCurrentInstance } from 'vue'

  // 获取原型
  const { proxy } = getCurrentInstance()
  
  // 输出
  console.log(proxy.name)
</script>
```
17. v-bind() CSS变量注入
```javascript
<template>
  <span>Jerry</span>
</template>

<script setup>
  import { ref, reactive } from 'vue'
  // prop接收样式
  const props = defineProps({
    border: {
      type: String,
      default: '1px solid yellow'
    }
  })
  
  // 常量声明样式
  const background = 'red'
  
  // 响应式数据声明样式
  const color = ref('blue')
  const style = reactive({
    opacity: '0.8'
  })
</script>

<style lang="scss" scoped>
  span {
    // 使用常量声明的样式
    background: v-bind(background);
    
    // 使用响应式数据声明的样式
    color: v-bind(color);
    opacity: v-bind('style.opacity');
    
    // 使用prop接收的样式
    border: v-bind('props.border');
  }
</style>
```
18. provide和inject
父组件

```javascript
<template>
  <child/>
</template>

<script setup>
  import { ref, watch, provide } from 'vue'
  // 引入子组件
  import child from './child.vue'

  let name = ref('Jerry')
  // 声明provide
  provide('provideState', {
    name,
    changeName: () => {
      name.value = 'Tom'
    }
  })

  // 监听name改变
  watch(name, () => {
    console.log(`name变成了${name}`)
    setTimeout(() => {
      console.log(name.value) // Tom
    }, 1000)
  })
</script>
```
子组件
```javascript
<script setup>
  import { inject } from 'vue'
  // 注入，第二个参数为默认值
  const provideState = inject('provideState', {})
  
  // 子组件触发name改变
  provideState.changeName()
</script>
```
20. 对await的支持
不必再配合 async 就可以直接使用 await 了，这种情况下，组件的 setup 会自动变成 async setup 。

```javascript
<script setup>
  const post = await fetch('/api').then(() => {})
</script>
```

