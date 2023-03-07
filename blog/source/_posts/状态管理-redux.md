---
title: 状态管理-redux
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 111

---

## Redux

Redux是一个使用叫作"actions"的事件去管理和更新应用状态的模式和工具库。它以集中式Store的方式对整个应用中使用的状态进行集中管理，其规则确保状态只能以可预测的方式更新。

Redux提供的模式和工具使你更容易理解应用程序中的状态合适、何地、为什么以及如何更新，以及当这些更改发生时你的应用程序逻辑将如何表现。Redux知道你编写可预测和可测试的代码，这有助于让你确信你的应用程序将按预期工作

适用场景

- 在应用的大量地方，都存在大量的状态
- 应用状态会随着时间的推移而频繁更新
- 更新该状态的逻辑可能很复杂
- 中型和大型代码量的应用，很多人协同开发

<!--more-->
#### 核心概念

- 单一数据源
- State是只读的
- 使用Reducer纯函数进行更改

#### Redux Toolkit

Redux Toolkit是我们推荐的编写Redux逻辑的方法。它包含我们认为对于构建Redux应用程序必不可少的包和函数。Redux Toolkit构建在我们建议的最佳实践中，简化了大多数Redux任务，防止了常见错误，并使编写Redux应用程序变得更加容易

#### Redux DevTools 拓展

Redux DevTools 拓展可以显示Redux存储中状态随时间变化的历史记录。这允许你有效的调试应用程序，包括使用强大的技术，如“事件旅行调试”

#### Redux Store

所有Redux应用的中心都是store。"store"是保存应用程序的全局state的容器。

store是一个JavaScript对象，但是对于一般的对象它具有一些特殊的功能和能力：

- 不要直接修改或更改保存在Redux存储中的状态
- 更新状态的唯一方法是创建一个描述“应用程序中发生的某些事情”的普通action对象，然后将该action    dispatch到store以告诉它发生了什么
- 当一个action被dispatch后，store会调用reducer方法，让其根剧action和旧state计算出新state
- store会通知订阅者状态已更新，以便可以使用新数据更新UI界面

#### Actions

action是一个具有`type`字段的普通JavaScript对象。你可以将action看作应用程序中发生了什么的事情`type`字段是一个字符串，给这个action一个描述性的名字，比如`''todos/todoAdded''`。我们通常把那个类型的字符串写成“域/事件名称”，其中第一部分是这个action所属的特征或类别，第二部分是发生的具体事情。action对象可以有其他字段，其中包含有关发生的事情的附加信息。按照惯例，我们将该信息放在名为`payload`的字段中。

```javascript
const addTodoAction = {
    type:'todos/todoAdded',
    payload:'Buy milk'
}
```

#### Reducers

reducer是一个函数，接收当前的`state`和一个`action`对象，必要时决定如何更新状态，并返回新状态。函数签名是：`(state,action) ⇒ newState`。你可以将reducer视为一个时间监听器，他根剧接收到的action类型处理事件。

Reducer必须符合一下规则：

- 仅使用`state`和`action`参数计算新 的状态值
- 禁止直接修改`state`。必须通过复制现有的`state`并对复制的值进行更改的方式来做 不可变更新
- 禁止任何异步逻辑、依赖随机值或者“副作用”的代码

Reducer函数内部的逻辑通常包括一下步骤：

检查Reducer是否关心这个action，是的话就复制state，使用新值更新state副本，然后返回新state，不是的返回原来的state不变

```javascript
const initialState = { value: 0 }

function counterReducer(state = initialState, action) {
  // 检查 reducer 是否关心这个 action
  if (action.type === 'counter/increment') {
    // 如果是，复制 `state`
    return {
      ...state,
      // 使用新值更新 state 副本
      value: state.value + 1
    }
  }
  // 返回原来的 state 不变
  return state
}
```

#### Store

当前Redux应用的状态存在于一个名为store的对象中。Store是通过传入一个reducer来创建的，并且有一个名为`getState`的方法，它返回当前状态值

```javascript
import { configureStore } from '@reduxjs/toolkit'

const store = configureStore({ reducer: counterReducer })

console.log(store.getState())
// {value: 0}
```

#### Dispatch

Redux store有一种方法叫`dispatch`。更新state的唯一方法是调用`store.dispatch`并传入一个action对象。store将执行所有reducer函数并计算更新后的state，调用`getState()`可以获取新state

```javascript
store.dispatch({ type: 'counter/incremented' })

console.log(store.getState())
// {value: 1}
```

#### Selectors

Selector函数可以从store状态书中提取指定的片段。随着应用变得越来越大，会遇到应用程序的不同部分需要读取相同的数据，selector可以避免重复这样的读取逻辑

```javascript
const selectCounterValue = state => state.value

const currentValue = selectCounterValue(store.getState())
console.log(currentValue)
// 2
```
