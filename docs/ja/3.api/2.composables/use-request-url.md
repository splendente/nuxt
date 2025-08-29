---
title: 'useRequestURL'
description: 'useRequestURL composable で受信リクエスト URL にアクセスします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/url.ts
    size: xs
---

`useRequestURL` は、サーバーサイドとクライアントサイドの両方で動作する [URL オブジェクト](https://developer.mozilla.org/en-US/docs/Web/API/URL/URL)を返すヘルパー関数です。

::important
キャッシュ戦略で [Hybrid Rendering](/docs/guide/concepts/rendering#hybrid-rendering) を使用する場合、[Nitro キャッシュレイヤー](https://nitro.build/guide/cache)でキャッシュされたレスポンスを処理する際に、すべての受信リクエストヘッダーが削除されます（つまり `useRequestURL` は `host` に対して `localhost` を返します）。

[`cache.varies` オプション](https://nitro.build/guide/cache#options)を定義して、マルチテナント環境のための `host` や `x-forwarded-host` など、レスポンスのキャッシュおよび提供時に考慮されるヘッダーを指定できます。
::

::code-group

```vue [pages/about.vue]
<script setup lang="ts">
const url = useRequestURL()
</script>

<template>
  <p>URL: {{ url }}</p>
  <p>パス: {{ url.pathname }}</p>
</template>
```

```html [開発環境での結果]
<p>URL: http://localhost:3000/about</p>
<p>パス: /about</p>
```

::

::tip{icon="i-simple-icons-mdnwebdocs" to="https://developer.mozilla.org/en-US/docs/Web/API/URL#instance_properties" target="_blank"}
URL インスタンスプロパティについては MDN ドキュメントを参照してください。
::
