---
title: "useResponseHeader"
description: "useResponseHeader を使用してサーバーレスポンスヘッダーを設定します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

::important
この composable は Nuxt v3.14+ で利用できます。
::

組み込みの [`useResponseHeader`](/docs/api/composables/use-response-header) composable を使用して、ページ、コンポーネント、プラグイン内で任意のサーバーレスポンスヘッダーを設定できます。

```ts
// カスタムレスポンスヘッダーを設定
const header = useResponseHeader('X-My-Header');
header.value = 'my-value';
```

## 例

`useResponseHeader` を使用して、ページごとにレスポンスヘッダーを簡単に設定できます。

```vue [pages/test.vue]
<script setup>
// pages/test.vue
const header = useResponseHeader('X-My-Header');
header.value = 'my-value';
</script>

<template>
  <h1>カスタムヘッダー付きテストページ</h1>
  <p>この "/test" ページのサーバーからのレスポンスには、カスタム "X-My-Header" ヘッダーが含まれます。</p>
</template>
```

例えば Nuxt [middleware](/docs/guide/directory-structure/middleware) で `useResponseHeader` を使用して、すべてのページのレスポンスヘッダーを設定できます。

```ts [middleware/my-header-middleware.ts]
export default defineNuxtRouteMiddleware((to, from) => {
  const header = useResponseHeader('X-My-Always-Header');
  header.value = `I'm Always here!`;
});

```
