## Vue 3.0 笔记

b站直播间链接：https://www.bilibili.com/video/BV1Tg4y1z7FH?from=search&seid=15421437200956884924

Vue3.0-beta 源码：https://github.com/vuejs/vue-next#status-beta

vue-next-template-explorer： https://vue-next-template-explorer.netlify.app/

Vue3.0-bate API文档：https://composition-api.vuejs.org/

### Vue3.0设计目标

- 更小

- - 全局 API 和内置组件 / 功能支持 tree-shaking
  - 常驻的代码尺寸控制在 10kb gzipped 上下

- 更快

- - 基于 Proxy 的变动侦测，性能整体优于 getter / setter
  - Virtual DOM 重构
  - 编译器架构重构，更多的编译时优化

- 加强API设计一致性

- 加强TypeScript支持

- 提高自身可维护性

- - 代码采用 monorepo 结构，内部分层更清晰
  - TypeScript 使得外部贡献者更有信心做改动

- 开放更多底层功能

对于我们开发者来讲，最关心的还是它的语法，实际上这块变化非常大。虽然目前是beta版本，但我们依然可以尝鲜，在本地创建Vue项目，并做一做Demo；

## 一、创建项目



```js
// 先升级vue-cli到4.x版本
cnpm install -g @vue/cli
// 通过脚手架创建项目，一路回车
vue create vue3.0
```

注：这一步实际上用的依然是2.x的版本



## 二、升级2.6到3.0beta版本



```js
// 安装完vue/cli以后，可以使用vue add添加插件
// 目前3.0对应的是vue-next项目
vue add vue-next
```



安装完vue-next以后，我们就发现本地项目已经升级到了3.0. 打开main.js如图：

![](/Users/ykk/Desktop/WX20200428-143727@2x.png)

左侧目录结构没有太大变化，main的语法却大不一样了；

前面我们提到Vue3.0更小，因为它支持Tree-Shaking，可以把每一个用到的API都抽取出来，通过上图我们发现，可以只解构出一个createApp函数，相比2.0简化了很多。

## 三、LifeCycle介绍(Hooks)

Vue3.0中，生命周期方法已经发生了很大变化，接下来我们对比一下：

![](/Users/ykk/Desktop/WX20200428-143826@2x.png)



## 四、Composition API介绍

实际上，起初定义的是Vue-Function-API，后经过社区意见收集，更名为Vue-Composition-API.

接下来，我们介绍几个变化大的Composition API：

- reactive API
- ref API
- watch API变化
- computed API变化
- 生命周期钩子变化（参考上面）
- TypeScript和JSX支持（暂时忽略）



### reactive

作用：创建响应式对象，非包装对象，类似于在2.0的data中声明变量。

它本身一种Hooks能力，用过React Hook的，实际上就等同于useState();大家估计很好奇，为什么叫reactive? 让人莫名有一种你再抄袭React的感觉！

```js
// 打开 App.vue，删除多余代码
<template>
  <div id="app">
    <h1>{{title.name}}</h1>
  </div>
  <div>
    // 此处可并列多个div，不再要求一个根元素了
  </div>
</template>

<script>
import { reactive } from 'vue'
export default {
  name: 'App',
  setup(){
    const title = reactive({
      name:'欢迎学习Vue3.0'
    })
    return { title }
  }
}
</script>
<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

页面效果图：

![](/Users/ykk/Desktop/WX20200428-144100@2x.png)

### Ref

作用：创建一个包装式对象，含有一个响应式属性value

它和reactive的差别，就是前者没有包装属性value

```js

<template>
  <div id="app">
    <h1>{{title.name}}</h1>
    <div>{{user}}</div>
  </div>
</template>

<script>
import { reactive,ref } from 'vue'
export default {
  name: 'App',
  setup(){
    const title = reactive({
      name:'欢迎学习Vue3.0'
    })
    const user = ref('河畔一角');
    //如果需要修改值，可通过value
    user.value = '河畔老师'
    return { title ,user }
  }
}
</script>
<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

效果图如下：

![](/Users/ykk/Desktop/WX20200428-144214@2x.png)

### 事件处理

既然不用methods，那事件处理该怎么调用方法？

```js
<template>
  <div id="app">
    <h1>{{title.name}}</h1>
    <div>{{user}}</div>
    <button @click="updateUser">修改名称</button>
  </div>
</template>

<script>
import { reactive,ref } from 'vue'
export default {
  name: 'App',
  setup(){
    const title = reactive({
      name:'欢迎学习Vue3.0'
    })
    const user = ref('河畔一角');
    const updateUser = ()=>{
      //如果需要修改值，可通过value
      user.value = '河畔老师'
    }
    return { title , user, updateUser }
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

### onMounted/computed

作用：周期函数和计算函数

```js
<template>
  <div id="app">
    <h1>{{title.name}}</h1>
    <div>{{user}}</div>
    <button @click="updateUser">修改名称</button>
    <div>当前count：{{computedCount}}</div>
    <button @click="increment">修改count</button>
  </div>
</template>

<script>
import { reactive,ref,onMounted,computed } from 'vue'
export default {
  name: 'App',
  setup(){
    const title = reactive({
      name:'欢迎学习Vue3.0'
    })
    const user = ref('河畔一角');
    //如果需要修改值，可通过value
    
    const updateUser = ()=>{
      user.value = '河畔老师'
    }
    
    //生命周期方法
    onMounted(()=>{
      console.log('init mounted...')
    })
    // 初始化count值
    const count = ref(0);
    const increment = ()=>{
      count.value++
    }
    // 调用计算属性函数Hook
    const computedCount=computed(()=>count.value*10)
    return { title , user, updateUser,count,increment,computedCount }
  }
}
</script>

<style>
#app {
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

实际上用法远不止这些，包括父子传递、支持jsx语法等，我们这儿不做一一介绍了，大家可以自已通过第三方资料整理，了解更多Vue3.0的语法；

Composition API 是基于函数组合的，是在原来的基础上做了拓展，能更好的复用逻辑。

温馨提示：目前Vue3.0是会兼容大部分2.x的语法，实际上主要的变化，在上面的Vue3.0设计目标里面已经列出，我们主要的学习成本可能就在Composition API这块，新增了很多组合API，它本身不依赖this指针，方便我们对逻辑做抽取封装。

![](/Users/ykk/Desktop/WX20200428-144412@2x.png)



笔记参考： https://mp.weixin.qq.com/s/oiXR1JZpfsRZWA3b_iX25w