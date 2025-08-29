---
title: 'preloadComponents'
description: Nuxt は、コンポーネントのプリロードを制御するためのユーティリティを提供します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/preload.ts
    size: xs
---

コンポーネントのプリロードは、ページがまもなく必要とするコンポーネントを読み込み、レンダリングライフサイクルの早い段階で読み込みを開始したいものです。これにより、コンポーネントがより早く利用可能になり、ページのレンダリングをブロックする可能性が低くなり、パフォーマンスが向上します。

`preloadComponents` を使用して、Nuxt アプリでグローバルに登録されている個々のコンポーネントを手動でプリロードします。デフォルトでは、Nuxt はこれらを非同期コンポーネントとして登録します。コンポーネント名の PascalCase バージョンを使用する必要があります。

```js
await preloadComponents('MyGlobalComponent')

await preloadComponents(['MyGlobalComponent1', 'MyGlobalComponent2'])
```

::note
サーバーでは、`preloadComponents` は何の効果もありません。
::
