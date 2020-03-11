# start - refs

Ref 转发是一个可选特性，其允许某些组件接收 ref，并将其向下传递（换句话说，“转发”它）给子组件。

```
import React, { Component } from 'react'
import ReactDOM from 'react-dom';

// ref 会转发到这个组件中
const Refs = React.forwardRef((props, ref) => {
    return (
        <div>
            <div className="inner" ref={ref}>This is inside Refs component.</div>
        </div>
    )
})

// 通过 React.createRef() 创建 ref，传递到子组件中
// 此时，父组件中保存了子组件中的 DOM 引用
export default class Index extends Component {
    constructor (props) {
        super(props);
        this.state = {
            ref: React.createRef()
        }
    }
    handleClick = () => {
        console.log(this.state.ref.current)
    }
    render() {
        return (
            <div>
                <Refs ref={this.state.ref}/>
                <div onClick={this.handleClick}>Click</div>
            </div>
        )
    }
}

ReactDOM.render(<Index />, document.querySelector('#root'))
```

## 展示 `displayName`

如果 `React.forwardRef` 的参数是一个匿名函数，则在开发者工具中该 ref 的displayName展示为 `ForwardRef`；如果是具名函数或者手动设置了 displayName，则开发者工具中展示为：`ForwardRef(myFunction)`：

```
const WrappedComponent = React.forwardRef(
  function myFunction(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }
);
```

```
function logProps(Component) {
  class LogProps extends React.Component {
    // ...
  }

  function forwardRef(props, ref) {
    return <LogProps {...props} forwardedRef={ref} />;
  }

  // 在 DevTools 中为该组件提供一个更有用的显示名。
  // 例如 “ForwardRef(logProps(MyComponent))”
  const name = Component.displayName || Component.name;
  forwardRef.displayName = `logProps(${name})`;

  return React.forwardRef(forwardRef);
}
```