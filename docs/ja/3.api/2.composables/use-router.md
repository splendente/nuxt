---
title: "useRouter"
description: "useRouter composable はルーターインスタンスを返します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

```vue [pages/index.vue]
<script setup lang="ts">
const router = useRouter()
</script>
```

テンプレート内でルーターインスタンスのみが必要な場合は、`$router` を使用してください:

```vue [pages/index.vue]
<template>
  <button @click="$router.back()">Back</button>
</template>
```

`pages/` ディレクトリがある場合、`useRouter` は `vue-router` で提供されるものと同一の動作をします。

::read-more{icon="i-simple-icons-vuedotjs" to="https://router.vuejs.org/api/interfaces/Router.html#Properties-currentRoute" target="_blank"}
`Router` インターフェースについての `vue-router` ドキュメントを参照してください。
::

## 基本的な操作

- [`addRoute()`](https://router.vuejs.org/api/interfaces/Router.html#addRoute): ルーターインスタンスに新しいルートを追加します。既存のルートの子として新しいルートを追加するために `parentName` を提供できます。
- [`removeRoute()`](https://router.vuejs.org/api/interfaces/Router.html#removeRoute): 名前で既存のルートを削除します。
- [`getRoutes()`](https://router.vuejs.org/api/interfaces/Router.html#getRoutes): すべてのルートレコードの完全なリストを取得します。
- [`hasRoute()`](https://router.vuejs.org/api/interfaces/Router.html#hasRoute): 指定した名前のルートが存在するかどうかをチェックします。
- [`resolve()`](https://router.vuejs.org/api/interfaces/Router.html#resolve): ルートロケーションの正規化されたバージョンを返します。既存のベースを含む `href` プロパティも含まれます。

```ts [Example]
const router = useRouter()

router.addRoute({ name: 'home', path: '/home', component: Home })
router.removeRoute('home')
router.getRoutes()
router.hasRoute('home')
router.resolve({ name: 'home' })
```

::note
`router.addRoute()` はルートの詳細をルートの配列に追加し、[Nuxt プラグイン](/docs/guide/directory-structure/plugins)を構築する際に有用です。一方、`router.push()` は即座に新しいナビゲーションをトリガーし、ページ、Vue コンポーネント、composable で有用です。
::

## History API ベース

- [`back()`](https://router.vuejs.org/api/interfaces/Router.html#back): 可能であれば履歴を戻ります。`router.go(-1)` と同じです。
- [`forward()`](https://router.vuejs.org/api/interfaces/Router.html#forward): 可能であれば履歴を進みます。`router.go(1)` と同じです。
- [`go()`](https://router.vuejs.org/api/interfaces/Router.html#go): `router.back()` や `router.forward()` で強制される階層制限なしに、履歴を前後に移動します。
- [`push()`](https://router.vuejs.org/api/interfaces/Router.html#push): 履歴スタックにエントリをプッシュして新しい URL にプログラムでナビゲートします。**代わりに [`navigateTo`](/docs/api/utils/navigate-to) を使用することを推奨します。**
- [`replace()`](https://router.vuejs.org/api/interfaces/Router.html#replace): ルート履歴スタックの現在のエントリを置き換えて新しい URL にプログラムでナビゲートします。**代わりに [`navigateTo`](/docs/api/utils/navigate-to) を使用することを推奨します。**

```ts [Example]
const router = useRouter()

router.back()
router.forward()
router.go(3)
router.push({ path: "/home" })
router.replace({ hash: "#bio" })
```

::read-more{icon="i-simple-icons-mdnwebdocs" to="https://developer.mozilla.org/en-US/docs/Web/API/History" target="_blank"}
ブラウザの History API について詳しく参照してください。
::

## ナビゲーションガード

`useRouter` composable はナビゲーションガードとして機能する `afterEach`、`beforeEach`、`beforeResolve` ヘルパーメソッドを提供します。

しかし、Nuxt には**ルートミドルウェア**の概念があり、ナビゲーションガードの実装を簡素化し、より良い開発者エクスペリエンスを提供します。

:read-more{to="/docs/guide/directory-structure/middleware"}

## Promise とエラーハンドリング

- [`isReady()`](https://router.vuejs.org/api/interfaces/Router.html#isReady): ルーターが初期ナビゲーションを完了したときに解決される Promise を返します。
- [`onError`](https://router.vuejs.org/api/interfaces/Router.html#onError): ナビゲーション中にキャッチされないエラーが発生するたびに呼び出されるエラーハンドラーを追加します。

:read-more{icon="i-simple-icons-vuedotjs" to="https://router.vuejs.org/api/interfaces/Router.html#Methods" title="Vue Router Docs" target="_blank"}

## ユニバーサルルーターインスタンス

`pages/` フォルダーがない場合、[`useRouter`](/docs/api/composables/use-router) は同様のヘルパーメソッドを持つユニバーサルルーターインスタンスを返しますが、すべての機能がサポートされているわけではなく、`vue-router` とまったく同じように動作しない可能性があることに注意してください。
