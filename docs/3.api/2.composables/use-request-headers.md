---
title: "useRequestHeaders"
description: "useRequestHeaders を使用して受信リクエストヘッダーにアクセスします。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

組み込みの [`useRequestHeaders`](/docs/api/composables/use-request-headers) composable を使用して、ページ、コンポーネント、プラグイン内で受信リクエストヘッダーにアクセスできます。

```js
// すべてのリクエストヘッダーを取得
const headers = useRequestHeaders()

// cookie リクエストヘッダーのみを取得
const headers = useRequestHeaders(['cookie'])
```

::tip
ブラウザでは、`useRequestHeaders` は空のオブジェクトを返します。
::

## 例

`useRequestHeaders` を使用して、SSR 中に初期リクエストの `authorization` ヘッダーにアクセスし、将来の内部リクエストにプロキシできます。

以下の例では、同形 `$fetch` 呼び出しに `authorization` リクエストヘッダーを追加します。

```vue [pages/some-page.vue]
<script setup lang="ts">
const { data } = await useFetch('/api/confidential', {
  headers: useRequestHeaders(['authorization'])
})
</script>
```
