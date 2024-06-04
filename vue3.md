# Vue3 快速上手

<img src="https://user-images.githubusercontent.com/499550/93624428-53932780-f9ae-11ea-8d16-af949e16a09f.png" style="width:200px" />

## 1.Vue3 简介

- 2020 年 9 月 18 日，Vue.js 发布 3.0 版本，代号：One Piece（海贼王）
- 耗时 2 年多、[2600+次提交](https://github.com/vuejs/vue-next/graphs/commit-activity)、[30+个 RFC](https://github.com/vuejs/rfcs/tree/master/active-rfcs)、[600+次 PR](https://github.com/vuejs/vue-next/pulls?q=is%3Apr+is%3Amerged+-author%3Aapp%2Fdependabot-preview+)、[99 位贡献者](https://github.com/vuejs/vue-next/graphs/contributors)
- github 上的 tags 地址：https://github.com/vuejs/vue-next/releases/tag/v3.0.0
- Vue3 支持 vue2 的大多数特性

## 2.Vue3 带来了什么

### 1.性能的提升

- 打包大小减少 41%

- 初次渲染快 55%, 更新渲染快 133%

- 内存减少 54%

  ......

### 2.源码的升级

- 使用 Proxy 代替 defineProperty 实现响应式

- 重写虚拟 DOM 的实现和 Tree-Shaking

  ......

### 3.拥抱 TypeScript

- Vue3 可以更好的支持 TypeScript

### 4.新的特性

1. Composition API（组合 API）

   - setup 配置
   - ref 与 reactive
   - watch 与 watchEffect
   - provide 与 inject
   - ......

2. 新的内置组件

   - Fragment - 文档碎片
   - Teleport - 瞬移组件的位置
   - Suspense - 异步加载组件的 loading 界面

3. 其他改变

   - 新的生命周期钩子
   - data 选项应始终被声明为一个函数
   - 移除 keyCode 支持作为 v-on 的修饰符
   - ......

# 一、创建 Vue3.0 工程

## 1.使用 vue-cli 创建

官方文档：https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create

```bash
## 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上
vue --version
## 安装或者升级你的@vue/cli
npm install -g @vue/cli
## 创建
vue create vue_test
## 启动
cd vue_test
npm run serve
```

## 2.使用 vite 创建

官方文档：https://v3.cn.vuejs.org/guide/installation.html#vite

vite 官网：https://vitejs.cn

- 什么是 vite？—— 新一代前端构建工具。
- 优势如下：
  - 开发环境中，无需打包操作，可快速的冷启动。
  - 轻量快速的热重载（HMR）。
  - 真正的按需编译，不再等待整个应用编译完成。

```bash
## 创建工程
npm init vite-app <project-name>
## 进入工程目录
cd <project-name>
## 安装依赖
npm install
## 运行
npm run dev
```

# 二、常用 Composition API

官方文档: https://v3.cn.vuejs.org/guide/composition-api-introduction.html

## 1.setup

1. 理解：Vue3.0 中一个新的配置项，值为一个函数。
2. setup 是所有<strong style="color:#DD5145">Composition API（组合 API）</strong><i style="color:gray;font-weight:bold">“ 表演的舞台 ”</i>。
3. 组件中所用到的：数据、方法等等，均要配置在 setup 中。
4. setup 函数的两种返回值：
   1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用。（重点关注！）
   2. <span style="color:#aad">若返回一个渲染函数：则可以自定义渲染内容。（了解）</span>
5. 注意点：
   1. 尽量不要与 Vue2.x 配置混用
      - Vue2.x 配置（data、methos、computed...）中<strong style="color:#DD5145">可以访问到</strong>setup 中的属性、方法。
      - 但在 setup 中<strong style="color:#DD5145">不能访问到</strong>Vue2.x 配置（data、methos、computed...）。
      - 如果有重名, setup 优先。
   2. setup 不能是一个 async 函数，因为返回值不再是 return 的对象, 而是 promise, 模板看不到 return 对象中的属性。（后期也可以返回一个 Promise 实例，但需要 Suspense 和异步组件的配合）

## 2.ref 函数

- 作用: 定义一个响应式的数据

- 语法: `const xxx = ref(initValue)`

  - 创建一个包含响应式数据的<strong style="color:#DD5145">引用对象（reference 对象，简称 ref 对象）</strong>。
  - JS 中操作数据： `xxx.value`
  - 模板中读取数据: 不需要.value，直接：`<div>{{xxx}}</div>`

- 备注：

  - 接收的数据可以是：基本类型、也可以是对象类型。
  - 基本类型的数据：响应式依然是靠`Object.defineProperty()`的`get`与`set`完成的。
  - 对象类型的数据：内部 <i style="color:gray;font-weight:bold">“ 求助 ”</i> 了 Vue3.0 中的一个新函数—— `reactive`函数。

  ```js
  <template>
    <h2>{{count}}</h2>
    <hr>
    <button @click="update">更新</button>
  </template>

  <script>
  import { ref } from 'vue'
  export default {

    /* 在Vue3中依然可以使用data和methods配置, 但建议使用其新语法实现 */
    // data () {
    //   return {
    //     count: 0
    //   }
    // },
    // methods: {
    //   update () {
    //     this.count++
    //   }
    // }

    /* 使用vue3的composition API */
    setup () {

      // 定义响应式数据 ref对象
      const count = ref(1)
      console.log(count)

      // 更新响应式数据的函数
      function update () {
        // alert('update')
        count.value = count.value + 1
      }

      return {
        count,
        update
      }
    }
  }
  </script>
  ```

## 3.reactive 函数

- 作用: 定义一个<strong style="color:#DD5145">对象类型</strong>的响应式数据（基本类型不要用它，要用`ref`函数）
- 语法：`const 代理对象= reactive(源对象)`接收一个对象（或数组），返回一个<strong style="color:#DD5145">代理对象（Proxy 的实例对象，简称 proxy 对象）</strong>
- reactive 定义的响应式数据是“深层次的”。
- 内部基于 ES6 的 Proxy 实现，通过代理对象操作源对象内部数据进行操作。

```js
<template>
  <h2>name: {{state.name}}</h2>
  <h2>age: {{state.age}}</h2>
  <h2>wife: {{state.wife}}</h2>
  <hr>
  <button @click="update">更新</button>
</template>

<script>
import {
  reactive,
} from 'vue'
export default {
  setup () {
    /*     定义响应式数据对象    */
    const state = reactive({
      name: 'tom',
      age: 25,
      wife: {
        name: 'marry',
        age: 22
      },
    })

    const update = () => {
      state.name += '--'
      state.age += 1
      state.wife.name += '++'
      state.wife.age += 2
    }

    return {
      state,
      update,
    }
  }
}
</script>
```

## 4.Vue3.0 中的响应式原理

### vue2.x 的响应式

- 实现原理：

  - 对象类型：通过`Object.defineProperty()`对属性的读取、修改进行拦截（数据劫持）。

  - 数组类型：通过重写更新数组的一系列方法来实现拦截。（对数组的变更方法进行了包裹）。

    ```js
    Object.defineProperty(data, 'count', {
      get() {},
      set() {},
    })
    ```

- 存在问题：

  - 新增属性、删除属性, 界面不会更新。
  - 直接通过下标修改数组, 界面不会自动更新。

### Vue3.0 的响应式

- 实现原理:

  - 通过 Proxy（代理）: 拦截对象中任意属性的变化, 包括：属性值的读写、属性的添加、属性的删除等。

  - 通过 Reflect（反射）: 对源对象的属性进行操作。

  - 文档中描述的 Proxy 与 Reflect：

    - Proxy：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy

    - Reflect：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect

      ```js
      new Proxy(data, {
        // 拦截读取属性值
        get(target, prop) {
          return Reflect.get(target, prop)
        },
        // 拦截设置属性值或添加新属性
        set(target, prop, value) {
          return Reflect.set(target, prop, value)
        },
        // 拦截删除属性
        deleteProperty(target, prop) {
          return Reflect.deleteProperty(target, prop)
        },
      })

      proxy.name = 'tom'
      ```

### 使用 Proxy 替代 Object.defineProperty

为什么使用  `Proxy`  替代  `Object.defineProperty`，简单总结`Proxy`的几点优势

- Proxy 是对整个对象的代理，而 Object.defineProperty 只能代理某个属性

- 对象上新增属性，Proxy 可以监听到，Object.defineProperty 不能

- 数组新增修改，Proxy 可以监听到，Object.defineProperty 不能

- 若对象内部属性要全部递归代理，Proxy 可以只在调用的时候递归，而 Object.definePropery 需要一次完成所有递归，性能比 Proxy 差

使用也很简单，`Proxy`本质是构造函数，通过 new 即可产生对象，它接收两个参数：

- `target`表示的就是要拦截（代理）的目标对象

- `handler`是用来定制拦截行为（**13 种**）

## 5.reactive 对比 ref

- 从定义数据角度对比：

  - ref 用来定义：<strong style="color:#DD5145">基本类型数据</strong>。
  - reactive 用来定义：<strong style="color:#DD5145">对象（或数组）类型数据</strong>。
  - 备注：ref 也可以用来定义<strong style="color:#DD5145">对象（或数组）类型数据</strong>, 它内部会自动通过`reactive`转为<strong style="color:#DD5145">代理对象</strong>。

- 从原理角度对比：

  - ref 通过`Object.defineProperty()`的`get`与`set`来实现响应式（数据劫持）。
  - reactive 通过使用<strong style="color:#DD5145">Proxy</strong>来实现响应式（数据劫持）, 并通过<strong style="color:#DD5145">Reflect</strong>操作<strong style="color:orange">源对象</strong>内部的数据。

- 从使用角度对比：

  - ref 定义的数据：操作数据<strong style="color:#DD5145">需要</strong>`.value`，读取数据时模板中直接读取<strong style="color:#DD5145">不需要</strong>`.value` (内部解析模板时会自动添加.value)。
  - reactive 定义的数据：操作数据与读取数据：<strong style="color:#DD5145">均不需要</strong>`.value`。

  ```js
  <template>
    <h2>App</h2>
    <p>m1: {{m1}}</p>
    <p>m2: {{m2}}</p>
    <p>m3: {{m3}}</p>
    <button @click="update">更新</button>
  </template>

  <script lang="ts">
  import {
    reactive,
    ref
  } from 'vue'

  export default {

    setup () {
      const m1 = ref('abc')
      const m2 = reactive({x: 1, y: {z: 'abc'}})

      // 使用ref处理对象  ==> 对象会被自动reactive为proxy对象
      const m3 = ref({a1: 2, a2: {a3: 'abc'}})
      console.log(m1, m2, m3)
      console.log(m3.value.a2) // 也是一个proxy对象

      function update() {
        m1.value += '--'
        m2.x += 1
        m2.y.z += '++'

        m3.value = {a1: 3, a2: {a3: 'abc---'}}
        m3.value.a2.a3 += '==' // reactive对对象进行了深度数据劫持
        console.log(m3.value.a2)
      }

      return {
        m1,
        m2,
        m3,
        update
      }
    }
  }
  </script>
  ```

## 6.setup 的两个注意点

- setup 执行的时机

  - 在 beforeCreate 之前执行一次，this 是 undefined。

- setup 的参数

  - props：值为对象，包含：组件外部传递过来，且组件内部声明接收了的属性。
  - context：上下文对象
    - attrs: 值为对象，包含：组件外部传递过来，但没有在 props 配置中声明的属性, 相当于 `this.$attrs`。
    - slots: 收到的插槽内容, 相当于 `this.$slots`。
    - emit: 分发自定义事件的函数, 相当于 `this.$emit`。

  ```js
  <template>
    <h2>App</h2>
    <p>msg: {{msg}}</p>
    <button @click="fn('--')">更新</button>

    <child :msg="msg" msg2="cba" @fn="fn"/>
  </template>

  <script lang="ts">
  import {
    reactive,
    ref,
  } from 'vue'
  import child from './child.vue'

  export default {

    components: {
      child  },

    setup () {
      const msg = ref('abc')

      function fn (content: string) {
        msg.value += content
      }
      return {
        msg,
        fn
       }
    }
  }
  </script>
  ```

  ```js
  <template>
    <div>
      <h3>{{n}}</h3>
      <h3>{{m}}</h3>

      <h3>msg: {{msg}}</h3>
      <h3>msg2: {{$attrs.msg2}}</h3>

      <slot name="xxx"></slot>

      <button @click="update">更新</button>
    </div>
  </template>

  <script lang="ts">

  import {
    ref,
    defineComponent
  } from 'vue'

  export default defineComponent({
    name: 'child',

    props: ['msg'],

    emits: ['fn'], // 可选的, 声明了更利于程序员阅读, 且可以对分发的事件数据进行校验

    data () {
      console.log('data', this)
      return {
        // n: 1
      }
    },

    beforeCreate () {
      console.log('beforeCreate', this)
    },

    methods: {
      // update () {
      //   this.n++
      //   this.m++
      // }
    },

    // setup (props, context) {
    setup (props, {attrs, emit, slots}) {

      console.log('setup', this)
      console.log(props.msg, attrs.msg2, slots, emit)

      const m = ref(2)
      const n = ref(3)

      function update () {
        // console.log('--', this)
        // this.n += 2
        // this.m += 2

        m.value += 2
        n.value += 2

        // 分发自定义事件
        emit('fn', '++')
      }

      return {
        m,
        n,
        update,
      }
    },
  })
  </script>
  ```

## 7.计算属性与监视

### 1.computed 函数

- 与 Vue2.x 中 computed 配置功能一致

- 写法

  ```js
  import {computed} from 'vue'

  setup(){
      ...
      //计算属性——简写
      let fullName = computed(()=>{
          return person.firstName + '-' + person.lastName
      })
      //计算属性——完整
      let fullName = computed({
          get(){
              return person.firstName + '-' + person.lastName
          },
          set(value){
              const nameArr = value.split('-')
              person.firstName = nameArr[0]
              person.lastName = nameArr[1]
          }
      })
  }
  ```

### 2.watch 函数

- 与 Vue2.x 中 watch 配置功能一致

- 两个小“坑”：

  - 监视 reactive 定义的响应式数据时：oldValue 无法正确获取、强制开启了深度监视（deep 配置失效）。
  - 监视 reactive 定义的响应式数据中某个属性时：deep 配置有效。

  ```js
  //情况一：监视ref定义的响应式数据
  watch(
    sum,
    (newValue, oldValue) => {
      console.log('sum变化了', newValue, oldValue)
    },
    { immediate: true }
  )

  //情况二：监视多个ref定义的响应式数据
  watch([sum, msg], (newValue, oldValue) => {
    console.log('sum或msg变化了', newValue, oldValue)
  })

  /* 情况三：监视定reactive义的响应式数据
              若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue！！
              若watch监视的是reactive定义的响应式数据，则强制开启了深度监视 
  */
  watch(
    person,
    (newValue, oldValue) => {
      console.log('person变化了', newValue, oldValue)
    },
    { immediate: true, deep: false }
  ) //此处的deep配置不再奏效

  //情况四：监视reactive定义的响应式数据中的某个属性
  watch(
    () => person.name,
    (newValue, oldValue) => {
      console.log('person的name变化了', newValue, oldValue)
    }
  )

  //情况五：监视reactive定义的响应式数据中的某些属性
  watch([() => person.age, () => person.name], (newValue, oldValue) => {
    console.log('person的age/name变化了', newValue, oldValue)
  })

  //特殊情况 监视的reactive定义的某个属性依然是一个对象，deep有效
  watch(
    () => person.job,
    (newValue, oldValue) => {
      console.log('person的job变化了', newValue, oldValue)
    },
    { deep: true }
  ) //此处由于监视的是reactive素定义的对象中的某个属性，所以deep配置有效
  ```

### 3.watchEffect 函数

- watch 的套路是：既要指明监视的属性，也要指明监视的回调。

- watchEffect 的套路是：不用指明监视哪个属性，监视的回调中用到哪个属性，那就监视哪个属性。

- watchEffect 有点像 computed：

  - 但 computed 注重的计算出来的值（回调函数的返回值），所以必须要写返回值。
  - 而 watchEffect 更注重的是过程（回调函数的函数体），所以不用写返回值。

  ```js
  //watchEffect所指定的回调中用到的数据只要发生变化，则直接重新执行回调。
  watchEffect(() => {
    const x1 = sum.value
    const x2 = person.job.address
    console.log('watchEffect配置的回调执行了')
  })
  ```

## 8.生命周期

<div style="border:1px solid black;width:380px;float:left;margin-right:20px;"><strong>vue2.x的生命周期</strong><img src="https://tangjiusheng.com/d/file/image/20220607/1654610489606675.png" alt="lifecycle_2" style="zoom:33%;width:1200px" /></div><div style="border:1px solid black;width:510px;height:985px;float:left"><strong>vue3.0的生命周期</strong><img src="https://tangjiusheng.com/d/file/image/20220607/1654610470462649.png" alt="lifecycle_2" style="zoom:33%;width:2500px" /></div>


- Vue3.0 中可以继续使用 Vue2.x 中的生命周期钩子，但有有两个被更名：

  `beforeDestroy`改名为` beforeUnmount`

  `destroyed`改名为 `unmounted`

- Vue3.0 也提供了 Composition API 形式的生命周期钩子，与 Vue2.x 中钩子对应关系如下：

  - `beforeCreate`===>`setup()`
  - `created`=======>`setup()`
  - `beforeMount` ===>`onBeforeMount`
  - `mounted`=======>`onMounted`
  - `beforeUpdate`===>`onBeforeUpdate`
  - `updated` =======>`onUpdated`
  - `beforeUnmount` ==>`onBeforeUnmount`
  - `unmounted` =====>`onUnmounted`

## 9.自定义 hook 函数

- 什么是 hook？—— 本质是一个函数，把 setup 函数中使用的 Composition API 进行了封装。

- 类似于 vue2.x 中的 mixin。

- 自定义 hook 的优势: 复用代码, 让 setup 中的逻辑更清楚易懂。

## 10.toRef

- 作用：创建一个 ref 对象，其 value 值指向另一个对象中的某个属性。

- 语法：`const name = toRef(person,'name')`

- 应用: 要将响应式对象中的某个属性单独提供给外部使用时。

- 扩展：`toRefs` 与`toRef`功能一致，但可以批量创建多个 ref 对象，语法：`toRefs(person)`

## 11.使用 ref 获取子组件的数据和方法

在 vue2.x 中，可以通过给元素添加 ref='xxx'属性，然后在代码中通过 this.$refs.xxx 获取到对应的元素

然而在 vue3 中时没有$refs 这个东西的，因此 vue3 中通过 ref 属性获取元素就不能按照 vue2 的方式来获取

vue3 需要借助生命周期方法，原因很简单，在 setup 执行时，template 中的元素还没挂载到页面上，所以必须在 onMounted 之后才能获取到元素。

```js
<template>
  <div>HelloWorld</div>
  <p>{{obj.name}}</p>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'HelloWorld',
  setup(){
    let obj = reactive({
      name:'xxx',
      age:12
    })
    let handleChangeName = () => {
      obj.name = 'jjj'
    }
    return{
      obj,
      handleChangeName
    }
  },
}
</script>
```

```js
<template>
  <HelloWorld ref="hello" />
  <button @click="test">尝试调取子组件中的事件</button>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import { ref, onMounted } from "vue";
export default {
  name: "App",
  components: {
    HelloWorld,
  },
  setup() {
    let hello = ref(null);
    onMounted(() => {
      console.log(hello.value, "hello");
    });
    let test = () => {
      hello.value.handleChangeName();
    };
    return {
      hello,
      test,
    };
  },
};
</script>
```

# 三、其它 Composition API

## 1.shallowReactive 与 shallowRef

- shallowReactive：只处理对象最外层属性的响应式（浅响应式）。

- shallowRef：只处理基本数据类型的响应式, 不进行对象的响应式处理。

- 什么时候使用?

  - 如果有一个对象数据，结构比较深, 但变化时只是外层属性变化 ===> shallowReactive。
  - 如果有一个对象数据，后续功能不会修改该对象中的属性，而是生新的对象来替换 ===> shallowRef。

## 2.readonly 与 shallowReadonly（数据不会变，只读）

- readonly: 让一个响应式数据变为只读的（深只读）。
- shallowReadonly：让一个响应式数据变为只读的（浅只读）。
- 应用场景: 不希望数据被修改时。

## 3.toRaw 与 markRaw（数据会变，但不是响应式的）

- toRaw：
  - 作用：将一个由`reactive`生成的<strong style="color:orange">响应式对象</strong>转为<strong style="color:orange">普通对象</strong>。
  - 使用场景：用于读取响应式对象对应的普通对象，对这个普通对象的所有操作，不会引起页面更新。
- markRaw：
  - 作用：标记一个对象，使其永远不会再成为响应式对象。
  - 应用场景:
    1. 有些值不应被设置为响应式的，例如复杂的第三方类库等。
    2. 当渲染具有不可变数据源的大列表时，跳过响应式转换可以提高性能。(通常是往 reactive 创建的响应式对象中添加一个对象，正常情况添加的这个对象会是响应式的数据，使用 markRaw 标记过后就不再是响应式的)

## 4.customRef

- 作用：创建一个自定义的 ref，并对其依赖项跟踪和更新触发进行显式控制。

- 实现防抖效果：

  ```vue
  <template>
    <input type="text" v-model="keyword" />
    <h3>{{ keyword }}</h3>
  </template>

  <script>
  import { ref, customRef } from 'vue'
  export default {
    name: 'Demo',
    setup() {
      // let keyword = ref('hello') //使用Vue准备好的内置ref
      //自定义一个myRef
      function myRef(value, delay) {
        let timer
        //通过customRef去实现自定义
        return customRef((track, trigger) => {
          return {
            get() {
              track() //告诉Vue这个value值是需要被“追踪”的
              return value
            },
            set(newValue) {
              clearTimeout(timer)
              timer = setTimeout(() => {
                value = newValue
                trigger() //告诉Vue去更新界面
              }, delay)
            },
          }
        })
      }
      let keyword = myRef('hello', 500) //使用程序员自定义的ref
      return {
        keyword,
      }
    },
  }
  </script>
  ```

## 5.provide 与 inject

- 作用：实现<strong style="color:#DD5145">祖与后代组件间</strong>通信

- 套路：父组件有一个 `provide` 选项来提供数据，后代组件有一个 `inject` 选项来开始使用这些数据

- 具体写法：

  1. 祖组件中：

     ```js
     setup(){
         ......
         let car = reactive({name:'奔驰',price:'40万'})
         provide('car',car)
         ......
     }
     ```

  2. 后代组件中：

     ```js
     setup(props,context){
         ......
         const car = inject('car')
         return {car}
         ......
     }
     ```

## 6.响应式数据的判断

- isRef: 检查一个值是否为一个 ref 对象
- isReactive: 检查一个对象是否是由 `reactive` 创建的响应式代理
- isReadonly: 检查一个对象是否是由 `readonly` 创建的只读代理
- isProxy: 检查一个对象是否是由 `reactive` 或者 `readonly` 方法创建的代理

# 四、Composition API 的优势

## 1.Options API 存在的问题

使用传统 OptionsAPI 中，新增或者修改一个需求，就需要分别在 data，methods，computed 里修改 。

<div style="width:600px;height:370px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f84e4e2c02424d9a99862ade0a2e4114~tplv-k3u1fbpfcp-watermark.image" style="width:600px;float:left" />
</div>
<div style="width:300px;height:370px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e5ac7e20d1784887a826f6360768a368~tplv-k3u1fbpfcp-watermark.image" style="zoom:50%;width:560px;left" /> 
</div>

## 2.Composition API 的优势

我们可以更加优雅的组织我们的代码，函数。让相关功能的代码更加有序的组织在一起。

<div style="width:500px;height:340px;overflow:hidden;float:left">
    <img src="https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bc0be8211fc54b6c941c036791ba4efe~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>
<div style="width:430px;height:340px;overflow:hidden;float:left">
    <img src="https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6cc55165c0e34069a75fe36f8712eb80~tplv-k3u1fbpfcp-watermark.image"style="height:360px"/>
</div>

# 五、新的组件

## 1.Fragment

- 在 Vue2 中: 组件必须有一个根标签
- 在 Vue3 中: 组件可以没有根标签, 内部会将多个标签包含在一个 Fragment 虚拟元素中
- 好处: 减少标签层级, 减小内存占用

## 2.Teleport

- 什么是 Teleport？—— `Teleport` 是一种能够将我们的<strong style="color:#DD5145">组件 html 结构</strong>移动到指定位置的技术。

  ```vue
  <teleport to="移动位置">
      <div v-if="isShow" class="mask">
          <div class="dialog">
              <h3>我是一个弹窗</h3>
              <button @click="isShow = false">关闭弹窗</button>
          </div>
      </div>
  </teleport>
  ```

## 3.Suspense(实验阶段)

- 等待异步组件时渲染一些额外内容，让应用有更好的用户体验

- 使用步骤：

  - 异步引入组件

    ```js
    import { defineAsyncComponent } from 'vue'
    const Child = defineAsyncComponent(() => import('./components/Child.vue'))
    ```

  - 使用`Suspense`包裹组件，并配置好`default` 与 `fallback`

    ```vue
    <template>
      <div class="app">
        <h3>我是App组件</h3>
        <Suspense>
          <template v-slot:default>
            <Child />
          </template>
          <template v-slot:fallback>
            <h3>加载中.....</h3>
          </template>
        </Suspense>
      </div>
    </template>
    ```

# 六、其他

## 1.全局 API 的转移

- Vue 2.x 有许多全局 API 和配置。

  - 例如：注册全局组件、注册全局指令等。

    ```js
    //注册全局组件
    Vue.component('MyButton', {
      data: () => ({
        count: 0
      }),
      template: '<button @click="count++">Clicked {{ count }} times.</button>'
    })

    //注册全局指令
    Vue.directive('focus', {
      inserted: el => el.focus()
    }
    ```

- Vue3.0 中对这些 API 做出了调整：

  - 将全局的 API，即：`Vue.xxx`调整到应用实例（`app`）上

    | 2.x 全局 API（`Vue`）    | 3.x 实例 API (`app`)                        |
    | ------------------------ | ------------------------------------------- |
    | Vue.config.xxxx          | app.config.xxxx                             |
    | Vue.config.productionTip | <strong style="color:#DD5145">移除</strong> |
    | Vue.component            | app.component                               |
    | Vue.directive            | app.directive                               |
    | Vue.mixin                | app.mixin                                   |
    | Vue.use                  | app.use                                     |
    | Vue.prototype            | app.config.globalProperties                 |

## 2.其他改变

- data 选项应始终被声明为一个函数。

- 过度类名的更改：

  - Vue2.x 写法

    ```css
    .v-enter,
    .v-leave-to {
      opacity: 0;
    }
    .v-leave,
    .v-enter-to {
      opacity: 1;
    }
    ```

  - Vue3.x 写法

    ```css
    .v-enter-from,
    .v-leave-to {
      opacity: 0;
    }

    .v-leave-from,
    .v-enter-to {
      opacity: 1;
    }
    ```

- <strong style="color:#DD5145">移除</strong>keyCode 作为 v-on 的修饰符，同时也不再支持`config.keyCodes`

- <strong style="color:#DD5145">移除</strong>`v-on.native`修饰符

  - 父组件中绑定事件

    ```vue
    <my-component
      v-on:close="handleComponentEvent"
      v-on:click="handleNativeClickEvent"
    />
    ```

  - 子组件中声明自定义事件

    ```vue
    <script>
    export default {
      emits: ['close'],
    }
    </script>
    ```

- <strong style="color:#DD5145">移除</strong>过滤器（filter）

  > 过滤器虽然这看起来很方便，但它需要一个自定义语法，打破大括号内表达式是 “只是 JavaScript” 的假设，这不仅有学习成本，而且有实现成本！建议用方法调用或计算属性去替换过滤器。

- ......
