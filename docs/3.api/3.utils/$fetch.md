---
title: "$fetch"
description: Nuxt は ofetch を使用して HTTP リクエストを行うための $fetch ヘルパーをグローバルに公開しています。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/entry.ts
    size: xs
---

Nuxt は [ofetch](https://github.com/unjs/ofetch) を使用して、Vue アプリや API ルート内で HTTP リクエストを行うための `$fetch` ヘルパーをグローバルに公開しています。

::tip{icon="i-lucide-rocket"}
サーバーサイドレンダリング中に `$fetch` を呼び出して内部の [API ルート](/docs/guide/directory-structure/server)をフェッチすると、関連関数を直接呼び出し（リクエストをエミュレート）、**追加の API 呼び出しを節約**します。
::

::note{color="blue" icon="i-lucide-info"}
[`useAsyncData`](/docs/api/composables/use-async-data) でラップせずにコンポーネントで `$fetch` を使用すると、データを 2 回フェッチしてしまいます：最初にサーバーで、その後 hydration 中にクライアントサイドで再度フェッチします。これは `$fetch` がサーバーからクライアントに状態を転送しないためです。したがって、クライアントがデータを再取得する必要があるため、フェッチが両方で実行されます。
::

## 使用方法

コンポーネントデータをフェッチする際にデータの二重フェッチを防ぐため、[`useFetch`](/docs/api/composables/use-fetch) または [`useAsyncData`](/docs/api/composables/use-async-data) + `$fetch` の使用を推奨します。

```vue [app.vue]
<script setup lang="ts">
// SSR 中にデータが 2 回フェッチされます。サーバーで 1 回、クライアントで 1 回。
const dataTwice = await $fetch('/api/item')

// SSR 中にデータはサーバーサイドでのみフェッチされ、クライアントに転送されます。
const { data } = await useAsyncData('item', () => $fetch('/api/item'))

// useAsyncData + $fetch のショートカットとして useFetch を使用することもできます
const { data } = await useFetch('/api/item')
</script>
```

:read-more{to="/docs/getting-started/data-fetching"}

クライアントサイドでのみ実行される任意のメソッドで `$fetch` を使用できます。

```vue [pages/contact.vue]
<script setup lang="ts">
async function contactForm() {
  await $fetch('/api/contact', {
    method: 'POST',
    body: { hello: 'world '}
  })
}
</script>

<template>
  <button @click="contactForm">Contact</button>
</template>
```

::tip
`$fetch` は、Nuxt 2 用に作られた [@nuxt/http](https://github.com/nuxt/http) や [@nuxtjs/axios](https://github.com/nuxt-community/axios-module) の代わりに、Nuxt で HTTP 呼び出しを行う推奨される方法です。
::

::note
開発環境で自己署名証明書を使用する（外部）HTTPS URL を `$fetch` で呼び出す場合、環境変数に `NODE_TLS_REJECT_UNAUTHORIZED=0` を設定する必要があります。
::

### ヘッダーとクッキーの渡し

ブラウザで `$fetch` を呼び出すとき、`cookie` などのユーザーヘッダーは API に直接送信されます。

しかし、サーバーサイドレンダリング中は、**サーバーサイドリクエスト偽造（SSRF）**や**認証の誤用**などのセキュリティリスクのため、`$fetch` はユーザーのブラウザクッキーを含めず、フェッチレスポンスからのクッキーを渡しません。

::code-group

```vue [pages/index.vue]
<script setup lang="ts">
// これは SSR 中にヘッダーやクッキーを転送しません
const { data } = await useAsyncData(() => $fetch('/api/cookies'))
</script>
```

```ts [server/api/cookies.ts]
export default defineEventHandler((event) => {
  const foo = getCookie(event, 'foo')
  // ... クッキーで何かを行う
})
```
::

サーバーでヘッダーとクッキーを転送する必要がある場合、手動で渡す必要があります:

```vue [pages/index.vue]
<script setup lang="ts">
// これはユーザーのヘッダーとクッキーを `/api/cookies` に転送します
const requestFetch = useRequestFetch()
const { data } = await useAsyncData(() => requestFetch('/api/cookies'))
</script>
```

しかし、サーバーで相対 URL で `useFetch` を呼び出すとき、Nuxt は [`useRequestFetch`](/docs/api/composables/use-request-fetch) を使用してヘッダーとクッキーをプロキシします（`host` などの転送されるべきではないヘッダーを除く）。
