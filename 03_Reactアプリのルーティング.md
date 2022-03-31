# React アプリのルーティング

## 目次

1. ルーティング設定
1. レイアウトのコンポーネント化

## ルーティング設定

ルーティングはユーザがアクセスした URL に対してどのページを表示するか決めるための設定です。

**例：サンプルアプリのルーティング**

| ルート      | ページ    |
| ----------- | --------- |
| index       | Home      |
| `about`     | About     |
| `dashboard` | Dashboard |

このプロジェクトでは React Router というライブラリを使用します。
（※現時点の最新版 React Router v6 を使用します）

https://reactrouter.com/

`create-react-app`で作成したプロジェクトに`react-router-dom`をインストールします。（参考：[02_Reactアプリの作り方.md](./02_Reactアプリの作り方.md)）

```sh
$ cd my-app
$ npm install react-router-dom
```

> 《補足》
> `npm install`を実行するとカレントディレクトリの`node_modules`フォルダにパッケージをインストールします。
> `package.json`の`dependencies`でバージョンが管理されます。

**例：src/index.js**

`index.js`を編集してルーティング機能を有効化します。

`react-router-dom`から`BrowserRouter`をインポートします。

```js
import { BrowserRouter } from 'react-router-dom'
```

アプリのルートエレメントとなる`<App />`を`<BrowserRouter>`で囲みます。

```js
<BrowserRouter>
  <App />
</BrowserRouter>
```

これで App コンポーネント内はルーティング機能が使えるようになりました。

最終的な`src/index.js`の内容は以下です。

```js
import React from 'react'
import * as ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import './index.css'
import App from './App'
import reportWebVitals from './reportWebVitals'

const container = document.getElementById('root')
const root = ReactDOM.createRoot(container)

root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
)

reportWebVitals()
```

**例：src/pages**

ページコンポーネントの置き場所として`src/pages`フォルダを新規作成します。

次にページの例として`Home.js`、`About.js`、`Dashboard.js`を作成します。

**例：src/pages/Home.js**

```js
export default function Home() {
  return <h2>Home</h2>
}
```

**例：src/pages/About.js**

```js
export default function About() {
  return <h2>About</h2>
}
```

**例：src/pages/Dashboard.js**

```js
export default function Dashboard() {
  return <h2>Dashboard</h2>
}
```

> 《補足》
> React は JavaScript の中に HTML のようなタグ構文を埋め込むことができます。これを JSX といいます。

**例：src/App.js**

`App.js`を編集してページコンポーネントを表示できるようにします。

`react-router-dom`から`Routes`と`Route`をインポートします。

```js
import { Routes, Route } from 'react-router-dom'
```

各ページコンポーネントをインポートします。

```js
import Home from './pages/Home'
import About from './pages/About'
import Dashboard from './pages/Dashboard'
```

URL パスとコンポーネントの紐づけは`<Route>`を追加することで行います。
`<Route>`は`<Routes>`で囲む必要があります。

```js
<Routes>
  <Route index element={<Home />} />
  <Route path="about" element={<About />} />
  <Route path="dashboard" element={<Dashboard />} />
<Routes>
```

最終的な`src/App.js`の内容は以下です。

```js
import { Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'
import Dashboard from './pages/Dashboard'

function App() {
  return (
   <Routes>
      <Route index element={<Home />} />
      <Route path="about" element={<About />} />
      <Route path="dashboard" element={<Dashboard />} />
   </Routes>
  )
}

export default App
```

また上記の変更により`src/App.css`と`src/App.test.js`は使わなくなるのでファイルを削除してかまいません。

アプリを実行してルーティングを試します。

```sh
$ npm start
```

- `/`にアクセスすると Home が表示されます。
- `/about`にアクセスすると About が表示されます。
- `/dashboard`にアクセスすると Dashboard が表示されます。

## レイアウトのコンポーネント化

`<Route>`の階層を使ってアプリ共通のレイアウト（ヘッダ、フッタ、ナビゲーションバーなど）を 1 つのページコンポーネントにまとめる実装が可能です。

例えば次のように親 Route として Layout コンポーネントを追加できます。

```js
<Routes>
  <Route path="/" element={<Layout />}>
    <Route index element={<Home />} />
    <Route path="about" element={<About />} />
    <Route path="dashboard" element={<Dashboard />} />
  </Route>
<Routes>
```

次にレイアウトのサンプルを示します。

**例：src/pages/Layout.js**

レイアウトは 子 Route を描画する位置に`<Outlet />`を追加する必要があります。

```js
import { Outlet } from 'react-router-dom'

export default function Layout() {
  return (
    <div>
      <h1>サンプルアプリ</h1>
      <Outlet />
    </div>
  )
}
```

**例：src/App.js**

レイアウトに対応した App.js の例です。

```js
import { Routes, Route } from 'react-router-dom'
import Home from './pages/Home'
import About from './pages/About'
import Dashboard from './pages/Dashboard'
import Layout from './pages/Layout'

function App() {
  return (
    <Routes>
      <Route path="/" element={<Layout />}>
        <Route index element={<Home />} />
        <Route path="about" element={<About />} />
        <Route path="dashboard" element={<Dashboard />} />
      </Route>
    </Routes>
  )
}

export default App
```

アプリを実行してレイアウトを確認します。

```sh
$ npm start
```

- どのページも共通して Layout が表示されます。
- Layout の中に各ページの内容が表示されます。

