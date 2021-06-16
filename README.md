# VueDemo

## vue 笔记

### vue简介

1. MVVM (Model-View-ViewModel) 
   - viewModel： 介于view与model之间 用于view与model之间的数据双向绑定，负责转换Mode中的数据对象来让数据变得更容易管理和使用，在vue中viewModel通过虚拟Dom技术实现。vue中在不改变的Dom元素实现了数据的双向绑定。
   - 作用：该层向上与视图层进行双向数据绑定、向下与model层通过接口请求进行数据交互
2. SOC 原则
3. sass less 生成css的工具（css预处理程序）拓展了css 使得css更加容易编写，并使之变成动态的。并且提供了强大函数功能加入到了css中
4. vue的优势，不需要频繁的操作DOM元素。加快了网页的渲染速度。
5. viewModel 完全解耦了View层和Model层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环。

### 组件

​	吸取了react中组件的概念，使得页面的变得组件化，耦合性更低，并且对于组件来说，是可以重复使用的。

### 钩子函数

​	生命周期中随时插入方法改变其执行顺序。极大的丰富了开发途中的可拓展性

<img src="https://cn.vuejs.org/images/lifecycle.png" alt="img" style="zoom: 30%;" />





![image-20210609173527497](C:\Users\59949\AppData\Roaming\Typora\typora-user-images\image-20210609173527497.png)

XMLHttpRequest 对象用于和服务器交换数据。当你的页面全部加载完毕后，客户端会通过 `XMLHttpRequest` 对象向服务器请求数据，服务器端接受数据并处理后，向客户端反馈数据。类似于AJAX 实现异步请求

### computed：计算属性

​	类似缓存（虚拟dom）

slot（插槽）在vue.js 中可以使用<slot>元素作为承载分发内容的出口，作者称为插槽，可以应用在组合组件的场景中。

：表示v-bind  @表示v-on

this.$emit：自定义事件分发

1. 父组件调用子组件的方法
   1. 使用 $refs
2. 自组件调用父组件的方法
   1. 使用 $emit 

### vue-cli 应用程序

1. vue init webpack myvue
2. vue-cli 脚手架类似于Maven 会自动生成固定的目录结构，使开发更加规范。

### Webpack与Vite  Node与Deno

​	webpack与node 往往会结合起来使用，随着互联网的不断进化，这俩种技术已经不能满足广大程序员的需求了，Vite与Deno应运而生，Deno的作者便是node.js的作者，作者在前几年提出由于node的环境过于庞大，依赖的类库越来越多，程序也变得越来越笨重。决定舍弃node.js，继而开发一种全新的环境。这点我们观察对应项目的node_models 目录便不辨自明。

### webpack

​	本质上，**webpack** 是一个用于现代 JavaScript 应用程序的 *静态模块打包工具*。当 webpack 处理应用程序时，它会在内部构建一个 [依赖图(dependency graph)](https://webpack.docschina.org/concepts/dependency-graph/)，此依赖图对应映射到项目所需的每个模块，并生成一个或多个 *bundle*。

```bash
webpack  # 打包项目
```



### vue Router

​	官方提供的路由管理器，实现页面的迁移处理

1. 安装

   ```bach
   npm install vue-router --save-dev		
   ```

2. 定义一个router文件夹，编写路由文件index.js

   ```typescript
   import Vue from "vue";
   import VueRouter from "vue-router";
   
   Vue.use(VueRouter);
   
   const routes = [
     {
       path: "/main/:id/list",
       name: "main",
       component: () => import("@/views/main/main.vue"),
       props: true
     },
     {
       path: "/profile",
       name: "profile",
       component: () => import("@/views/profile/profile.vue")
     }
     {
       path: "*",
       redirect: "/home"
     }
   ];
   ```

3. 在main.js 中导入路由

   ```typescript
   import Vue from "vue";
   import App from "./App.vue";
   import router from "./router";
   import store from "./store";
   import vuetify from "./plugins/vuetify"; //页面框架 也可使用element-ui等等
   
   Vue.config.productionTip = false;
   
   new Vue({
     router,
     store,
     vuetify,
     render: h => h(App)
   }).$mount("#app");
   ```

4. 设置封装container

   ```vue
   <template>
     <v-app>
       <Container />
     </v-app>
   </template>
   
   <script lang="ts">
   import Vue from "vue";
   import Container from "./views/Container.vue";//相当于baseView
   
   @Component({
     components: {
       Container
     }
   })
   </script>
   
   <style lang="scss">
   @import "./styles/app.scss";
   </style>
   
   ```

5. container.vue中设置相关view

   ```vue
   <router-link
       :to="/profile"
   >
   <router-view />
   
   ```

### vue +elementUi

1. 创建工程

   ```bash
   1. vue init webpack  hello-vue
   2. npm install vue-router --save-dev #--save 安装到项目目录下
   3. npm i element-ui -S 
   4. npm install
   5. npm install sass-loader node-sall --save-dev # 安装sass加载器
   6. npm run dev #启动
   ```

2. 路由嵌套

   ```js
   {
       path: "/user/:id/profile",
       name: "porfile",
       component: () => import("@/views/user/Profile.vue"),
       props: true,
       children: [
         {
           path: ":talkId",
           component: () => import("@/views/user/Comment.vue"),
           props: true
         }
       ]
     }
   {
       path: "/user/:id/profile1",
       name: "porfile1",
       redirect: '/main' //重定向
       component: () => import("@/views/user/Profile1.vue"),
       props: true,
     }	
   ```

   ### 路由模式

    	1. hash：路径带 # 符号
    	2. history：路径不带 # 符号

   ```js
   修改路由配置
   export default new Router({
   	mode: 'history',
   	routes: [
   	]
   });
   ```

   ### 路由钩子

   ```js
   const Foo = {
     template: `...`,
     beforeRouteEnter(to, from, next) {
       // 在渲染该组件的对应路由被 confirm 前调用
       // 不！能！获取组件实例 `this`
       // 因为当守卫执行前，组件实例还没被创建
        next(); //继续执行
     },
     beforeRouteUpdate(to, from, next) {
       // 在当前路由改变，但是该组件被复用时调用
       // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
       // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
       // 可以访问组件实例 `this`
     },
     beforeRouteLeave(to, from, next) {
       // 导航离开该组件的对应路由时调用
       // 可以访问组件实例 `this`
     }
   }
   ```

   参数

   - **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
   - **`from: Route`**: 当前导航正要离开的路由
   - **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
     - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
     - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
     - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
     - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

