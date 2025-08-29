---
title: "useRequestHeader"
description: "useRequestHeader を使用して特定の受信リクエストヘッダーにアクセスします。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

組み込みの [`useRequestHeader`](/docs/api/composables/use-request-header) composable を使用して、ページ、コンポーネント、プラグイン内で任意の受信リクエストヘッダーにアクセスできます。

```ts
// authorization リクエストヘッダーを取得
const authorization = useRequestHeader('authorization')
```

::tip
ブラウザでは、`useRequestHeader` は `undefined` を返します。
::

## 例

`useRequestHeader` を使用して、ユーザーが授権されているかどうかを簡単に判断できます。

以下の例では、`authorization` リクエストヘッダーを読み取り、ユーザーが制限されたリソースにアクセスできるかどうかを判断します。

```ts [middleware/authorized-only.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  if (!useRequestHeader('authorization')) {
    return navigateTo('/not-authorized')
  }
})
```
