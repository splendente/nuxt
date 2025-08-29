---
title: 'prefetchComponents'
description: Nuxt は、コンポーネントのプリフェッチを制御するためのユーティリティを提供します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/preload.ts
    size: xs
---


コンポーネントのプリフェッチは、コンポーネントがレンダリングに使用される可能性が高いという仮定に基づいて、バックグラウンドでコードをダウンロードします。これにより、ユーザーがリクエストした場合に、コンポーネントを瞬時に読み込むことができます。コンポーネントは、ユーザーが明示的にリクエストすることなく、将来の使用に備えてダウンロードされ、キャッシュされます。

`prefetchComponents` を使用して、Nuxt アプリでグローバルに登録されている個々のコンポーネントを手動でプリフェッチします。デフォルトでは、Nuxt はこれらを非同期コンポーネントとして登録します。コンポーネント名の PascalCase バージョンを使用する必要があります。

```ts
await prefetchComponents('MyGlobalComponent')

await prefetchComponents(['MyGlobalComponent1', 'MyGlobalComponent2'])
```

::note
現在の実装は、プリフェッチではなくコンポーネントをプリロードすることにより、[`preloadComponents`](/docs/api/utils/preload-components) とまったく同じ動作をします。この動作を改善するために取り組んでいます。
::

::note
サーバーでは、`prefetchComponents` は何の効果もありません。
::
