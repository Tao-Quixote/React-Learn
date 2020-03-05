# Next.js

## Features

* An intuitive page-based routing system (with support for dynamic routes)
* SSR
* Automatic code splitting
* Client-side routing with optimized page prefetching
* Built-in CSS support, and support for any CSS-in-JS library
* webpack-based

## Base

```shell
mkdir nextjs
cd nextjs
npm init
npm i react react-dom next
mkdir pages
```

```json
// package.json
"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
```

```shell
npm run dev
```

```react
// pages/index.js

export default function Index () {
	return (
		<div>Next.js</div>
	)
}
```

## Link

页面跳转不能使用 `<a />` 跳转，需要使用 `<Link />` 组件跳转。否则客户端会刷新页面，向服务端重新请求。

```nextjs
import Link from 'next/link';

export default function Index() {
  return (
    <div>
      <Link href="/about?title=xxx">
        <a title="About page">About Page</a>
      </Link>
      <p>Hello Next.js</p>
    </div>
  );
}
```

注意：Link 只是一个 wrapper 组件，本身不能添加 `href` 之外的属性，所以如果要添加，请添加到 `props.children` 标签上，否则会不起作用。

### `useRouter` - 用于获取链接中的参数

```
import { useRouter } from 'next/router';
import Layout from '../components/MyLayout';

const Page = () => {
  const router = useRouter();

  return (
    <Layout>
      <h1>{router.query.title}</h1>
      <p>This is the blog post content.</p>
    </Layout>
  );
};

export default Page;
```

## shared components

共享组件与 React 中一样使用即可，其他骚操作可以参考 [Using Shared Components](https://nextjs.org/learn/basics/using-shared-components/the-layout-component)。

## Dynamic Pages

[Create Dynamic Pages](https://nextjs.org/learn/basics/create-dynamic-pages/adding-a-list-of-posts)

## Dynamic routing

```react
// /pages/index.js
import Link from 'next/link'
const PostLink = props => (
  <li>
    <Link href="/p/[id]" as={`/p/${props.id}`}>
      <a>{props.id}</a>
    </Link>
  </li>
);

export default function Blog() {
  return (
    <div>
      <h1>My Blog</h1>
      <ul>
        <PostLink id="hello-nextjs" />
        <PostLink id="learn-nextjs" />
        <PostLink id="deploy-nextjs" />
      </ul>
    </div>
  );
}
```

注：`<Link />` 组件中的 as 属性用来指定地址栏中显示的地址。

```react
// /pages/p/[id].js
import { useRouter } from 'next/router'

export default function () {
	const router = useRouter()
	
	return <div>{router.query.id}</div>
}
```

> Having brackets ([]) in the page name makes it a dynamic route. Currently, you can not make part of a page name dynamic, only the full name. For example, /pages/p/[id].js is supported but /pages/p/post-[id].js is not currently.

> When creating the dynamic route we added id between the brackets ([]). This is the name of the query param received by the page, so for /p/hello-nextjs the query object will have { id: 'hello-nextjs'}, and we can access it with useRouter().

## Fetching Data

`getInitialProps ` 用来给默认导出的组件提供 props。在其他组件上调用无效。

```
import Layout from '../components/MyLayout';
import Link from 'next/link';
import fetch from 'isomorphic-unfetch';

const Index = props => (
  <Layout>
    <h1>Batman TV Shows</h1>
    <ul>
      {props.shows.map(show => (
        <li key={show.id}>
          <Link href="/p/[id]" as={`/p/${show.id}`}>
            <a>{show.name}</a>
          </Link>
        </li>
      ))}
    </ul>
  </Layout>
);

Index.getInitialProps = async function() {
  const res = await fetch('https://api.tvmaze.com/search/shows?q=batman');
  const data = await res.json();

  console.log(`Show data fetched. Count: ${data.length}`);

  return {
    shows: data.map(entry => entry.show)
  };
};

export default Index;
```
 
如果是  `/pages/p/[id].js` 这种被跳转页面，可以通过 `getInitialProps` 参数中的 `context` 来获取查询参数 `context.query` 是个对象，包含查询参数。

## 样式设置

```
export default function () {
	render () {
		return (
			<div>
				<h1>This is the h1 element.</h1>
				<style jsx>{`
					h1 {color: red;}
				`}</style>
			</div>
		)
	}
}
```

* 只能设置当前组件的样式
* 不能设置子组件的样式，即样式为 scoped
* 只能放置在 ```<style jsx>{``}</style>```中，必须使用模版字符串
* 要复写子元素中的样式需要使用 `<style jsx global>{``}</style>`，例如复写 markdown 或者 swiper 这种插件的样式