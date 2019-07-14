---
title: react Hooks 介绍
date: 2019-07-14 18:26:51
tags:  react  hooks
---

## Hooks

## 为什么要有Hooks
### 1. 想要复用一个有状态的组件太麻烦
我们都知道react都核心思想就是，将一个页面拆成一堆独立的，可复用的组件，并且用自上而下的单向数据流的形式将这些组件串联起来。但假如你在大型的工作项目中用react，你会发现你的项目中实际上很多react组件冗长且难以复用。尤其是那些写成class的组件，它们本身包含了状态（state），所以复用这类组件就变得很麻烦。
那之前，官方推荐怎么解决这个问题呢？答案是：渲染属性（Render Props）和高阶组件（Higher-Order Components）。
渲染属性(Render Props)指的是使用一个值为函数的prop来传递需要动态渲染的nodes或组件。如下面的代码可以看到我们的 Mouse组件包含了所有跟状态相关的代码，而 Cat组件则可以是一个单纯的展示型组件，这样一来 Mouse就可以单独复用了。
```
class Cat extends React.Component {
  render() {
    const mouse = this.props.mouse;
    return (
      <img src="/cat.jpg" style={{ position: 'absolute', left: mouse.x, top: mouse.y }} />
    );
  }
}

class Mouse extends React.Component {
  constructor(props) {
    super(props);
    this.handleMouseMove = this.handleMouseMove.bind(this);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove(event) {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100%' }} onMouseMove={this.handleMouseMove}>

        {/*
          Instead of providing a static representation of what <Mouse> renders,
          use the `render` prop to dynamically determine what to render.
        */}
        {this.props.render(this.state)}
      </div>
    );
  }
}

<Mouse render={mouse => (
    <Cat mouse={mouse} />
)}/>
```
高阶组件这个概念就更好理解了，说白了就是一个函数接受一个组件作为参数，经过一系列加工后，最后返回一个新的组件。看下面的代码示例， withUser函数就是一个高阶组件，它返回了一个新的组件，这个组件具有了它提供的获取用户信息的功能。
```
const withUser = WrapperedComponent => {
      const user = sessionStorage.getItem("user");
      return props => <WrappedComponent user={user} {...props} />;
};

const UserPage = props => {
    <div class="user-container">
        <p>My name is {props.user}!</p>
    </div>
};
export default withUser(UserPage);

```
以上这两种模式看上去都挺不错的，很多库也运用了这种模式，比如我们常用的React Router, React Redux。但我们仔细看这两种模式，会发现它们会增加我们代码的层级关系。最直观的体现，打开devtool看看你的组件层级嵌套是不是很夸张吧。

### 2.生命周期钩子函数里的逻辑太乱

我们通常希望一个函数只做一件事情，但我们的生命周期钩子函数里通常同时做了很多事情。比如我们需要在 componentDidMount中发起ajax请求获取数据，绑定一些事件监听等等。同时，有时候我们还需要在 componentDidUpdate做一遍同样的事情。当项目变复杂后，这一块的代码也变得不那么直观。

### 3.classes真的太让人困惑

我们用class来创建react组件时，还有一件很麻烦的事情，就是this的指向问题。为了保证this的指向正确，我们要经常写这样的代码： this.handleClick=this.handleClick.bind(this)，或者是这样的代码： <buttononClick={()=>this.handleClick(e)}>。一旦我们不小心忘了绑定this，各种bug就随之而来，很麻烦。

还有一件很苦恼的事情,尽可能把你的组件写成无状态组件的形式，因为它们更方便复用，可独立测试。然而很多时候，我们用function写了一个简洁完美的无状态组件，后来因为需求变动这个组件必须得有自己的state，我们又得很麻烦的把function改成class。

在这样的背景下，Hooks便出来了


## Hooks的基本用法

### 1. State Hook
useState是react自带的一个hook函数，它的作用就是用来声明状态变量。 useState这个函数接收的参数是我们的状态初始值（initial state），它返回了一个数组，这个数组的第 [0]项是当前当前的状态值，第 [1]项是可以改变状态值的方法函数。
```
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
我们对比着看一下，如果没有hooks，我们会怎么写？
```
import React from 'react';


class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
### 2. Effect Hook
我们写的有状态组件，通常会产生很多的副作用（side effect），比如发起ajax请求获取数据，添加一些监听的注册和取消注册，手动修改dom等等。我们之前都把这些副作用的函数写在生命周期函数钩子里，比如componentDidMount，componentDidUpdate和componentWillUnmount。而现在的useEffect就相当与这些声明周期函数钩子的集合体。它以一抵三。
这里要注意几点：

第一，react首次渲染和之后的每次渲染都会调用一遍传给useEffect的函数。而之前我们要用两个声明周期函数来分别表示首次渲染（componentDidMount），和之后的更新导致的重新渲染（componentDidUpdate）。

第二，useEffect中定义的副作用函数的执行不会阻碍浏览器更新视图，也就是说这些函数是异步执行的，而之前的componentDidMount或componentDidUpdate中的代码则是同步执行的。这种安排对大多数副作用说都是合理的，但有的情况除外，比如我们有时候需要先根据DOM计算出某个元素的尺寸再重新渲染，这时候我们希望这次重新渲染是同步发生的，也就是说它会在浏览器真的去绘制这个页面前发生。

```
import React, { useState, useEffect } from 'react';

export default function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
  
    document.title = `You clicked ${count} times`;

    return function cleanup() {
      	     // todo
    };
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

我们对比着看一下，如果没有hooks，我们会怎么写？
```
export default class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
  }

  componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
还有哪些自带的Effect Hooks?

除了上文重点介绍的useState和useEffect，react还给我们提供来很多有用的hooks：

1. useContext
2. useReducer
3. useCallback
4. useMemo
5. useRef
6. useImperativeMethods
7. useMutationEffect
8. useLayoutEffect

我不再一一介绍，大家自行去查阅官方文档。


### 3. 自定义Hook

为什么要自己去写一个Effect Hooks? 这样我们才能把可以复用的逻辑抽离出来，变成一个个可以随意插拔的“插销”，哪个组件要用来，我就插进哪个组件里，so easy！看一个完整的例子，你就明白了。

```
import React, { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}







function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```
