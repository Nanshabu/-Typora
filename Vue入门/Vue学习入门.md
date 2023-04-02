# Vue学习入门

小满zs：（前置知识vue2+ts）+vue3+实战（持续更新[Vue3 + vite + Ts + pinia + 实战 + 源码 +electron_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1dS4y1y7vd/?spm_id_from=333.999.0.0)

李立超老师：vue3（持续更新）[09_自动创建项目_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1re4y1M7ko?p=9&spm_id_from=pageDriver&vd_source=341418dd6ce38b875e36cf6b18b2908c)

写网页的叮叮：[16.条件渲染v-if_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1QA4y1d7xf?p=16&vd_source=341418dd6ce38b875e36cf6b18b2908c)



>  HTML、CSS、less、sass这些是最基本的；
>  javascript、es6、ajax、[node.js](https://www.zhihu.com/search?q=node.js&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A"2175330487"})、webpack属于进阶的基础
> Promise、axios这些涉及到了vue中与后端数据的交互，会使用它们获取后端数据
>  elementUI以及其它可以和vue搭配使用的前端UI框架，

声明，响应，渐进

API风格：
选项式API和组合式API` <script setup> `

> 选项式API:
>
> 代码风格：date选项写数据，methods选项写函数、、、、，一个功能逻辑的代码分散
>
> 优点：易于学习和使用，写代码的位置已经约定好了
>
> 缺点：代码组织性差，相似的逻辑代码不便于复用，逻辑复杂代码多了不好阅读。

> 组合 API
>
> 代码风格：一个功能逻辑的代码组织在一起（包括数据，函数、、、）
>
> 优点：功能逻辑复杂繁多情况下，各个功能逻辑代码组织再一起，便于阅读和维护
>
> 缺点：需要有良好的代码组织能力和拆分逻辑能力 （在 vue3 中也可以支持 vue2 选项API 写法）

## 声明式渲染

vue的核心功能，改变时触发更新的状态被称作是**响应式**的。





# 超哥Vue

## vue入门-项目初始化

### 引入vue

1. 使用cdn引入，适合小项目
2. 使用yarn构建工具

> `yarn add vite -D` 开发模式安装vite
>
> `yarn add vue `
>
> `yarn dev`  运行

3. 代码：

```js
// 构建组件
const App = {}

// 创建应用
const app = createApp(app)

// 挂载到页面
app.mount("#root")
```

### Vue组件化

组件化类似于模块化，将组件和应用及挂载分开，同时根组件和子组件也分开。一般子组件存放在components文件中，同时根组件使用子组件需要添加代码：

```js
{
    data(){},
    components:{
        子组件1，
        子组件2
    }, // 添加该对象
    template:`<子组件1></子组件1>`
}
```

### SFC单文件组件

simple file component，也就是将js和template合在一起的文件.vue

`App.vue`代码

```html
<script>
import MyButton from "./components/MyButton";

export default {
  data() {
    return {
      message: "helloVue",
    };
  },
  components: {
    MyButton,
  },
};
</script>

<template>
  <h1>{{ message }}</h1>
</template>

```

由于浏览器**无法识别**识别vue文件，所以需要使用@vitejs/plugin-vue插件将vue转化为js文件。

1. 安装开发模式@vitejs/plugin-vue 

```powershell
yarn add @vitejs/plugin-vue -D
```

2. 配置vite.config.js文件

```js
import vue from "@vitejs/plugin-vue"

export default{
    plugins:[vue()]
}
```

配置无误就可以运行`yarn dev`

对于子组件也是同样道理，使用单文件组件

### 自动创建项目

`yarn create vue `

进入到项目中初始化`yarn` / `npm install`

开始运行`yarn dev` / `npm run dev`

## vue入门-响应式原理-代理

vue实现响应式的原理是通过使用代理对象对实例进行操作，对代理对象进行更改操作时，handler可以将响应反馈给实例，实现响应式。

`proxy`->`handler`->`app`

App.vue是根组件文件
createApp(App) 将根组件关联到应用上- 会返回一个**应用的实例**

app.mount(“#app") 将应用挂载到页面中- 会返回一个**根组件的实例**  也称作视图模型（viem model简称vm）

组件实例是一个**代理对象**`porxy`

为了使得在根组件的<u>data中可以直接访问到组件实例</u>，vue将data的this与组件实例连接，假如使用箭头函数则需要一个参数来接收。但最好在vue中少用箭头函数。

```js
// App.vue
export default {
  data: (vm) => {
    console.log(vm) // 输出：Proxy{...}
    return {
      message: "helloVue!",
    };
  },
};

// main.js
const app = createApp(App);
app.mount("#app");

```



### data对象详解

以根组件为例，data会返回一个对象作为返回值，，data返回的对象最终会被vue**代理**，从而将其转换为响应式数据。

data的代理对象实现了深层响应式对象代理，也就是对象的属性可以响应对象的对象也可以响应。

也可以做一个浅响应shallowReactive `import { shallowReactive } from "vue"` 对data返回的对象进行浅层响应的转换`return shallowReactive({ 对象 })`

### method和computed对比

method对象<u>方法</u>--与挂钩的对象同步更新`只要改变对象的任何细节就会发生`

conputed属性计算方法<u>（属性）</u>--与挂钩的响应属性同步更新`只要不改变提及的属性，就不会发生`，一般提供一个返回值

> 可以想象为method侧重于方法，没有响应，只是单纯做方法处理，但他却又是实例的内部方法，所以实例修改时，内部方法又重新调用了一次。
>
> computed侧重于属性，一个与绑定属性相关的响应属性，而return的就是相当于表现的值。相当于实例的外部方法，只要与之挂钩的属性不改变就不重置，即重新调用。

### Vue调试工具

google应用商店或者微软获取扩展，Vue.js devtools，控制台最右边。

## 组合式API

组合式api是区别于选项式api的

1. 属性不会直接写在对象中，而是写在setup的狗子函数中，需要暴露的属性需要return
2. 钩子函数中的属性默认不是响应式的，需要从vue中导入reactive包对对象进行加工，才会响应。
3. 方法和属性都需要暴露才可以使用

使用`<script setup></script>` 标记为组合式API，不需要写export default 和setup钩子函数了。直接定义属性和方法，需要响应式的同样使用reactive包加工

### reactive()

`import { reactive } from "vue"`

返回一个对象的深层响应式代理

### shallowReactive()

`import { shallowReactive } from "vue"`

同选项式API一样。返回一个对象的浅层响应式代理

### ref()

`import { ref } from "vue"`

例`const a = ref(0)`接收任意值，返回其响应式代理。

通过将基本类型属性包装为对象属性，在生成代理对象，所以在js中访问值的时候需要对其value属性进行访问，在模板中则会进行自动解包可直接访问。

针对对象使用ref，需要注意的是直接对对象进行ref定义，和对对象中的属性进行ref定义的区别。前者着重于对全体属性进行响应式处理，**模板会自动解包**；后者着重对小部分属性进行响应式处理，**模板只能识别顶部对象**，无法识别内部属性，所以需要额外添加value属性访问。

## v-bind

## 样式选择

1. `<style scoped>`: 样式将变为局部样式

   局部样式下想影响全局 使用 `deep()` 将某个标签包裹

   样式选择的原理是通过生成一个随机的字符属性，通过属性选择器来实现。

   在局部样式下通过 `globel()` 包裹标签实现全局选择

2.  `<style module>` : 模块化style，类名hash化确保类名的唯一性，使用时需要`:class="$style.classname"`，不然生成的hash类名太长不好记。

推荐使用`<style scoped>`

# 尤雨溪Vue

> 在李立超老师的入门教学后，对vue的使用有了初步的认识。
>
> 选项式和组合式，cdn引入和单文件组件的区别有了初步的认识。
>
> 会使用yarn自动创建项目，对配置文件有了初步理解。
>
> 对vue3vue2的响应式的原理有了初步认识。认识了选项式中data，methods、computed有了初步认识。对模板的使用也有了初步的认识。

## 创建一个Vue应用

createApp()

mount()

### 选项式和组合式的选择

选项式和组合式的好坏比较；

选项式的写法

组合式的写法，`<script setup>`  或者setup()钩子函数   

### 应用配置TODO

应用实例会暴露一个 `.config` 对象允许我们配置一些应用级的选项，例如定义一个应用级的错误处理器，用来捕获所有子组件上的错误：

```js
app.config.errorHandler = (err) => {
  /* 处理错误 */
}
```

应用实例还提供了一些方法来注册应用范围内可用的资源，例如注册一个组件：

```js
app.component('TodoDeleteButton', TodoDeleteButton)
```

注意：可以创建多个应用实例，但是最好不要，除非你只需要控制页面中的某几个小部分。

## 模板语法

文本插值：可以直接使用两个大括号包裹值

html插入：也可以使用v-html插入html片段。一般不推荐使用

### Attribute 绑定

响应式的绑定一个标签的属性attribute，可以使用v-bind指令，例如`v-bind: id="dynamicId"` `v-bind: src = "img"` 可以简写为`:id="dynamicId" :src = "img"`

#### 布尔型 Attribute

布尔型 attribute 依据 true / false 值来决定 attribute 是否应该存在于该元素上。`disabled `就是最常见的例子之一。

#### 动态绑定多个值

可以用v-bind绑定一个对象到标签上，键为属性attribute，值为属性的取值，常用于id和class的绑定。

```js
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}
// <div v-bind="objectOfAttrs"></div>
```

### 使用 JavaScript 表达式

可以在文本插入和`v-`开头的特殊attribute中使用JavaScript代码

仅支持单一表达式，即一段能被求值，且`return`的代码

可以绑定组件暴露的调用函数

### 模板对对象列表的访问有限TODO

模板中的表达式将被沙盒化，仅能够访问到有限的全局对象列表。该列表中会暴露常用的内置全局对象，比如 ·Math· 和 ·Date·。

没有显式包含在列表中的全局对象将不能在模板内表达式中访问，例如用户附加在 ·window ·上的属性。然而，你也可以自行在 ·app.config.globalProperties· 上显式地添加它们，供所有的 Vue 表达式使用。

## 响应式基础

前面已经了解到了vue3是通过代理对象实现响应式。我们也可以使用 `reactive()` 函数创建一个响应式对象或数组

### DOM 更新时机TODO

当你更改响应式状态后，DOM 会自动更新。然而，你得注意 DOM 的更新并不是同步的。相反，Vue 将缓冲它们直到更新周期的 “下个时机” 以确保无论你进行了多少次状态更改，每个组件都只更新一次。

若要等待一个状态改变后的 DOM 更新完成，你可以使用 `nextTick()` 这个全局 API：

```js
import { nextTick } from 'vue'

function increment() {
  state.count++
  nextTick(() => {
    // 访问更新后的 DOM
  })
}
```

### 响应式代理和原始对象并不相等

代理===原始对象  false

代理===代理  true

代理===代理的代理 true

这套规则对嵌套对象仍然适用

代理的内部属性的代理 === 代理的内部属性 false

### reactive和ref

#### reactive的局限性

1、reactive只能对对象和数组 map set这样的对象类型有效，而对原始类型无效。

2、因为 Vue 的响应式系统是通过属性访问进行追踪的，因此我们必须始终保持对该响应式对象的相同引用。这意味着我们不可以随意地**“替换”**一个响应式对象，因为这将导致对初始引用的响应性连接丢失，同时，**解构赋值**或者是将**属性传入函数**都会导致失去响应性。

#### 使用ref定义响应式变量

针对reactive的局限性，Vue 提供了一个 `ref()` 方法来允许我们创建可以使用任何值类型的响应式 ref

简言之，`ref()` 让我们能创造一种对任意值的 “引用”，并能够在不丢失响应性的前提下传递这些引用。这个功能很重要，因为它经常用于将逻辑提取到 [组合函数](https://cn.vuejs.org/guide/reusability/composables.html) 中

#### ref在模板中自动解包

#### ref在响应式对象中的解包

当一个 `ref` 被嵌套在一个响应式对象中，作为属性被访问或更改时，它会自动解包，因此会表现得和一般的属性一样：

```js
const count = ref(0)
const state = reactive({
  count
})

console.log(state.count) // 0

state.count = 1
console.log(count.value) // 1
```

如果将一个新的 ref 赋值给一个关联了已有 ref 的属性，那么它会替换掉旧的 ref： TODO**试了不能解包**

```js
const otherCount = ref(2)

state.count = otherCount
console.log(state.count) // 2
// 原始 ref 现在已经和 state.count 失去联系
console.log(count.value) // 1

```

#### 数组和集合类型的 ref 不自动解包

跟响应式对象不同，当 ref 作为响应式数组或像 `Map` 这种原生集合类型的元素被访问时，不会进行解包。

```js
const books = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// 这里需要 .value
console.log(map.get('count').value)
```

## 计算属性

计算属性是对某个响应式属性同步响应的**方法**，但他可以直接被当作响应式属性被使用。无论是`.value` 还是直接而在模板中被解包。通过`computed(function())`定义

### 计算属性vs方法

李立超老师也讲过，但是没那么深入。

计算属性会缓存其依赖的响应式属性，而不是在每次更碰到属性的上层对象发生改变时，重新计算。

举个例子：一个响应式对象的属性与计算属性形成依赖，当响应式对象的其他属性发生改变，只要不改变形成依赖的属性，就不需要重新计算。

> 想象一下我们有一个非常耗性能的计算属性 `list`，需要循环一个巨大的数组并做许多计算逻辑，并且可能也有其他计算属性依赖于 `list`。没有缓存的话，我们会重复执行非常多次 `list` 的 getter，然而这实际上没有必要！如果你确定不需要缓存，那么也可以使用方法调用。

### 可写计算属性

计算属性默认是只读的。当你尝试修改一个计算属性时，你会收到一个运行时警告。告诉你可写属性为只读状态，转化为可写状态，可以使用getter和setter来创建：

```html
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    // 注意：我们这里使用的是解构赋值语法
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
</script>
```

### Getter 不应有副作用

计算属性的 getter 应只做计算而没有任何其他的副作用，这一点非常重要，请务必牢记。举例来说，**不要在 getter 中做异步请求或者更改 DOM**！一个计算属性的声明中描述的是如何根据其他值派生一个值。因此 getter 的职责应该仅为计算和返回该值。在之后的指引中我们会讨论如何使用[监听器](https://cn.vuejs.org/guide/essentials/watchers.html)根据其他响应式状态的变更来创建副作用。

### 避免直接修改计算属性值

从计算属性返回的值是派生状态。可以把它看作是一个“临时快照”，每当源状态发生变化时，就会创建一个新的快照。更改快照是没有意义的，因此计算属性的返回值应该被视为只读的，并且永远不应该被更改——应该更新它所依赖的源状态以触发新的计算

## Class与样式绑定

```html
<script setup>
import { ref,reactive } from "vue";
const isActive = ref(true)
const hasError = ref(false)
</script>

<template>
	<div
	  class="static"
	  :class="{ active: isActive, 'text-danger': hasError }"
	></div>
</template>

//  ---用外联字面量优化一下----
```

```html
<script setup>
import { ref,reactive } from "vue";
const isActive = ref(true);
const hasError = ref(false);
const classObject = reactive({ active: isActive, "text-danger": hasError, static:true });
</script>

<template>
  <div :class="classObject"></div>
</template>
//  ---用计算属性优化一下----
```

```html
<script setup>
const isActive = ref(true)
const error = ref(null)

const classObject = computed(() => ({
  active: isActive.value && !error.value,
  'text-danger': error.value && error.value.type === 'fatal'
}))
</script>
<template>
  <div :class="classObject"></div>
</template>
```

### 绑定数组

```html
<script setup>
const isActive = ref(true)
const error = ref(null)
</script>
<template>
<div :class="[activeClass, errorClass]"></div>
</template>
```

也可以在数组里嵌套对象，不过看着有点复杂

```html
<div :class="[{ active: isActive }, errorClass]"></div>
```

### class与样式的绑定涉及到透传Attribute

在后面的深入组件会讲

### 绑定内联样式

```html
<div :style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

但最好还是绑定一个样式对象比较推荐。看起来更加简洁。

```html
<script>
const styleObject = reactive({
  color: 'red',
  fontSize: '13px'
})
</script>

<template>
<div :style="styleObject"></div>
</template>
```

### 自动前缀

当你在 `:style` 中使用了需要[浏览器特殊前缀](https://developer.mozilla.org/en-US/docs/Glossary/Vendor_Prefix)的 CSS 属性时，Vue 会自动为他们加上相应的前缀。Vue 是在运行时检查该属性是否支持在当前浏览器中使用。如果浏览器不支持某个属性，那么将尝试加上各个浏览器特殊前缀，以找到哪一个是被支持的。

### 样式多值

你可以对一个样式属性提供多个 (不同前缀的) 值，举例来说

```html
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

数组仅会渲染浏览器支持的最后一个值。在这个示例中，在支持不需要特别前缀的浏览器中都会渲染为 `display: flex`。

## 条件渲染

v-if

v-else-if

v-else

v-show: 区别于v-if，show是操作display属性，if是完全销毁和生成，切换开销大，而show初始渲染开销大。

## 列表渲染

v-for

v-for="(value,key,index) in obj"

v-for="item of items"

可以使用多层for

可以使用解构赋值

```html
<li v-for="{ message } in items">
  {{ message }}
</li>

<!-- 有 index 索引时 -->
<li v-for="({ message }, index) in items">
  {{ message }} {{ index }}
</li>

```

### 通过 key 管理状态

Vue 默认按照“就地更新”的策略来更新通过 `v-for` 渲染的元素列表。当数据项的顺序改变时，Vue 不会随之移动 DOM 元素的顺序，而是就地更新每个元素，确保它们在原本指定的索引位置上渲染。

为了给 Vue 一个提示，以便它可以跟踪每个节点的标识，从而重用和重新排序现有的元素，你需要为每个元素对应的块提供一个唯一的 `key` attribute：

```html
<div v-for="item in items" :key="item.id">
  <!-- 内容 -->
</div>
```

当你使用 `<template v-for>` 时，`key` 应该被放置在这个 `<template>` 容器上：

```html
<template v-for="todo in todos" :key="todo.name">
  <li>{{ todo.name }}</li>
</template>

```

### 组件上使用 `v-for`

我们可以直接在组件上使用 `v-for`，和在一般的元素上使用没有区别 (别忘记提供一个 `key`)：

```html
<MyComponent v-for="item in items" :key="item.id" />
```

但是，这不会自动将任何数据传递给组件，因为组件有自己独立的作用域。为了将迭代后的数据传递到组件中，我们还需要传递 props：

```html
<MyComponent
  v-for="(item, index) in items"
  :item="item"
  :index="index"
  :key="item.id"
/>
```

不自动将 `item` 注入组件的原因是，这会使组件与 `v-for` 的工作方式紧密耦合。明确其数据的来源可以使组件在其他情况下重用。

这里是一个简单的 [Todo List 的例子](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdCBzZXR1cD5cbmltcG9ydCB7IHJlZiB9IGZyb20gJ3Z1ZSdcbmltcG9ydCBUb2RvSXRlbSBmcm9tICcuL1RvZG9JdGVtLnZ1ZSdcbiAgXG5jb25zdCBuZXdUb2RvVGV4dCA9IHJlZignJylcbmNvbnN0IHRvZG9zID0gcmVmKFtcbiAge1xuICAgIGlkOiAxLFxuICAgIHRpdGxlOiAnRG8gdGhlIGRpc2hlcydcbiAgfSxcbiAge1xuICAgIGlkOiAyLFxuICAgIHRpdGxlOiAnVGFrZSBvdXQgdGhlIHRyYXNoJ1xuICB9LFxuICB7XG4gICAgaWQ6IDMsXG4gICAgdGl0bGU6ICdNb3cgdGhlIGxhd24nXG4gIH1cbl0pXG5cbmxldCBuZXh0VG9kb0lkID0gNFxuXG5mdW5jdGlvbiBhZGROZXdUb2RvKCkge1xuICB0b2Rvcy52YWx1ZS5wdXNoKHtcbiAgICBpZDogbmV4dFRvZG9JZCsrLFxuICAgIHRpdGxlOiBuZXdUb2RvVGV4dC52YWx1ZVxuICB9KVxuICBuZXdUb2RvVGV4dC52YWx1ZSA9ICcnXG59XG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuXHQ8Zm9ybSB2LW9uOnN1Ym1pdC5wcmV2ZW50PVwiYWRkTmV3VG9kb1wiPlxuICAgIDxsYWJlbCBmb3I9XCJuZXctdG9kb1wiPkFkZCBhIHRvZG88L2xhYmVsPlxuICAgIDxpbnB1dFxuICAgICAgdi1tb2RlbD1cIm5ld1RvZG9UZXh0XCJcbiAgICAgIGlkPVwibmV3LXRvZG9cIlxuICAgICAgcGxhY2Vob2xkZXI9XCJFLmcuIEZlZWQgdGhlIGNhdFwiXG4gICAgLz5cbiAgICA8YnV0dG9uPkFkZDwvYnV0dG9uPlxuICA8L2Zvcm0+XG4gIDx1bD5cbiAgICA8dG9kby1pdGVtXG4gICAgICB2LWZvcj1cIih0b2RvLCBpbmRleCkgaW4gdG9kb3NcIlxuICAgICAgOmtleT1cInRvZG8uaWRcIlxuICAgICAgOnRpdGxlPVwidG9kby50aXRsZVwiXG4gICAgICBAcmVtb3ZlPVwidG9kb3Muc3BsaWNlKGluZGV4LCAxKVwiXG4gICAgPjwvdG9kby1pdGVtPlxuICA8L3VsPlxuPC90ZW1wbGF0ZT4iLCJpbXBvcnQtbWFwLmpzb24iOiJ7XG4gIFwiaW1wb3J0c1wiOiB7XG4gICAgXCJ2dWVcIjogXCJodHRwczovL3NmYy52dWVqcy5vcmcvdnVlLnJ1bnRpbWUuZXNtLWJyb3dzZXIuanNcIlxuICB9XG59IiwiVG9kb0l0ZW0udnVlIjoiPHNjcmlwdCBzZXR1cD5cbmRlZmluZVByb3BzKFsndGl0bGUnXSlcbmRlZmluZUVtaXRzKFsncmVtb3ZlJ10pXG48L3NjcmlwdD5cblxuPHRlbXBsYXRlPlxuICA8bGk+XG4gICAge3sgdGl0bGUgfX1cbiAgICA8YnV0dG9uIEBjbGljaz1cIiRlbWl0KCdyZW1vdmUnKVwiPlJlbW92ZTwvYnV0dG9uPlxuICA8L2xpPlxuPC90ZW1wbGF0ZT4ifQ==)，展示了如何通过 `v-for` 来渲染一个组件列表，并向每个实例中传入不同的数据。

## 数组变化侦测

Vue 能够侦听响应式数组的变更方法，并在它们被调用时触发相关的更新。

相对地，也有一些不可变 (immutable) 方法，例如 `filter()`，`concat()` 和 `slice()`，这些都不会更改原数组，而总是**返回一个新数组**。当遇到的是非变更方法时，我们**需要将旧的数组替换为新的**：

```js
// `items` 是一个数组的 ref
items.value = items.value.filter((item) => item.message.match(/Foo/))
```

虽然替换了，但是Vue没有丢弃掉原有的Dom并重新渲染新的列表，而是最大化对Dom元素的重用，节省开销，牛逼。

### 展示过滤或排序后的结果

有时，我们希望显示数组经过过滤或排序后的内容，而不实际变更或重置原始数据。在这种情况下，你可以创建返回已过滤或已排序数组的计算属性。

```js
const numbers = ref([1, 2, 3, 4, 5])

const evenNumbers = computed(() => {
  return numbers.value.filter((n) => n % 2 === 0)
})
```

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```

在计算属性不可行的情况下 (例如在多层嵌套的 `v-for` 循环中)，你可以使用以下方法：

```js
const sets = ref([
  [1, 2, 3, 4, 5],
  [6, 7, 8, 9, 10]
])

function even(numbers) {
  return numbers.filter((number) => number % 2 === 0)
}
```

```html
<ul v-for="numbers in sets">
  <li v-for="n in even(numbers)">{{ n }}</li>
</ul>
```

在计算属性中使用 `reverse()` 和 `sort()` 的时候务必小心！这两个方法将变更原始数组，计算函数中不应该这么做。

## 事件处理

### 监听事件

我们可以使用 `v-on` 指令 (简写为 `@`) 来监听 DOM 事件，并在事件触发时执行对应的 JavaScript。用法：`v-on:click="methodName"` 或 `@click="handler"`。

事件处理器的值可以是：

1. **内联事件处理器**：事件被触发时执行的内联 JavaScript 语句 (与 `onclick` 类似)。
2. **方法事件处理器**：一个指向组件上定义的方法的属性名或是路径。

#### 内联事件处理器

内联事件处理器通常用于简单场景，例如：

```js
const count = ref(0)
```



```html
<button @click="count++">Add 1</button>
<p>Count is: {{ count }}</p>

```

#### 方法事件处理器

随着事件处理器的逻辑变得愈发复杂，内联代码方式变得不够灵活。因此 `v-on` 也可以接受一个方法名或对某个方法的调用。

```js
const name = ref('Vue.js')

function greet(event) {
  alert(`Hello ${name.value}!`)
  // `event` 是 DOM 原生事件
  if (event) {
    alert(event.target.tagName)
  }
}
```

```html
<!-- `greet` 是上面定义过的方法名 -->
<button @click="greet">Greet</button>
```

方法事件处理器会自动接收原生 DOM 事件并触发执行。在上面的例子中，我们能够通过被触发事件的 `event.target.tagName` 访问到该 DOM 元素。

### 在内联事件处理器中访问事件参数

@click标签会返回一个event对象，接收后可以在 `<script>` 中使用

```html
<!-- 使用特殊的 $event 变量 -->
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

<!-- 使用内联箭头函数 -->
<button @click="(event) => warn('Form cannot be submitted yet.', event)">
  Submit
</button>
```

```js
function warn(message, event) {
  // 这里可以访问原生事件
  if (event) {
    event.preventDefault()
  }
  alert(message)
}
```

### 事件修饰符

在处理事件时调用 `event.preventDefault()` 或 `event.stopPropagation()` 是很常见的。尽管我们可以直接在方法内调用，但如果方法能更专注于数据逻辑而不用去处理 DOM 事件的细节会更好。

为解决这一问题，Vue 为 `v-on` 提供了**事件修饰符**。修饰符是用 `.` 表示的指令后缀，包含以下这些：*修饰语可以使用链式书写*

- `.stop`: 单击事件停止传递
- `.prevent`: 提交事件将不再重新加载页面
- `.self`: 仅当 event.target 是元素本身时才会触发事件处理器
- `.capture`
- `.once`: 点击事件最多被触发一次
- `.passive`: 将立即发生而非等待事件完成

`.passive` 修饰符一般用于触摸事件的监听器，可以用来[改善移动端设备的滚屏性能](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener#使用_passive_改善滚屏性能)。

### 按键修饰符

在监听键盘事件时，我们经常需要检查特定的按键。Vue 允许在 `v-on` 或 `@` 监听按键事件时添加按键修饰符。

```html
<!-- 仅在 `key` 为 `Enter` 时调用 `submit` -->
<input @keyup.enter="submit" />
```

你可以直接使用 [`KeyboardEvent.key`](https://developer.mozilla.org/zh-CN/docs/Web/API/KeyboardEvent/key/Key_Values) 暴露的按键名称作为修饰符，但需要转为 kebab-case 形式。

#### 按键别名

Vue 为一些常用的按键提供了别名：

- `.enter`
- `.tab`
- `.delete` (捕获“Delete”和“Backspace”两个按键)
- `.esc`
- `.space`
- `.up`
- `.down`
- `.left`
- `.right`

#### 系统按键修饰符

你可以使用以下系统按键修饰符来触发鼠标或键盘事件监听器，只有当按键被按下时才会触发。

- `.ctrl`
- `.alt`
- `.shift`
- `.meta`

```html
<!-- Alt + Enter -->
<input @keyup.alt.enter="clear" />

<!-- Ctrl + 点击 -->
<div @click.ctrl="doSomething">Do something</div>

```

#### `.exact` 修饰符

`exact` 修饰符允许控制触发一个事件所需的确定组合的系统按键修饰符

```html
<!-- 当按下 Ctrl 时，即使同时按下 Alt 或 Shift 也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 仅当按下 Ctrl 且未按任何其他键时才会触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 仅当没有按下任何系统按键时触发 -->
<button @click.exact="onClick">A</button>
```

#### 鼠标按键修饰符

- `.left`
- `.right`
- `.middle`

这些修饰符将处理程序限定为由特定鼠标按键触发的事件。

### 表单输入绑定

v-model

#### 文本

#### 多行文本textarea

#### 复选框

我们也可以将多个复选框绑定到同一个数组或集合的值：

```js
const checkedNames = ref([])
```

```html
<div>Checked names: {{ checkedNames }}</div>

<input type="checkbox" id="name1" value="Jack" v-model="checkedNames">
<label for="jack">Jack</label>

<input type="checkbox" id="name2" value="John" v-model="checkedNames">
<label for="john">John</label>

<input type="checkbox" id="name3" value="Mike" v-model="checkedNames">
<label for="mike">Mike</label>
```

#### 单选框

```js
const picked = ref('');
```

```html
<div>Picked: {{ picked }}</div>

<input type="radio" id="one" value="One" v-model="picked" />
<label for="one">One</label>

<input type="radio" id="two" value="Two" v-model="picked" />
<label for="two">Two</label>
```

#### 选择器

##### 单选：


<div>Selected: {{ selected }}</div>
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
##### 多选：

<div>Selected: {{ selected }}</div><select v-model="selected" multiple>  <option>A</option>  <option>B</option>  <option>C</option>
</select>
#### 组件上的 `v-model` TODO深入组件

> 如果你还不熟悉 Vue 的组件，那么现在可以跳过这个部分。

HTML 的内置表单输入类型并不总能满足所有需求。幸运的是，我们可以使用 Vue 构建具有自定义行为的可复用输入组件，并且这些输入组件也支持 `v-model`！要了解更多关于此的内容，请在组件指引中阅读[配合 `v-model` 使用](https://cn.vuejs.org/guide/components/v-model.html)。<-深入组件

```html
// App.vue
<script setup>
import { ref } from 'vue'
import CustomInput from './CustomInput.vue'
  
const message = ref('hello')
</script>

<template>
  <CustomInput v-model="message" /> {{ message }}
  <CustomInput v-model="message" /> {{ message }}
  <CustomInput v-model="message" /> {{ message }}
</template>
```

```html
// CustomInput.vue
<script setup>
defineProps(['modelValue'])
defineEmits(['update:modelValue'])
</script>

<template>
  <input
    :value="modelValue"
    @input="$emit('update:modelValue', $event.target.value)"
  />
</template>
```





##  生命周期钩子

每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

### 注册周期钩子

举例来说，`onMounted` 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码：

```html
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 [`onMounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onmounted)、[`onUpdated`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onupdated) 和 [`onUnmounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onunmounted)。所有生命周期钩子的完整参考及其用法请参考 [API 索引](https://cn.vuejs.org/api/composition-api-lifecycle.html)。

注意：

当调用 `onMounted` 时，Vue 会自动将回调函数注册到当前正被初始化的组件实例上。这意味着这些钩子应当在组件初始化时被**同步**注册。例如，请不要这样做：

```js
setTimeout(() => {
  onMounted(() => {
    // 异步注册时当前组件实例已丢失
    // 这将不会正常工作
  })
}, 100)
```

注意这并不意味着对 `onMounted` 的调用必须放在 `setup()` 或 `` 内的词法上下文中。`onMounted()` 也可以在一个外部函数中调用，只要调用栈是同步的，且最终起源自 `setup()` 就可以。

### 生命周期图示

![组件生命周期图示](Vue学习入门.assets/lifecycle.16e4c08e.png)



## 侦听器

计算属性允许我们声明性地计算衍生值。然而在有些情况下，我们需要在状态变化时执行一些“副作用”：例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。

在组合式 API 中，我们可以使用 [`watch` 函数](https://cn.vuejs.org/api/reactivity-core.html#watch)在每次响应式状态发生变化时触发回调函数：

`watch()` 第一个参数可以是侦听数据源包括ref、计算属性、getter函数、或者多个数据源组成的数组。第二个参数作为一个回调函数，第一个参数接收侦听数据的改变值，第二个参数接收原来的值。

❗ 注意，你不能直接侦听响应式对象的属性值，例如:

```js
const obj = reactive({ count: 0 })

// 错误，因为 watch() 得到的参数是一个 number
watch(obj.count, (count) => {
  console.log(`count is: ${count}`)
})
```

这里需要用一个返回该属性的 getter 函数：

```js
// 提供一个 getter 函数
watch(
  () => obj.count,
  (count) => {
    console.log(`count is: ${count}`)
  }
)

```

### 深层侦听器

直接给 `watch()` 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的变更时都会被触发。

相比之下，一个返回响应式对象的 getter 函数，只有在返回不同的对象时，才会触发回调：

你也可以给上面这个例子显式地加上 `deep` 选项，强制转成深层侦听器。

```js
watch(
  () => state.someObject,
  (newValue, oldValue) => {
    // 注意：`newValue` 此处和 `oldValue` 是相等的
    // *除非* state.someObject 被整个替换了
  },
  { deep: true }
)
```



### 即时回调的侦听器

`watch` 默认是懒执行的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望在创建侦听器时，立即执行一遍回调。举例来说，我们想请求一些初始数据，然后在相关状态更改时重新请求数据。

我们可以通过传入 `immediate: true` 选项来强制侦听器的回调立即执行：

```js
watch(source, (newValue, oldValue) => {
  // 立即执行，且当 `source` 改变时再次执行
}, { immediate: true })
```

### `watchEffect()`

侦听器的回调使用与源完全相同的响应式状态是很常见的。例如下面的代码，在每当 `todoId` 的引用发生变化时使用侦听器来加载一个远程资源：

```js
const todoId = ref(1)
const data = ref(null)

watch(todoId, async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
}, { immediate: true })
```

特别是注意侦听器是如何两次使用 `todoId` 的，一次是作为源，另一次是在回调中。

我们可以用 `watchEffect` 函数来简化上面的代码。`watchEffect()` 允许我们自动跟踪回调的响应式依赖。上面的侦听器可以重写为：

```js
watchEffect(async () => {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/todos/${todoId.value}`
  )
  data.value = await response.json()
})
```

这个例子中，回调会立即执行，不需要指定 `immediate: true`。在执行期间，它会自动追踪 `todoId.value` 作为依赖（和计算属性类似）。每当 `todoId.value` 变化时，回调会再次执行。有了 `watchEffect()`，我们不再需要明确传递 `todoId` 作为源值。

对于这种只有一个依赖项的例子来说，`watchEffect()` 的**好处**相对较小。但是对于有多个依赖项的侦听器来说，使用 `watchEffect()` 可以消除手动维护依赖列表的负担。此外，如果你需要侦听一个嵌套数据结构中的几个属性，`watchEffect()` 可能会比深度侦听器更有效，因为它将只跟踪回调中被使用到的属性，而不是递归地跟踪所有的属性。

> `watch` vs. `watchEffect`
>
> `watch` 和 `watchEffect` 都能响应式地执行有副作用的回调。它们之间的主要区别是追踪响应式依赖的方式：
>
> - `watch` 只追踪明确侦听的数据源。它不会追踪任何在回调中访问到的东西。另外，仅在数据源确实改变时才会触发回调。`watch` 会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机。
> - `watchEffect`，则会在副作用发生期间追踪依赖。它会在同步执行过程中，自动追踪所有能访问到的响应式属性。这更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确。

### 回调的触发时机

当你更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调。

默认情况下，用户创建的侦听器回调，都会在 Vue 组件更新**之前**被调用。这意味着你在侦听器回调中访问的 DOM 将是被 Vue 更新之前的状态。

如果想在侦听器回调中能访问被 Vue 更新**之后**的 DOM，你需要指明 `flush: 'post'` 选项：

```js
watch(source, callback, {
  flush: 'post'
})

watchEffect(callback, {
  flush: 'post'
})
```

后置刷新的 `watchEffect()` 有个更方便的别名 `watchPostEffect()`：

```js
import { watchPostEffect } from 'vue'

watchPostEffect(() => {
  /* 在 Vue 更新后执行 */
})
```

### 停止侦听器

在 `setup()` 或 `` 中用同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止。因此，在大多数情况下，你无需关心怎么停止一个侦听器。

一个关键点是，侦听器必须用**同步**语句创建：如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防**内存泄漏**。

```js
<script setup>
import { watchEffect } from 'vue'

// 它会自动停止
watchEffect(() => {})

// ...这个则不会！
setTimeout(() => {
  watchEffect(() => {})
}, 100)
</script>
```

要手动停止一个侦听器，请调用 `watch` 或 `watchEffect` 返回的函数：

```js
const unwatch = watchEffect(() => {})

// ...当该侦听器不再需要时
unwatch()
```

注意，需要异步创建侦听器的情况很少，请尽可能选择同步创建。如果需要等待一些异步数据，你可以使用条件式的侦听逻辑。

## 模板引用

虽然 Vue 的声明性渲染模型为你抽象了大部分对 DOM 的直接操作，但在某些情况下，我们仍然需要直接访问底层原生 DOM 元素。要实现这一点，我们可以使用特殊的 `ref` attribute：

```html
//app挂载后将焦点放在input上
<script setup>
import { ref, onMounted } from 'vue'

// 声明一个 ref 来存放该元素的引用
// 必须和模板里的 ref 同名
const input = ref(null)

onMounted(() => {
  input.value.focus()
})
</script>

<template>
  <input ref="input" />
</template>
```

### `v-for` 中的模板引用

当在 `v-for` 中使用模板引用时，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素。

应该注意的是，ref 数组**并不**保证与源数组相同的顺序。

### 函数模板引用

除了使用字符串值作名字，`ref` attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：

```html
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }">
```

注意我们这里需要使用动态的 `:ref` 绑定才能够传入一个函数。当绑定的元素被卸载时，函数也会被调用一次，此时的 `el` 参数会是 `null`。你当然也可以绑定一个组件方法而不是内联函数。

### 组件上的 ref

> 这一小节假设你已了解[组件](https://cn.vuejs.org/guide/essentials/component-basics.html)的相关知识，或者你也可以先跳过这里，之后再回来看。

模板引用也可以被用在一个子组件上。这种情况下引用中获得的值是组件实例：

```html
<script setup>
import { ref, onMounted } from 'vue'
import Child from './Child.vue'

const child = ref(null)

onMounted(() => {
  // child.value 是 <Child /> 组件的实例
})
</script>

<template>
  <Child ref="child" />
</template>
```

如果一个子组件使用的是选项式 API 或没有使用`<script setup>`，被引用的组件实例和该子组件的 `this` 完全一致，这意味着父组件对子组件的每一个属性和方法都有完全的访问权。这使得在父组件和子组件之间创建紧密耦合的实现细节变得很容易，当然也因此，应该只在绝对需要时才使用组件引用。大多数情况下，你应该首先使用标准的 `props` 和 `emit` 接口来实现父子组件交互。

有一个例外的情况，使用了`<script setup>`的组件是**默认私有**的：一个父组件无法访问到一个使用了`<script setup>`的子组件中的任何东西，除非子组件在其中通过 `defineExpose` 宏显式暴露：

```html
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

// 像 defineExpose 这样的编译器宏不需要导入
defineExpose({
  a,
  b
})
</script>
```

当父组件通过模板引用获取到了该组件的实例时，得到的实例类型为 `{ a: number, b: number }` (ref 都会自动解包，和一般的实例一样)。

## 组件基础

### 传递 props

如果我们正在构建一个博客，我们可能需要一个表示博客文章的组件。我们希望所有的博客文章分享相同的视觉布局，但有不同的内容。要实现这样的效果自然必须向组件中传递数据，例如每篇文章标题和内容，这就会使用到 props。

Props 是一种特别的 attributes，你可以在组件上声明注册。要传递给博客文章组件一个标题，我们必须在组件的 props 列表上声明它。这里要用到 [`defineProps`](https://cn.vuejs.org/api/sfc-script-setup.html#defineprops-defineemits) 宏：

```html
<!-- BlogPost.vue -->
<script setup>
defineProps(['title'])
</script>

<template>
  <h4>{{ title }}</h4>
</template>
```

在实际应用中，我们可能在父组件中会有如下的一个博客文章数组：

```js
const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])
```

```html
<BlogPost
  v-for="post in posts"
  :key="post.id"
  :title="post.title"
 />
```

### 监听事件

```html
// App.vue
<script setup>
import { ref } from 'vue'
import BlogPost from './BlogPost.vue'
  
const posts = ref([
  { id: 1, title: 'My journey with Vue' },
  { id: 2, title: 'Blogging with Vue' },
  { id: 3, title: 'Why Vue is so fun' }
])

const postFontSize = ref(1)
</script>

<template>
	<div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPost
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      @enlarge-text="postFontSize += 0.1"
    ></BlogPost>
  </div>
</template>
```

子组件可以通过调用内置的 `$emit` 方法，通过传入事件名称来抛出一个事件。

因为有了 `@enlarge-text="postFontSize += 0.1"` 的监听，父组件会接收这一事件，从而更新 `postFontSize` 的值。

```html
// BlogPost.vue
<script setup>
defineProps(['title'])
defineEmits(['enlarge-text'])
</script>

<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```

### 通过插槽来分配内容

有时候我们需要在子组件接收父组件通过html传递的内容，可以在子组件中使用 `</slot>` **占位符**接收html文本。

```html
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot /> <!-- 这里会插入来自父组件的文本 -->
  </div>
</template>

<style scoped>
.alert-box {
  /* 通过这里改变父组件文本的样式 */ 
}
</style>
```

### 动态组件

有些场景会需要在两个组件间来回切换，比如 Tab 界面：

```html
// App.vue
<script setup>
import Home from './Home.vue'
import Posts from './Posts.vue'
import Archive from './Archive.vue'
import { ref } from 'vue'
 
const currentTab = ref('Home')

const tabs = {
  Home,
  Posts,
  Archive
}
</script>

<template>
  <div class="demo">
    <button
       v-for="(_, tab) in tabs"
       :key="tab"
       :class="['tab-button', { active: currentTab === tab }]"
       @click="currentTab = tab"
     >
      {{ tab }}
    </button>
	  <component :is="tabs[currentTab]" class="tab"></component>
  </div>
</template>

<style>
.demo {
  font-family: sans-serif;
  border: 1px solid #eee;
  border-radius: 2px;
  padding: 20px 30px;
  margin-top: 1em;
  margin-bottom: 40px;
  user-select: none;
  overflow-x: auto;
}

.tab-button {
  padding: 6px 10px;
  border-top-left-radius: 3px;
  border-top-right-radius: 3px;
  border: 1px solid #ccc;
  cursor: pointer;
  background: #f0f0f0;
  margin-bottom: -1px;
  margin-right: -1px;
}
.tab-button:hover {
  background: #e0e0e0;
}
.tab-button.active {
  background: #e0e0e0;
}
.tab {
  border: 1px solid #ccc;
  padding: 10px;
}
</style>
```

子组件：

```html
// Home.vue
<template>
  <div class="tab">
    Home component
  </div>
</template>
// Posts.vue
<template>
  <div class="tab">
    Posts component
  </div>
</template>
// Archive.vue
<template>
  <div class="tab">
    Archive component
  </div>
</template>
```

### 闭合标签

这是因为 Vue 的模板解析器支持任意标签使用 `/>` 作为标签关闭的标志。

然而在 DOM 模板中，我们必须显式地写出关闭标签：

```html
<MyComponent />
// 显式地写出关闭标签
<my-component></my-component>
```

# VueRouter

简单来说就是为了实现不刷新页面也不发送请求，更新页面的内容。

Router有自己的目录，在目录中有一个出口文件index.js，其中需要有路由表（一个数组，将请求路径和视图绑定），默认导出由VueRouter提供的 `createRouter()` 方法创建的router。

需要在main.js中引入router并使用到app上， `app.use(router)` 再将app挂载到实例上。 

app.vue上使用 `router-link` 标签和 `router-view` 标签占位，前者需要使用to属性指定请求路径，后者为路径绑定的对应视图（路由表对应）

## Home-About-NotFound演示

1、 `src/router/index.js` 文件

```js
// 引入vuerouter所需要的包
import { createRouter, createWebHashHistory } from "vue-router";
// 引入视图
// import { About, Home } from "../views";
import Home from '../views/Home.vue'
import About from '../views/About.vue'
import NotFound from "../views/NotFound.vue"

// 定义路由表
const routes = [
  { path: "/", component: Home },
  { path: "/About", component: About },
    // path的值含义为，上面匹配不到的我全部匹配得到
    // :path表示动态匹配类似:id, (.*)为任意字符串
  { path: "/:path(.*)", component: NotFound }
];

// 创建路由
const router = createRouter({
  history: createWebHashHistory(),
  routes: routes,
});

// 导出路由 默认
export default router;
```

2、 `src/App.vue` 文件

```html
<script setup></script>

<template>
  <router-link to="/">GoToHome</router-link><br />
  <router-link to="/about">GoToAbout</router-link>
  <!-- router-link相当于占位，路径改变，内容改变 -->
  <router-view></router-view>
</template>
```

3、 `src/main.js` 文件

```js
import { createApp } from "vue";
import App from "./App.vue";
// 引入路由
import  router from "./router";

const app = createApp(App);
// 将路由使用到app上
app.use(router)
// 先使用后挂载到实例！
app.mount("#app");
```

4、 `views` 文件（包括 `Home.vue` 、 `About.vue` 、 `NotFound.vue` ）

在src下创建views文件，将视图存放在里面。

```html
<template>
  <h1>This is Home</h1>
</template>
```

```html
<template>
  <h1>This is About</h1>
</template>
```

```html
<template>
  <h1>Not Found</h1>
</template>
```

5、yarn dev | npm run

## 动态路由匹配

### 带参数的动态路由匹配

```js
import User from "../views/User.vue";

// router/index.js中定义路由表
const routes = [
  { path: "/", component: Home },
  { path: "/about", component: About },
    // 此处就是对带参数的动态路由匹配
  { path: "/user/:id", component: User },
];
```

对该参数进行操作，需要在 `User.vue` 下使用。

```html
<script setup>
import { useRoute } from "vue-router";
// setup可以通过vue-router提供的useRoute方法创建route对象，获取params
const route = useRoute();
console.log(route.params);
</script>
```

你可以在同一个路由中设置有多个 *路径参数*，它们会映射到 `route.params` 上的相应字段。

| 模板                             | 输入                       | 输出                                     |
| -------------------------------- | -------------------------- | ---------------------------------------- |
| `/users/:username/posts/:postId` | `users/eduardo/posts/123 ` | `{ username: 'eduardo', postId: '123' }` |

### 404页面

除了规定的路径，其他的路径统一设置为 `NotFound` 页面。可以简单的使用

```js
// 定义路由表
const routes = [
  { path: "/", component: Home },
  { path: "/About", component: About },
    // path的值含义为，上面匹配不到的我全部匹配得到
    // :path表示动态匹配类似:id, (.*)为任意字符串
  { path: "/:path(.*)", component: NotFound }
];
```

## 路由上使用正则

拿B站做例子，你想进入某人的主页，通过修改自己的主页的路径中uid的值即可找到对应uid的主页，当你的uid输入为非数值则会显示NotFound。这里的实现过程我们就可以在路由上使用正则，让参数只能是数字。

```js
const routes = [
  // /:orderId -> 仅匹配数字
  { path: '/:orderId(\\d+)', component },
  // /:productName -> 匹配其他任何内容
  { path: '/:productName', component },
]
```

现在，转到 `/25` 将匹配 `/:orderId`，其他情况将会匹配 `/:productName`。`routes` 数组的顺序并不重要!

参数虽然可以指定为仅匹配数字，却不能让 `route.params` 接受的时候成为数字，其值仍然为字符串。

### 可重复的参数

如果你需要匹配具有多个部分的路由，如 `/first/second/third`，你应该用 `*`（0 个或多个）和 `+`（1 个或多个）将参数标记为可重复：

```js
const routes = [
  // /:chapters ->  匹配 /one, /one/two, /one/two/three, 等
  { path: '/:chapters+', component },
  // /:chapters -> 匹配 /, /one, /one/two, /one/two/three, 等
  { path: '/:chapters*', component },
]
```

这将为你提供一个参数数组，而不是一个字符串。

### Sensitive 与 strict 路由配置(!important)

默认情况下，所有路由是不区分大小写的，并且能匹配带有或不带有尾部斜线的路由。例如，路由 `/users` 将匹配 `/users`、`/users/`、甚至 `/Users/`。这种行为可以通过 `strict` 和 `sensitive` 选项来修改，它们可以既可以应用在整个全局路由上，又可以应用于当前路由上：

```js
const router = createRouter({
  history: createWebHistory(),
  routes: [
    // 将匹配 /users/posva 而非：
    // - /users/posva/ 当 strict: true
    // - /Users/posva 当 sensitive: true
    { path: '/users/:id', sensitive: true },
    // 将匹配 /users, /Users, 以及 /users/42 而非 /users/ 或 /users/42/
    { path: '/users/:id?' },
  ]
  strict: true, // applies to all routes
})
```

### 可选参数

你也可以通过使用 `?` 修饰符(0 个或 1 个)将一个参数标记为可选：

```js
const routes = [
  // 匹配 /users 和 /users/posva
  { path: '/users/:userId?', component },
  // 匹配 /users 和 /users/42
  { path: '/users/:userId(\\d+)?', component },
]
```

原先如果不加userid的话就会进入NOTFOUND界面

## 嵌套路由（important）

一些应用程序的 UI 由多层嵌套的组件组成。在这种情况下，URL 的片段通常对应于特定的嵌套组件结构，例如：

```shell
/user/johnny/profile                     /user/johnny/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

第一层的 `router-view` 渲染顶层路由匹配的组件。同样地，一个被渲染的组件也可以包含自己嵌套的 `router-view` 。例如，如果我们在 `User` 组件的模板内添加一个 `router-view`：

User.vue文件

```html
<template>
  <h1>THIS USER'S ID IS {{ route.params.id }}</h1>
  <!-- 嵌套的router-view -->
  <router-view></router-view>
</template>
```

index.js文件

```js
const routes = [
  {
    path: '/user/:id',
    component: User,
    // 在此处添加一个children的属性
    children: [
      {
        // 当 /user/:id/profile 匹配成功
        // UserProfile 将被渲染到 User 的 <router-view> 内部
        path: 'profile',
        component: UserProfile,
      },
      {
        // 当 /user/:id/posts 匹配成功
        // UserPosts 将被渲染到 User 的 <router-view> 内部
        path: 'posts',
        component: UserPosts,
      },
    ],
  },
]
```

**注意，以 `/` 开头的嵌套路径将被视为根路径。这允许你利用组件嵌套，而不必使用嵌套的 URL。**

如你所见，`children` 配置只是另一个路由数组，就像 `routes` 本身一样。因此，你可以根据自己的需要，不断地嵌套视图。

### 嵌套的命名路由

在处理[命名路由](https://router.vuejs.org/zh/guide/essentials/named-routes.html)时，**你通常会给子路由命名**：

```js
const routes = [
  {
    path: '/user/:id',
    component: User,
    // 请注意，只有子路由具有名称
    children: [{ path: '', name: 'user', component: UserHome }],
  },
]
```

这将确保导航到 `/user/:id` 时始终显示嵌套路由。

在一些场景中，你可能希望导航到命名路由而不导航到嵌套路由。例如，你想导航 `/user/:id` 而不显示嵌套路由。那样的话，你还可以**命名父路由**，但请注意**重新加载页面将始终显示嵌套的子路由**，因为它被视为指向路径`/users/:id` 的导航，而不是命名路由：

```js
const routes = [
  {
    path: '/user/:id',
    name: 'user-parent',
    component: User,
    children: [{ path: '', name: 'user', component: UserHome }],
  },
]
```

## 编程式导航

除了使用 `router-link` 创建 a 标签来定义导航链接，我们还可以借助 router 的实例方法，通过编写代码来实现。

想要导航到不同的 URL，可以使用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，会回到之前的 URL。当你点击 `<router-link>` 时，内部会调用这个方法，所以点击 `<router-link>` 相当于调用 `router.push(...)` ：

我觉得不如router-link

### 导航到不同的位置

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：

```js
// 字符串路径
router.push('/users/eduardo')

// 带有路径的对象
router.push({ path: '/users/eduardo' })

// 命名的路由，并加上参数，让路由建立 url
router.push({ name: 'user', params: { username: 'eduardo' } })

// 带查询参数，结果是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// 带 hash，结果是 /about#team
router.push({ path: '/about', hash: '#team' })
```

**注意**：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path` ：

```js
const username = 'eduardo'
// 我们可以手动建立 url，但我们必须自己处理编码
router.push(`/user/${username}`) // -> /user/eduardo
// 同样
router.push({ path: `/user/${username}` }) // -> /user/eduardo
// 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
router.push({ name: 'user', params: { username } }) // -> /user/eduardo
// `params` 不能与 `path` 一起使用
router.push({ path: '/user', params: { username } }) // -> /user

```

当指定 `params` 时，可提供 `string` 或 `number` 参数（或者对于[可重复的参数](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#repeatable-params)可提供一个数组）。**任何其他类型（如 `undefined`、`false` 等）都将被自动字符串化**。对于[可选参数](https://router.vuejs.org/zh/guide/essentials/route-matching-syntax.html#repeatable-params)，你可以提供一个空字符串（`""`）来跳过它。

由于属性 `to` 与 `router.push` 接受的对象种类相同，所以两者的规则完全相同。

#### ↑↑总结  别用 用了会不幸



### 替换当前位置

它的作用类似于 `router.push`，唯一不同的是，它在导航时**不会向 history 添加新记录**，正如它的名字所暗示的那样——它取代了当前的条目。

`<router-link :to="..." replace>`  或者是 `router.replace(...)`

### 横跨历史

该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步，类似于 `window.history.go(n)`。

```js
// 向前移动一条记录，与 router.forward() 相同
router.go(1)

// 返回一条记录，与 router.back() 相同
router.go(-1)

// 前进 3 条记录
router.go(3)

// 如果没有那么多记录，静默失败
router.go(-100)
router.go(100)
```



### 篡改历史

你可能已经注意到，`router.push`、`router.replace` 和 `router.go` 是 [`window.history.pushState`、`window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History) 的翻版，它们确实模仿了 `window.history` 的 API。

因此，如果你已经熟悉 [Browser History APIs](https://developer.mozilla.org/en-US/docs/Web/API/History_API)，在使用 Vue Router 时，操作历史记录就会觉得很熟悉。

值得一提的是，无论在创建路由器实例时传递什么样的 [`history` 配置](https://router.vuejs.org/zh/api/#history)，Vue Router 的导航方法( `push`、`replace`、`go` )都能始终正常工作。

## 命名路由

除了 `path` 之外，你还可以为任何路由提供 `name`。这有以下优点：

- 没有硬编码的 URL
- `params` 的自动编码/解码。
- 防止你在 url 中出现打字错误。
- 绕过路径排序（如显示一个）

在路由表中，你可以不指定路径了，可以指定某个名字。

```js
const routes = [
  {
    path: '/user/:username',
    name: 'user',
    component: User,
  },
]
```

要链接到一个命名的路由，可以向 `router-link` 组件的 `to` 属性传递一个对象：

```html
<router-link :to="{ name: 'user', params: { username: 'erina' }}">
  User
</router-link>
```

这跟代码调用 `router.push()` 是一回事：

```js
router.push({ name: 'user', params: { username: 'erina' } })
```

在这两种情况下，路由将导航到路径 `/user/erina`。

完整的例子在[这里](https://github.com/vuejs/vue-router/blob/dev/examples/named-routes/app.js).

## 命名视图(important)

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 `router-view` 没有设置名字，那么默认为 `default`。

```html
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 **s**)：

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        // LeftSidebar: LeftSidebar 的缩写
        LeftSidebar,
        // 它们与 `<router-view>` 上的 `name` 属性匹配
        RightSidebar,
      },
    },
  ],
})
```

### 嵌套命名视图

我们也有可能使用命名视图创建嵌套视图的复杂布局。这时你也需要命名用到的嵌套 `router-view` 组件。我们以一个设置面板为例：

```shell
/settings/emails                                   /settings/profile
+-----------------------------------+              +------------------------------+
| UserSettings                      |              | UserSettings                 |
| +-----+-------------------------+ |              | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +-------->  | | Nav | UserProfile        | |
| |     +-------------------------+ |              | |     +--------------------+ |
| |     |                         | |              | |     | UserProfilePreview | |
| +-----+-------------------------+ |              | +-----+--------------------+ |
+-----------------------------------+              +------------------------------+

```

- `Nav` 只是一个常规组件。
- `UserSettings` 是一个视图组件。
- `UserEmailsSubscriptions`、`UserProfile`、`UserProfilePreview` 是嵌套的视图组件。

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar />
  <router-view />
  <router-view name="helper" />
</div>
```

```js
import UserSettings from "../views/UserSettings.vue";

const routes = [{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}]
```

## 重定向和别名

### 重定向

使用 `redirect=""` 通过 `routes` 配置来完成，下面例子是从 `/home` 重定向到 `/`：

```js
const routes = [{ path: '/home', redirect: '/' }]
```

重定向的目标也可以是一个命名的路由：

```js
const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
```

甚至是一个方法，动态返回重定向目标：

```js
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
```

### 别名

**将 `/` 别名为 `/home`，意味着当用户访问 `/home` 时，URL 仍然是 `/home`，但会被匹配为用户正在访问 `/`。**

```js
const routes = [{ path: '/', component: Homepage, alias: '/home' }]
```

通过别名，你可以自由地将 UI 结构映射到一个任意的 URL，而不受配置的嵌套结构的限制。使别名以 `/` 开头，以使嵌套路径中的路径成为**绝对路径**。你甚至可以将两者结合起来，用一个数组提供多个别名：

```js
const routes = [
  {
    path: '/users',
    component: UsersLayout,
    children: [
      // 为这 3 个 URL 呈现 UserList
      // - /users
      // - /users/list
      // - /people
      { path: '', component: UserList, alias: ['/people', 'list'] },
    ],
  },
]
```

如果你的路由有参数，请确保在任何绝对别名中包含它们。

```js
const routes = [
  {
    path: '/users/:id',
    component: UsersByIdLayout,
    children: [
      // 为这 3 个 URL 呈现 UserDetails
      // - /users/24
      // - /users/24/profile
      // - /24
      { path: 'profile', component: UserDetails, alias: ['/:id', ''] },
    ],
  },
]
```

## 路由组件传参

在你的组件中使用 `useRoute()` (选项式的话就可以不使用 `$route.params` ，而是先 `props: ['id']` 再直接从 `this.id` 中获取)会与路由紧密耦合，这限制了组件的灵活性，因为它只能用于特定的 URL。虽然这不一定是件坏事，但我们可以通过 `props` 配置来解除这种行为：

```js
const routes = [{ path: '/user/:id', component: User, props: true }] // props属性定义为true
```

在组件的语法糖中就可以使用 `defineProps({id: String})` 接收。

## 不同的历史记录模式 ! importat

hash模式和h5模式，区别在于前者会在路径上加个#号

## 导航守卫

在页面跳转的期间需要进行操作时就可以使用导航守卫。

### 全局前置守卫

可以使用 `router.beforeEach` 注册一个全局前置守卫: 

```js
const router = createRouter({ ... })

router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  return false
})
```

三个参数：to，from，next放行函数。设置了 `next` 参数的话，需要调用 `next()` 方法才能进入到from组件。

### 每路守卫

每个路由独享的守卫

```js
{
  path: "/chapters/:id",
  beforeEnter: (to, from) => {
    console.log(to);
    console.log(from);
  },
}
```

## 路由懒加载

使用到路由相对应的组件时才加载。

当打包构建应用时，JavaScript 包会变得非常大，影响页面加载。如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就会更加高效。

Vue Router 支持开箱即用的[动态导入](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#Dynamic_Imports)，这意味着你可以用动态导入代替静态导入：

```js
// 将
// import UserDetails from './views/UserDetails.vue'
// 替换成
const UserDetails = () => import('./views/UserDetails.vue')

const router = createRouter({
  // ...
  routes: [{ path: '/users/:id', component: UserDetails }],
})
```

