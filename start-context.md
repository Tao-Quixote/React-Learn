# start - context

context 刚开始的初衷是为了解决组件深层嵌套时最外层的属性值需要一层层传递才能到达最里面组件的问题，此时如果只有最里面的组件使用了该属性值，会给外层的组件造成很多冗余的 props。

通过 `React.createContext` 创造一个 context，配合 `MyContext.Provider` 的方式，可以将最外层的值传递到 `Provider` 组件中的任意一层，解决了上述问题。

## base

```
// context.js
import React from 'react';

const context = React.createContext({
  name: 'xx'
})

export default context;


// a.jsx
import React, { Component } from 'react'
import B from './b';

export default class A extends Component {
  render() {
    return (
      <div>
        <div onClick={this.props.click}>click</div>
        <B />
      </div>
    )
  }
}

// b.jsx
import React, { Component } from 'react'
import MyContext from './context'

export default class B extends Component {
  static contextType = MyContext
  render() {
    return (
      <div>
        {this.context.name}
      </div>
    )
  }
}

```

```
// index.jsx
import React, { Component } from 'react'
import ReactDOM from 'react-dom'
import MyContext from './context'
import A from './a'

export default class Index extends Component {
    constructor (props) {
        super(props)
        this.state = {
            ct: {
                name: 'rr'
            }
        }
    }
    handleCt = () => {
        this.setState({
            ct: {
                name: Math.random()
            }
        })
    }
    render() {
        return (
            <div>
                <MyContext.Provider value={this.state.ct}>
                    <div onClick={this.handleCt}>xxxx</div>
                    <A click={this.handleCt}/>
                </MyContext.Provider>
            </div>
        )
    }
}

ReactDOM.render(<Index />, document.querySelector('#root'))
```

## Hook `useContext`

Hook 的本意是为了在函数组件中钩入更多的 React API 以及生命周期，使原来的无状态组件升级为现在的函数组件，所以 `useContext` 是在函数组件中使用的，代替了 class 组件中的 `static contextType = MyContext`或者 `ComponentA.contextType = MyContext`这种写法。

```
const themes = {
  light: {
    foreground: "#000000",
    background: "#eeeeee"
  },
  dark: {
    foreground: "#ffffff",
    background: "#222222"
  }
};

const ThemeContext = React.createContext(themes.light);

function App() {
  return (
    <ThemeContext.Provider value={themes.dark}>
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext);

  return (
    <button style={{ background: theme.background, color: theme.foreground }}>
      I am styled by theme context!
    </button>
  );
}
```

使用上不再需要通过 `this.context` 的方式去获取距离最近的 context，`useContext` 返回值即为距离最近的 context，可以在 jsx 中直接使用：

```
import MyContext from './context.js';

function A () {
	const context = useContext(MyContext)
	return (<div>{context.xxx}</div>)
}
```