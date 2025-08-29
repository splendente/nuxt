---
title: "useRoute"
description: useRoute composable は現在のルートを返します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

::note
Vue コンポーネントのテンプレート内では、`$route` を使用してルートにアクセスできます。
::

## 例

以下の例では、ダイナミックページパラメーター - `slug` - を URL の一部として使用し、[`useFetch`](/docs/api/composables/use-fetch) で API を呼び出します。

```html [~/pages/[slug\\].vue]
<script setup lang="ts">
const route = useRoute()
const { data: mountain } = await useFetch(`/api/mountains/${route.params.slug}`)
</script>

<template>
  <div>
    <h1>{{ mountain.title }}</h1>
    <p>{{ mountain.description }}</p>
  </div>
</template>
```

ルートクエリパラメーターにアクセスする必要がある場合（例えばパス `/test?example=true` の `example`）、`useRoute().params` の代わりに `useRoute().query` を使用できます。

## API

ダイナミックパラメーターとクエリパラメーターのほかに、`useRoute()` は現在のルートに関連する以下の算出された参照も提供します:

- `fullPath`: パス、クエリ、ハッシュを含む現在のルートに関連付けられたエンコードされた URL
- `hash`: # で始まる URL のデコードされたハッシュ部分
- `query`: ルートクエリパラメーターへのアクセス
- `matched`: 現在のルートロケーションで正規化されたマッチしたルートの配列
- `meta`: レコードに添付されたカスタムデータ
- `name`: ルートレコードの一意の名前
- `path`: URL のエンコードされたパス名部分
- `redirectedFrom`: 現在のルートロケーションに着く前にアクセスしようとしたルートロケーション

::note
ブラウザはリクエスト時に [URL フラグメント](https://url.spec.whatwg.org/#concept-url-fragment)（例えば `#foo`）を送信しません。そのためテンプレートで `route.fullPath` を使用すると、クライアントではフラグメントが含まれるがサーバーでは含まれないため、hydration の問題が発生する可能性があります。
::

:read-more{icon="i-simple-icons-vuedotjs" to="https://router.vuejs.org/api/#RouteLocationNormalizedLoaded"}
