vue面试题整理
=

1.vue双向绑定的原理

    通过 Object.defineProperty() 来劫持各个属性的 setter，getter，在数据变动时发布消息给订阅者，触发相应监听回调
    
2.vue如何去除url的#

vue-router 默认使用 hash 模式，所以在路由加载的时候，项目中的 url 会自带 #。如果不想使用 #， 可以使用 vue-router 的另一种模式 history

```
new Router({
  mode: 'history',
  routes: [ ]
})
```

需要注意的是，当我们启用 history 模式的时候，由于我们的项目是一个单页面应用，所以在路由跳转的时候，就会出现访问不到静态资源而出现 404 的情况，这时候就需要服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面

3.介绍虚拟dom树

什么是: 仅包含可能变化的节点和可能变化的属性的树结构

为什么: 内容少，便于快速遍历比较不同

如何发挥作用: 

 当data中模型变量改变时，会通知虚拟DOM树
 
 虚拟DOM树先缓存本次的修改在元素对象上
 
 将一批修改生成新的DOM子树和原虚拟DOM树做对比。
 
 一旦发现不同的元素或内容，就只更新有修改的元素
 
 虚拟DOM树中，封装了传统DOM API: createElement() appendChild()  .innerHTML，避免了程序员编写大量重复的代码。
 
 4.vue生命周期的理解
 
beforeCreate() 在实例创建之间执行，数据未加载状态

created() 在实例创建、数据加载后，能初始化数据，dom渲染之前执行

beforeMount() 虚拟dom已创建完成，在数据渲染前最后一次更改数据

mounted() 页面、数据渲染完成，真实dom挂载完成

beforeUpdate() 重新渲染之前触发

updated() 数据已经更改完成，dom 也重新 render 完成,更改数据会陷入死循环

beforeDestory() 和 destoryed() 前者是销毁前执行（实例仍然完全可用），后者则是销毁后执行

5.组件通信

（1）父向子传值(子组件通过 props 属性，绑定父组件数据，实现双方通信)

```js
//父组件通过标签上面定义传值
<template>
  <Main :data="data"></Main>
</template>
<script>
  //引入子组件
  import Main from './main'
  export default {
    name:"parent",
    data(){
      return{
        data:'向子组件传递数据'
      }
    }
  }
  compoment:{
    Main
  }
</script>
//子组件通过props接受数据
<template>
  <div>
    {{data}}
  </div>
</template>
<script>
  export default{
    name:'son',
    props:['data']
  }
</script>
```

(2)子组件向父组件传值
```js
//子组件通过事件触发传值
<template>
  <button @click="events">send</div>
</template>
<script>
export default {
  data() {
    return {
      data: '子组件向父组件传参'
    }
  },
  methods: {
    events(){
      this.$emit('child', this.data)//child为父组件的自定义事件
    }
  }
}
</script>
//父组件接受数据
<template>
    <Main @child="parent"></Main>
    <div>子组件传来的值 : {{message}}</div>
</template>

<script>
import Main from './main'
export default {
    // ...
    data: {
        message: ''
    },
    methods: {
      parent(data) {
        this.message = data;
      }
    },
    component: {
        Main
    }
}
</script>
```

(3)非父子组件、兄弟组件之间的数据传递

```js
/*新建一个Vue实例作为中央事件总嫌*/
let event = new Vue();

/*监听事件*/
event.$on('eventName', (val) => {
    //......do something
});

/*触发事件*/
event.$emit('eventName', 'this is a message.')
```

6.v-if和v-show的区别

使用了 v-if 的时候，如果值为 false ，那么页面将不会有这个 html 标签生成。

v-show 则是不管值为 true 还是 false ，html 元素都会存在，只是 CSS 中的 display 显示或隐藏

7.$route和$router的区别 

$router 为 VueRouter 实例，想要导航到不同 URL，则使用 $router.push 方法

$route 为当前 router 跳转对象里面可以获取 name 、 path 、 query 、 params 等

8.vue组件data为什么是函数？

因为js本身的特性带来的，如果 data 是一个对象，那么由于对象本身属于引用类型，当我们修改其中的一个属性时，会影响到所有Vue实例的数据。如果将 data 作为一个函数返回一个对象，那么每一个实例的 data 属性都是独立的，不会相互影响了

9.computed和methods有什么区别？

    computed: 计算属性是基于它们的依赖进行缓存的,只有在它的相关依赖发生改变时才会重新求值
    对于 method ，只要发生重新渲染，method 调用总会执行该函数
    
10.对keep-alive的理解

keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染
```
<keep-alive>
  <component>
    <!-- 该组件将被缓存！ -->
  </component>
</keep-alive>
```

11.vue等单页面的优缺点

    优点：
    良好的交互体验
    良好的前后端工作分离模式
    减轻服务器压力
    缺点：
    SEO难度较高
    前进、后退管理
    初次加载耗时多

12.active-class是那个组件的属性？

    router-link组件的属性(router中可以添加属性linkExactActiveClass:'自定义css类')
    
13.嵌套路由怎么定义

```js
const route = [
{path:'/',component: 'home',children:[
    {path:'/', component: 'index'},
    {path:'/detail', component: 'details'}
]},
{path:'/login', component: 'login'}
]
```

14.懒加载（按需加载路由）

    Vue首屏加载非常慢
    原因: 当打包应用时，将所有JavaScript代码打包在一个文件中，导致js代码非常庞大，严重影响页面加载速度。
    解决: 
    1.配置打包工具，将组件分别打包到不同的js代码块中
     build/webpack.base.conf.js
       output:{
         path: config.build.assetsRoot,
          filename:’[name].js’,
         //新增
    chunkFilename:”[name].js”,
    publicPath: process.env.NODE_ENV===”production”
      ?config.build.assetsPublicPath
      :config.dev.assetsPublicPath
    }
    2.当路由请求到该组件时，才动态加载组件的内容
      路由字典中，路由配置和以前完全一样
      但是在引入组件对象时: 
      import Index from ‘@/views/Index.vue’
      改为
      const Index=()=>import(‘@/views/Index.vue’)//仅定义函数暂未执行
      当用户在vue中请求当前组件对应的路由地址时，由vue-router自动调用加载函数，动态请求Index.vue组件对象
      
15.vuex是什么

    vue中的状态管理。在main.js中引入store注入
    state:模型变量，可以使用$store.state.访问
    getters:可以理解为state的计算属性。我们在组件中使用 $sotre.getters.fun()
    mutations:修改状态，并且是同步的。在组件中使用$store.commit('fun',params)。
    actions:异步操作。在组件中使用是$store.dispath('')。context，它是一个和store对象具有相同对象属性的参数。
    modules：store的子模块，为了开发大型项目，方便状态管理而使用的

16.
