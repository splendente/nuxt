---
title: "defineNuxtRouteMiddleware"
description: "defineNuxtRouteMiddleware ヘルパー関数を使用して、名前付きルートミドルウェアを作成します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

ルートミドルウェアは、Nuxt アプリケーションの [`middleware/`](/docs/guide/directory-structure/middleware) に保存されます（[別途設定](/docs/api/nuxt-config#middleware)されていない限り）。

## 型

```ts
defineNuxtRouteMiddleware(middleware: RouteMiddleware) => RouteMiddleware

interface RouteMiddleware {
  (to: RouteLocationNormalized, from: RouteLocationNormalized): ReturnType<NavigationGuard>
}
```

## パラメーター

### `middleware`

- **型**: `RouteMiddleware`

Vue Router のルートロケーションオブジェクトを 2 つのパラメーターとして受け取る関数です。最初は次のルート `to`、2 番目は現在のルート `from` です。

`RouteLocationNormalized` の利用可能なプロパティについては、**[Vue Router docs](https://router.vuejs.org/api/#RouteLocationNormalized)** を参照してください。

## 例

### エラーページの表示

ルートミドルウェアを使用してエラーをスローし、役立つエラーメッセージを表示できます:

```ts [middleware/error.ts]
export default defineNuxtRouteMiddleware((to) => {
  if (to.params.id === '1') {
    throw createError({ statusCode: 404, statusMessage: 'Page Not Found' })
  }
})
```

上記のルートミドルウェアは、ユーザーを `~/error.vue` ファイルで定義されたカスタムエラーページにリダイレクトし、ミドルウェアから渡されたエラーメッセージとコードを公開します。

### リダイレクト

認証ステータスに基づいてユーザーを異なるルートにリダイレクトするために、ルートミドルウェア内で [`useState`](/docs/api/composables/use-state) を `navigateTo` ヘルパー関数と組み合わせて使用します:

```ts [middleware/auth.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  const auth = useState('auth')

  if (!auth.value.isAuthenticated) {
    return navigateTo('/login')
  }

  if (to.path !== '/dashboard') {
    return navigateTo('/dashboard')
  }
})
```

[navigateTo](/docs/api/utils/navigate-to) と [abortNavigation](/docs/api/utils/abort-navigation) は両方とも、`defineNuxtRouteMiddleware` 内で使用できるグローバルに利用可能なヘルパー関数です。
