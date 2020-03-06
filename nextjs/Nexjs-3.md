# Nexjs-3 export as static HTML app

## 1、设置配置文件

```
// next.config.js
const fetch = require('isomorphic-unfetch');

module.exports = {
  exportTrailingSlash: true,
  exportPathMap: async function () {
    const paths = {
      '/': { page: '/' },
      '/about': { page: '/about' }
    };
    const res = await fetch('https://api.tvmaze.com/search/shows?q=batman');
    const data = await res.json();
    const shows = data.map(entry => entry.show);

    shows.forEach(show => {
      paths[`/show/${show.id}`] = { page: '/show/[id]', query: { id: show.id } };
    });

    return paths;
  }
};
```

`exportPathMap` 对象中配置多少路径，静态页面项目就有多少个页面。所有动态页面，如果需要有内容的话，需要实现组件的 `getInitialProps` 方法，在执行 `npm run export`时会自动执行该方法获取、填充并生成该动态页面对应的静态页面。

## 2、package.json

```
"scripts": {
	"export": "next export"
}
```

```bash
npm run export
```

## 3、deploy

```
// method 1:
cd ./out
sudo npm install -g serve
serve -p 8000

// method 2:
sudo npm install -g now
cd ./out
now
```