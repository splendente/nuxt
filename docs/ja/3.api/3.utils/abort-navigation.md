---
title: 'abortNavigation'
description: 'abortNavigation はナビゲーションの実行を防ぎ、パラメーターとしてエラーが設定されている場合はエラーをスローするヘルパー関数です。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

::warning
`abortNavigation` は [ルートミドルウェアハンドラー](/docs/guide/directory-structure/middleware)内でのみ使用できます。
::

## 型

```ts
abortNavigation(err?: Error | string): false
```

## パラメーター

### `err`

- **Type**: [`Error`](https://developer.mozilla.org/pl/docs/Web/JavaScript/Reference/Global_Objects/Error) | `string`

  `abortNavigation` によってスローされるオプションのエラー。

## 例

以下の例では、ルートミドルウェアで `abortNavigation` を使用して未承認のルートアクセスを防ぐ方法を示しています:

```ts [middleware/auth.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  const user = useState('user')

  if (!user.value.isAuthorized) {
    return abortNavigation()
  }

  if (to.path !== '/edit-post') {
    return navigateTo('/edit-post')
  }
})
```

### 文字列としての `err`

エラーを文字列として渡すことができます:

```ts [middleware/auth.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  const user = useState('user')

  if (!user.value.isAuthorized) {
    return abortNavigation('権限が不十分です。')
  }
})
```

### Error オブジェクトとしての `err`

エラーを [`Error`](https://developer.mozilla.org/pl/docs/Web/JavaScript/Reference/Global_Objects/Error) オブジェクトとして渡すことができます。例えば `catch` ブロックでキャッチしたもの:

```ts [middleware/auth.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  try {
    /* エラーをスローする可能性のあるコード */
  } catch (err) {
    return abortNavigation(err)
  }
})
```
