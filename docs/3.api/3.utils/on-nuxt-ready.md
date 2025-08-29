---
title: "onNuxtReady"
description: onNuxtReady コンポーザブルは、アプリが初期化を完了した後にコールバックを実行することを可能にします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ready.ts
    size: xs
---

::important
`onNuxtReady` はクライアントサイドでのみ実行されます。:br
アプリの初期レンダリングをブロックすべきでないコードを実行するのに最適です。
::

```ts [plugins/ready.client.ts]
export default defineNuxtPlugin(() => {
  onNuxtReady(async () => {
    const myAnalyticsLibrary = await import('my-big-analytics-library')
    // do something with myAnalyticsLibrary
  })
})
```

アプリが初期化された後でも実行することは「安全」です。この場合、コードは次のアイドルコールバックで実行されるように登録されます。
