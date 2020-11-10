---
title: vue如何给各个组件设置title
date: 2020-8-10 12:05:03
tags: 组件
categories: vue
summary: 你知道如何给vue组件设置标题吗
---
### vue如何给各个组件设置头部

#### 第一种

1. **首先安装依赖**

   ```
   npm install vue-wechat-title --save
   ```

2. **在Vue.use里面使用**

   ```js
   //在main.js文件中配置
   import vueWeChatTitle from 'vue-wechat-title'
   Vue.use(vueWeChatTitle)
   
   //引入模块 
   import Entrance from 'vue-wechat-title'
   
   //在router的index.js文件中配置
   {
       path: '/',
       //路由名称
       name: 'Entrance',
       //Entrance 引入模块名称
       component: Entrance,
       meta: {
           //声明title
           title: '首页入口'    
       }
   }
   ```

3. **调用**

   ```js
   //调用 在router-view视图中添加修改title的指令
   <router-view v-wechat-title="$route.meta.title" />
   ```

#### 第二种

1. **首先安装依赖**

   ```
   npm install vue-wechat-title --save
   ```

2. **配置 main.js**

   ```
   //设置title
   import vueWeChatTitle from 'vue-wechat-title'
   Vue.use(vueWeChatTitle)
   
   router.beforeEach(to, form, next) => {
       //路由发生变化时，修改页面title
       if （to.meta.title) {
           document.title = to.meta.title
   	}
       next()
   }
   
   ```

   

3. **在你需要改变的页面 第一个div 上加上v-wechat-title="this.title", title 是下面再 data 里面定义的数据.**

   ```
   <div v-wechat-title="this.title">
   </div>
   ```

4. 动态的改变title

   ```js
   export default {
       data(){
           return {
               title: '你好啊'
   		}
       },
       created(){
           //动态修改 使用方法写获取数据的方法 获取完成之后把文章的标题给title
           this.title= 'sss'
       }
   }
   
   ```

[借鉴文章 ]: https://blog.csdn.net/Tomwildboar/article/details/8276700

