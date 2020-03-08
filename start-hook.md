# start - Hook

简单示例：

```
import React, { useState } from 'react';

function Example() {
  // 声明一个新的叫做 “count” 的 state 变量
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

特点：

* 100% 可选 - 可以使用也可以不使用
* 向后兼容
* 不具有破坏性的修改

Hook 使你在无需修改组件结构的情况下复用状态逻辑。 这使得在组件间或社区内共享 Hook 变得更便捷。

解决了每个生命周期中包含多个不相关逻辑的问题。将不同的状态逻辑拆分为更小的单元来单独管理，而不是统一一起塞到一个生命周期中管理。同时也解决关联状态逻辑被拆分的问题：比如在 `componentWillMount` 中声明定时器而在 `componentWillUnmount` 中清除定时器，这种相关逻辑被拆分到不同生命周期的问题得到了解决。

解决了 `class` 中一些不好理解的问题，比如 `this` 指向的问题。也解决了一些 class 带来的负面影响，比如压缩问题；热重载无效问题；还有一些优化无效的问题等。

Hook 和现有代码可以同时工作，你可以渐进式地使用他们。

**Hook** 不能在 `class` 组件中使用。

## 常用 hook

所有 hook 可以在同一个组件中多次使用。React 假设当你多次调用 useState 的时候，你能保证每次渲染时它们的调用顺序是不变的。

### `useState`

```
import {useState} from 'react'

function Foo () {
	const [state, setState] = useState(false)
	
	render () {
		return (<div>{state}</div>)
	}
}
```

###  `useEffect`

它跟 class 组件中的 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 具有相同的用途，只不过被合并成了一个 API。

默认情况下，React 会在每次渲染后调用副作用函数 —— 包括第一次渲染的时候。副作用函数还可以通过返回一个函数来指定如何“清除”副作用。

```
import React, { useState, useEffect } from 'react';

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  function handleStatusChange(status) {
    setIsOnline(status.isOnline);
  }

  useEffect(() => {
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

    return () => {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

可以在组件中多次使用 useEffect。通过使用 Hook，你可以把组件内相关的副作用组织在一起（例如创建订阅及取消订阅），而不要把它们拆分到不同的生命周期函数里。

### 注意

* 只能在函数的最外层调用 Hook
* 只能在函数组件中调用 Hook
* 约定 Hook 使用 use 开头：`useSomething`

Hook 是一种复用状态逻辑的方式，它不复用 state 本身。事实上 Hook 的每次调用都有一个完全独立的 state —— 因此你可以在单个组件中多次调用同一个自定义 Hook。这里的意思就是如果一个 Hook 被多次调用的话，其内部通过 useState 声明的 state 是分别独立的，所以如果在多个组件中分别调用了自定义的 Hook 的话，则每个组件都拥有独立的 state，互不影响。因此，Hook 不仅是对状态逻辑的封装，也保持了状态的独立。

另外还有 `useContext` / `useReducer` 等 Hook。useContext 让你不使用组件嵌套就可以订阅 React 的 Context。useReducer 可以让你通过 reducer 来管理组件本地的复杂 state。

## useEffect

在第一次渲染之后和每次更新之后都会执行。如果要在组件卸载之前做事情的话需要在 Hook 的最后返回一个函数。

可以通过传递第二个可选参数的方式来定制 `useEffect` 是否再次调用：

```
useEffect(() => {
	...
}, [a, b])
```
上面的例子中，只有 `a`, `b` 两个 state 发生变化的时候才会重新调用 `useEffect` 钩子。如果只想该钩子只调用一次的话，可以通过传一个空数组给可选参数的方式实现。