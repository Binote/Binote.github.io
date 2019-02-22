---
title: vue学习笔记(1)
date: 2019-02-21 12:16:37
tags: [vue, vue-cli, vue-router]
categories:
  - vue
---

## 1. vue 脚手架

用来创建 vue 项目的工具包
创建项目:

```bash
        npm install -g vue-cli
        vue init webpack VueDemo
```

开发环境运行:

```bash
        cd VueDemo
        npm install
        npm run dev
```

生产环境打包发布

```bash
        npm run build
        npm install -g serve
        serve dist
        http://localhost:5000
```

<!-- more -->

## 2. eslint

用来做项目编码规范检查的工具
基本原理: 定义了很多规则, 检查项目的代码一旦发现违背了某个规则就输出相应的提示信息
有相应的配置, 可定制检查

## 3. 组件化编程

vue 文件包含 3 个部分

```html
<template>
  <div></div>
</template>
<script>
      export default {
  props: []/{}
        data(){},
  computed: {}
        methods: {},

  watch: {}
  filters: {}
  directives: {}
  components: {}
      }
</script>
<style></style>
```

组件化编码的基本流程
1). 拆分界面, 抽取组件
2). 编写静态组件
3). 编写动态组件
初始化数据, 动态显示初始化界面
实现与用户交互功能
组件通信的 5 种方式
props
vue 的自定义事件
pubsub 第三方库
slot
vuex(后面单独讲)
props:
父子组件间通信的基本方式
属性值的 2 大类型:
一般: 父组件-->子组件
函数: 子组件-->父组件
隔层组件间传递: 必须逐层传递(麻烦)
兄弟组件间: 必须借助父组件(麻烦)
vue 自定义事件
子组件与父组件的通信方式
用来取代 function props
不适合隔层组件和兄弟组件间的通信
pubsub 第三方库(消息订阅与发布)
适合于任何关系的组件间通信
slot
通信是带数据的标签
注意: 标签是在父组件中解析
vuex
多组件共享状态(数据的管理)
组件间的关系也没有限制
功能比 pubsub 强大, 更适用于 vue 项目

## 4. ajax

相关库:
vue-resource: vue 插件, 多用于 vue1.x
axios: 第三方库, 多用于 vue2.x
vue-resource 使用

```javascript
// 引入模块
import VueResource from "vue-resource";
// 使用插件
Vue.use(VueResource);

// 通过vue/组件对象发送ajax请求
this.$http.get("/someUrl").then(
  response => {
    // success callback
    console.log(response.data); //返回结果数据
  },
  response => {
    // error callback
    console.log(response.statusText); //错误信息
  }
);
axios使用;
// 引入模块
import axios from "axios";

// 发送ajax请求
axios
  .get(url)
  .then(response => {
    console.log(response.data); // 得到返回结果数据
  })
  .catch(error => {
    console.log(error.message);
  });
```

## 5. vue-router

vue 用来实现 SPA 的插件
使用 vue-router

### 1. 创建路由器: router/index.js

```javascript
new VueRouter({
  routes: [
    {
      // 一般路由
      path: "/about",
      component: about
    },
    {
      // 自动跳转路由
      path: "/",
      redirect: "/about"
    }
  ]
});
```

### 2. 注册路由器: main.js

```javascript
import router from "./router";
new Vue({
  router
});
```

### 3. 使用路由组件标签:

```html
<router-link to="/xxx">Go to XXX</router-link> <router-view></router-view>
```

编写路由的 3 步 1. 定义路由组件 2. 映射路由 3. 编写路由 2 个标签
嵌套路由

```javascript
children: [
  {
    path: "/home/news",
    component: news
  },
  {
    path: "message",
    component: message
  }
];
```

向路由组件传递数据
params:

```html
<router-link to="/home/news/abc/123"></router-link>
```

props:

```html
<router-view msg="abc"></router-view>
```

缓存路由组件

```html
<keep-alive>
  <router-view></router-view>
</keep-alive>
```

路由的编程式导航
this.$router.push(path): 相当于点击路由链接(可以返回到当前路由界面)
this.$router.replace(path): 用新路由替换当前路由(不可以返回到当前路由界面)
this.\$router.back(): 请求(返回)上一个记录路由
