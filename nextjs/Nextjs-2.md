# Nextjs-2

## API

```
// /pages/api/demo.js
export default function (req, res) {
	res.status(200).json({
		name: '',
		gender: ''
	})
}
```

访问路由：`localhost:3000/api/demo`

## Deploy

methods1:

```
// ZEIT NOW

```

methods2:

```
// package.json

"scripts": {
	"dev": "next",
	"build": "next build",
	"start": "next start -p %PORT"
}

// bash/terminal
npm run build & PORT=8080 npm run start
```