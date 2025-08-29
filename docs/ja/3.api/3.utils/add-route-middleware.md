---
title: 'addRouteMiddleware'
description: 'addRouteMiddleware() はアプリケーションでミドルウェアを動的に追加するためのヘルパー関数です。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

::note
ルートミドルウェアは、Nuxt アプリケーションの [`middleware/`](/docs/guide/directory-structure/middleware) ディレクトリに保存されるナビゲーションガードです（[別の設定](/docs/api/nuxt-config#middleware)をしない限り）。
::

## 型

```ts
function addRouteMiddleware (name: string, middleware: RouteMiddleware, options?: AddRouteMiddlewareOptions): void
function addRouteMiddleware (middleware: RouteMiddleware): void

interface AddRouteMiddlewareOptions {
  global?: boolean
}
```

## パラメーター

### `name`

- **Type:** `string` | `RouteMiddleware`

文字列または `RouteMiddleware` 型の関数のいずれかです。関数は次のルート `to` を第一引数、現在のルート `from` を第二引数として受け取り、どちらも Vue ルートオブジェクトです。

[ルートオブジェクト](/docs/api/composables/use-route)の利用可能なプロパティについて詳しく学んでください。

### `middleware`

- **Type:** `RouteMiddleware`

第二引数は `RouteMiddleware` 型の関数です。上記と同様に、`to` と `from` のルートオブジェクトを提供します。`addRouteMiddleware()` の第一引数がすでに関数として渡されている場合はオプションになります。

### `options`

- **Type:** `AddRouteMiddlewareOptions`

オプションの `options` 引数では、ルーターミドルウェアがグローバルかどうかを示すために `global` の値を `true` に設定できます（デフォルトでは `false` に設定）。

## 例

### 名前付きルートミドルウェア

名前付きルートミドルウェアは、第一引数に文字列、第二引数に関数を提供することで定義されます:

```ts [plugins/my-plugin.ts]
export default defineNuxtPlugin(() => {
  addRouteMiddleware('named-middleware', () => {
    console.log('Nuxt プラグインで名前付きミドルウェアが追加されました')
  })
})
```

プラグインで定義された場合、`middleware/` ディレクトリにある同名の既存のミドルウェアをオーバーライドします。

### グローバルルートミドルウェア

グローバルルートミドルウェアは 2 つの方法で定義できます:

- 名前なしで関数を第一引数として直接渡す。自動的にグローバルミドルウェアとして扱われ、すべてのルート変更で適用されます。

  ```ts [plugins/my-plugin.ts]
  export default defineNuxtPlugin(() => {
    addRouteMiddleware((to, from) => {
      console.log('すべてのルート変更で実行される匿名グローバルミドルウェア')
    })
  })
  ```

- ルートミドルウェアがグローバルかどうかを示すために、オプションの第 3 引数 `{ global: true }` を設定する。

  ```ts [plugins/my-plugin.ts]
  export default defineNuxtPlugin(() => {
    addRouteMiddleware('global-middleware', (to, from) => {
        console.log('すべてのルート変更で実行されるグローバルミドルウェア')
      },
      { global: true }
    )
  })
  ```
