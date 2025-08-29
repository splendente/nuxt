---
title: 'prerenderRoutes'
description: prerenderRoutes は、追加のルートをプリレンダリングするよう Nitro にヒントを与えます。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

プリレンダリング時に、生成されたページの HTML に URL が表示されない場合でも、追加のパスをプリレンダリングするよう Nitro にヒントを与えることができます。

::important
`prerenderRoutes` は [Nuxt context](/docs/guide/going-further/nuxt-app#the-nuxt-context) 内でのみ呼び出すことができます。
::

::note
`prerenderRoutes` はプリレンダリング中に実行される必要があります。プリレンダリングされていない動的ページ・ルートで `prerenderRoutes` が使用される場合、実行されません。
::

```js
const route = useRoute()

prerenderRoutes('/')
prerenderRoutes(['/', '/about'])
```

::note
ブラウザ内、またはプリレンダリング外で呼び出された場合、`prerenderRoutes` は何の効果もありません。
::

API ルートもプリレンダリングできます。これは完全な静的生成サイト（SSG）に特に有用です。なぜなら、利用可能なサーバーがあるかのようにデータを `$fetch` できるからです！

```js
prerenderRoutes('/api/content/article/name-of-article')

// アプリの後の部分で
const articleContent = await $fetch('/api/content/article/name-of-article', {
  responseType: 'json',
})
```

::warning
本番環境でプリレンダリングされた API ルートは、デプロイ先のプロバイダーによっては、期待されるレスポンスヘッダーを返さない場合があります。例えば、JSON レスポンスが `application/octet-stream` コンテンツタイプで配信される可能性があります。
プリレンダリングされた API ルートを取得する際は、常に手動で `responseType` を設定してください。
::
