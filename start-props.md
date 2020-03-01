# start - props

## spread props

```react
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

```react
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
