---
title: 'defineRouteRules'
description: 'ページレベルでハイブリッドレンダリング用のルートルールを定義します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/pages/runtime/composables.ts
    size: xs
---

::read-more{to="/docs/guide/going-further/experimental-features#inlinerouterules" icon="i-lucide-star"}
この機能は実験的なもので、使用するには `nuxt.config` で `experimental.inlineRouteRules` オプションを有効にする必要があります。
::

## 使用方法

```vue [pages/index.vue]
<script setup lang="ts">
defineRouteRules({
  prerender: true
})
</script>

<template>
  <h1>Hello world!</h1>
</template>
```

以下のように変換されます:

```ts [nuxt.config.ts]
export default defineNuxtConfig({
  routeRules: {
    '/': { prerender: true }
  }
})
```

::note
[`nuxt build`](/docs/api/commands/build) を実行すると、ホームページは `.output/public/index.html` にプリレンダリングされ、静的に配信されます。
::

## 注意点

- `~/pages/foo/bar.vue` で定義されたルールは、`/foo/bar` リクエストに適用されます。
- `~/pages/foo/[id].vue` のルールは、`/foo/**` リクエストに適用されます。

ページの [`definePageMeta`](/docs/api/utils/define-page-meta) で設定されたカスタム `path` や `alias` を使用している場合など、より詳細な制御が必要な場合は、`nuxt.config` で直接 `routeRules` を設定する必要があります。

::read-more{to="/docs/guide/concepts/rendering#hybrid-rendering" icon="i-lucide-medal"}
`routeRules` についてもっと読む。
::
