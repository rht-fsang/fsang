---
title: 状态管理-dva
tags: 学习
categories: 前端
typora-copy-images-to: upload
top: 111
---

#### 数据流向

数据的改变发生通常是通过用户交互行为或者浏览器行为触发的，当此类行为会改变数据的时候可以通过`dispatch`发起一个action，如果是同步行为会直接通过`Reducers`改变`State`，如果是异步行为会先触发`Effects`然后流向`Reducers`最终改变`State`，所以在dva中，数据流向非常清晰简明。

![](https://raw.githubusercontent.com/rht-fsang/md-image/master/img/dva.png)

### 核心概念

- State：一个对象，保存整个应用状态
- View：React组件构成的视图层
- Action：一个对象，描述事件
- connect方法：一个函数，绑定State到View
- dispatch方法：一个函数，发送Action到State

#### State

State表示Model的状态数据，通常表现为一个JavaScript对象，也可以是任何值。操作的时候都要当成不可变数据来对待，保证每次都是全新对象，没有引用关系，以保证State的独立性，便于测试和追踪变化

#### Action

Action是一个普通JavaScript对象，它是改变State的唯一途径。action必须带有`type`属性指明具体的行为，其他字段可以自定义，如果要发起一个action需要使用`dispatch`函数；需要注意的是`dispatch`是在组件connect Models后，通过props传入的。

```javascript
dispatch({
  type:'add'
})
```

#### dispatch函数

dispatching function是一个用于触发action的函数，action是改变State的唯一途径，但是它只描述了一个行为，而dispatch可以看作是触发这个行为的方式，而Reducer则是描述如何改变数据的。

在dva中，connect Model的组件通过props可以访问到dispatch，可以调用Model中的Reducer或者Effects。

```javascript
dispatch({
  type:'user/add',//如果在model外调用，需要添加namespace
  payload:{},//需要传递的信息
})
```

#### Reducer

Reducer函数接收两个参数：之前已经累积运算的结果和当前要被累积的值，返回的是一个新的累积结果。该函数把一个集合归并成一个单位。

Reducer的概念来自于函数式编程，很多语言中都有reduce API。

```javascript
[{x:1},{y:2},{z:3}].reduce(function(prev, next){
    return Object.assign(prev, next);
})
//return {x:1, y:2, z:3}
```

在dva中，reducers聚合累积的结果是当前model的state对象。通过actions中传入的值，与当前reducers中的值进行运算获得新的值。需要注意的是Reducer必须是纯函数，所以以同样的输入必然得到相同的输出。以便使用热重载和时间旅行等功能。

#### Effect

Effect被称为副作用，在我们的应用中，最常见的就是异步操作。之所以叫副作用是因为它使得我们的函数变得不纯，同样的输入不一定获得同样的输出。

dva为了控制副作用的操作，底层引入了`redux-sagas`做异步流程控制，由于采用了`generator`的相关概念，所以将异步转成同步写法，从而将effects转为纯函数。

```javascript
function *addAfter1Second(action, { put, call }) {
  yield call(delay, 1000);
  yield put({ type: 'add' });
}
```

#### connect方法

connect方法返回的也是一个React组件，通常称为容器组件，因为它是原生UI组件的容器，，即在外面包了一层State。connect方法传入的第一个参数是mapStateToProps函数，mapStateToProps函数会返回一个对象，用于建立State到Props的映射关系

```javascript
import { connect } from 'dva';

function mapStateToProps(state) {

  return { todos: state.todos };

}

connect(mapStateToProps)(App);
```
