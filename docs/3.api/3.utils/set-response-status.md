---
title: 'setResponseStatus'
description: setResponseStatus は、レスポンスの statusCode（および必要に応じて statusMessage）を設定します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

Nuxt は、一流のサーバーサイドレンダリングサポート用のコンポーザブルとユーティリティを提供します。

`setResponseStatus` は、レスポンスの statusCode（および必要に応じて statusMessage）を設定します。

::important
`setResponseStatus` は [Nuxt context](/docs/guide/going-further/nuxt-app#the-nuxt-context) 内でのみ呼び出すことができます。
::

```js
const event = useRequestEvent()

// event はブラウザでは undefined になります
if (event) {
  // カスタム 404 ページのためにステータスコードを 404 に設定
  setResponseStatus(event, 404)

  // ステータスメッセージも設定
  setResponseStatus(event, 404, 'Page Not Found')
}
```

::note
ブラウザでは、`setResponseStatus` は何の効果もありません。
::

:read-more{to="/docs/getting-started/error-handling"}
