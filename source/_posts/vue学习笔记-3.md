---
title: vue学习笔记(3)
date: 2019-02-21 12:23:52
tags: [vue, vuex]
categories:
  - vue
---

![vuex结构](https://vuex.vuejs.org/vuex.png)

# 1. vuex 是什么

github 站点: https://github.com/vuejs/vuex
在线文档: https://vuex.vuejs.org/zh-cn/
简单来说: 对应用中组件的状态进行集中式的管理(读/写)

<!-- more -->

# 2. 状态自管理应用

state: 驱动应用的数据源
view: 以声明方式将 state 映射到视图
actions: 响应在 view 上的用户输入导致的状态变化(包含 n 个更新状态的方法)

![单向数据流](https://vuex.vuejs.org/flow.png)

# 3. 多组件共享状态的问题

多个视图依赖于同一状态
来自不同视图的行为需要变更同一状态
以前的解决办法

- 将数据以及操作数据的行为都定义在父组件
- 将数据以及操作数据的行为传递给需要的各个子组件(有可能需要多级传递)
  vuex 就是用来解决这个问题的

![vuex结构](https://vuex.vuejs.org/vuex.png)

# 4. vuex 的核心概念

## 1). state

vuex 管理的状态对象
它应该是唯一的

```javascript
const state = {
  xxx: initValue
};
```

## 2). mutations

包含多个直接更新 state 的方法(回调函数)的对象
谁来触发: action 中的 commit('mutation 名称')
只能包含同步的代码, 不能写异步代码

```javascript
const mutations = {
  yyy(state, data) {
    // 更新state的某个属性
  }
};
```

## 3). actions

包含多个事件回调函数的对象
通过执行: commit()来触发 mutation 的调用, 间接更新 state
谁来触发: 组件中: \$store.dispatch('action 名称') // 'zzz'
可以包含异步代码(定时器, ajax)

```javascript
const actions = {
  zzz({ commit, state }, data1) {
    commit("yyy", data2);
  }
};
```

## 4). getters

包含多个计算属性(get)的对象
谁来读取: 组件中: \$store.getters.xxx

```javascript
    const getters = {
      mmm (state) {
        return ...
      }
    }
```

## 5). modules

包含多个 module
一个 module 是一个 store 的配置对象
与一个组件(包含有共享数据)对应

## 6). 向外暴露 store 对象

```javascript
export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters
});
```

## 7). 组件中:

```javascript
import {mapGetters, mapActions} from 'vuex'
export default {
computed: mapGetters(['mmm'])
methods: mapActions(['zzz'])
}

{{mmm}} @click="zzz(data)"
```

## 8). 映射 store

```javascript
import store from "./store";
new Vue({
  store
});
```

## 9). store 对象

1.所有用 vuex 管理的组件中都多了一个属性\$store, 它就是一个 store 对象 2.属性:
state: 注册的 state 对象
getters: 注册的 getters 对象 3.方法:
dispatch(actionName, data): 分发 action

# 5. 将 vuex 引到项目中

## 1). 下载: npm install vuex --save

## 2). 使用 vuex

1.store.js

```javascript
import Vuex from "vuex";
export default new Vuex.Store({
  state,
  mutations,
  actions,
  getters,
  modules
});
```

2.main.js

```javascript
import store from "./store.js";
new Vue({
  store
});
```
