# start - props

## spread props

```jsx
class Demo extends React.Component {
	render () {
		return (
			<React.Fragment>
				<SubComponent {...this.props} />
			</React.Fragment>
		)
	}
}
```

## defaultProps

```jsx
class Demo extends React.Component {

	static defaultProps = {
		...
	}
	
	render () {
		return ...
	}
}

Demo.defaultProps = {}
```
