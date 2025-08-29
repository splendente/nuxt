---
title: useLazyAsyncData
description: useAsyncData のラッパーで、ナビゲーションを即座にトリガーします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

## 説明

デフォルトでは、[`useAsyncData`](/docs/api/composables/use-async-data) は非同期ハンドラーが解決されるまでナビゲーションをブロックします。`useLazyAsyncData` は [`useAsyncData`](/docs/api/composables/use-async-data) のラッパーで、`lazy` オプションを `true` に設定してハンドラーが解決される前にナビゲーションをトリガーします。

::note
`useLazyAsyncData` は [`useAsyncData`](/docs/api/composables/use-async-data) と同じシグネチャを持ちます。
::

:read-more{to="/docs/api/composables/use-async-data"}

## 例

```vue [pages/index.vue]
<script setup lang="ts">
/* フェッチが完了する前にナビゲーションが発生します。
  コンポーネントのテンプレート内で 'pending' と 'error' 状態を直接処理してください
*/
const { status, data: count } = await useLazyAsyncData('count', () => $fetch('/api/count'))

watch(count, (newCount) => {
  // count は初期値が null の可能性があるため、即座に
  // その内容にアクセスできませんが、監視することができます。
})
</script>

<template>
  <div>
    {{ status === 'pending' ? 'Loading' : count }}
  </div>
</template>
```

::warning
`useLazyAsyncData` はコンパイラによって変換される予約関数名であるため、独自の関数に `useLazyAsyncData` という名前を付けるべきではありません。
::

:read-more{to="/docs/getting-started/data-fetching"}
