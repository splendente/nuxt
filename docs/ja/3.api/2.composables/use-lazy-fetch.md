---
title: 'useLazyFetch'
description: useFetch のラッパーで、ナビゲーションを即座にトリガーします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/fetch.ts
    size: xs
---

## 説明

デフォルトでは、[`useFetch`](/docs/api/composables/use-fetch) は非同期ハンドラーが解決されるまでナビゲーションをブロックします。`useLazyFetch` は [`useFetch`](/docs/api/composables/use-fetch) のラッパーで、`lazy` オプションを `true` に設定してハンドラーが解決される前にナビゲーションをトリガーします。

::note
`useLazyFetch` は [`useFetch`](/docs/api/composables/use-fetch) と同じシグネチャを持ちます。
::

::note
このモードで `useLazyFetch` を await することは、呼び出しが初期化されることを保証するだけです。クライアントサイドナビゲーションでは、データがすぐに使用できない場合があり、アプリで保留中状態を適切に処理することを確認してください。
::

:read-more{to="/docs/api/composables/use-fetch"}

## 例

```vue [pages/index.vue]
<script setup lang="ts">
/* フェッチが完了する前にナビゲーションが発生します。
 * コンポーネントのテンプレート内で 'pending' と 'error' 状態を直接処理してください
 */
const { status, data: posts } = await useLazyFetch('/api/posts')
watch(posts, (newPosts) => {
  // posts は初期値が null の可能性があるため、即座に
  // その内容にアクセスできませんが、監視することができます。
})
</script>

<template>
  <div v-if="status === 'pending'">
    Loading ...
  </div>
  <div v-else>
    <div v-for="post in posts">
      <!-- 何かをする -->
    </div>
  </div>
</template>
```

::note
`useLazyFetch` はコンパイラによって変換される予約関数名であるため、独自の関数に `useLazyFetch` という名前を付けるべきではありません。
::

:read-more{to="/docs/getting-started/data-fetching"}
