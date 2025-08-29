---
title: 'useRequestEvent'
description: 'useRequestEvent composable で受信リクエストイベントにアクセスします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

[Nuxt コンテキスト](/docs/guide/going-further/nuxt-app#the-nuxt-context)内で `useRequestEvent` を使用して受信リクエストにアクセスできます。

```ts
// 基礎となるリクエストイベントを取得
const event = useRequestEvent()

// URL を取得
const url = event?.path
```

::tip
ブラウザでは、`useRequestEvent` は `undefined` を返します。
::
