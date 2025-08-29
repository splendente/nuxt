---
title: 'useRequestFetch'
description: 'useRequestFetch composable でサーバーサイドフェッチリクエストのリクエストコンテキストとヘッダーを転送します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

サーバーサイドフェッチリクエストを行う際に、`useRequestFetch` を使用してリクエストコンテキストとヘッダーを転送できます。

クライアントサイドでフェッチリクエストを行う場合、ブラウザは必要なヘッダーを自動的に送信します。
しかし、サーバーサイドレンダリング中にリクエストを行う場合、セキュリティ上の理由から、ヘッダーを手動で転送する必要があります。

::note
**転送すべきではない**ヘッダーはリクエストに**含まれません**。これらのヘッダーには、例えば以下が含まれます:
`transfer-encoding`, `connection`, `keep-alive`, `upgrade`, `expect`, `host`, `accept`
::

::tip
[`useFetch`](/docs/api/composables/use-fetch) composable は内部で `useRequestFetch` を使用して、リクエストコンテキストとヘッダーを自動的に転送します。
::

::code-group

```vue [pages/index.vue]
<script setup lang="ts">
// これはユーザーのヘッダーを `/api/cookies` イベントハンドラーに転送します
// 結果: { cookies: { foo: 'bar' } }
const requestFetch = useRequestFetch()
const { data: forwarded } = await useAsyncData(() => requestFetch('/api/cookies'))

// これは何も転送しません
// 結果: { cookies: {} }
const { data: notForwarded } = await useAsyncData(() => $fetch('/api/cookies')) 
</script>
```

```ts [server/api/cookies.ts]
export default defineEventHandler((event) => {
  const cookies = parseCookies(event)

  return { cookies }
})
```

::

::tip
クライアントサイドナビゲーション中のブラウザでは、`useRequestFetch` は通常の [`$fetch`](/docs/api/utils/dollarfetch) と同じように動作します。
::
