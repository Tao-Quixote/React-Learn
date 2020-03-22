# 修改 CRA 的默认 webpack 配置

[create-react-app](https://create-react-app.dev/docs/getting-started/)(简称 CRA)，默认将 webpack、babel 等的配置集成到了其内部组件中，有时候需要修改/扩展 webpack 配置，官方虽然提供了 `"eject": "react-scripts eject"`，但是推荐使用 react-app-rewired 这个工具：

## 在项目中安装

```bash
> npm install react-app-rewired --save-dev
```
**For create-react-app 1.x or react-scripts-ts with Webpack 3:**

```bash
> npm install react-app-rewired@1.6.2 --save-dev
```

## 创建配置文件 `config-overrides.js`

```js
module.exports = function override(config, env) {
  //do stuff with the webpack config...
  return config;
}
```
```
+-- your-project
|   +-- config-overrides.js
|   +-- node_modules
|   +-- package.json
|   +-- public
|   +-- README.md
|   +-- src
```

## 修改 `package.json`

```json
  /* package.json */

  "scripts": {
-   "start": "react-scripts start",
+   "start": "react-app-rewired start",
-   "build": "react-scripts build",
+   "build": "react-app-rewired build",
-   "test": "react-scripts test --env=jsdom",
+   "test": "react-app-rewired test --env=jsdom",
    "eject": "react-scripts eject"
}
```

##  Start the Dev Server
```bash
> npm start
```

##  Build your app
```bash
> npm run build
```

## Refs

* [GitHub timarney/react-app-rewired](https://github.com/timarney/react-app-rewired#3-flip-the-existing-calls-to-react-scripts-in-npm-scripts)