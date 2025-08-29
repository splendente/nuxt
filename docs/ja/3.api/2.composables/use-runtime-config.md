---
title: 'useRuntimeConfig'
description: 'useRuntimeConfig composable でランタイム設定変数にアクセスします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/nuxt.ts
    size: xs
---

## 使用方法

```vue [app.vue]
<script setup lang="ts">
const config = useRuntimeConfig()
</script>
```

```ts [server/api/foo.ts]
export default defineEventHandler((event) => {
  const config = useRuntimeConfig(event)
})
```

:read-more{to="/docs/guide/going-further/runtime-config"}

## ランタイム設定の定義

以下の例では、パブリック API ベース URL とサーバーでのみアクセス可能なシークレット API トークンを設定する方法を示しています。

`runtimeConfig` 変数は常に `nuxt.config` 内で定義すべきです。

```ts [nuxt.config.ts]
export default defineNuxtConfig({
  runtimeConfig: {
    // プライベートキーはサーバーでのみ利用可能
    apiSecret: '123',

    // クライアントに公開されるパブリックキー
    public: {
      apiBase: process.env.NUXT_PUBLIC_API_BASE || '/api'
    }
  }
})
```

::note
サーバーでアクセスできる必要がある変数は `runtimeConfig` の中に直接追加します。クライアントとサーバーの両方でアクセスできる必要がある変数は `runtimeConfig.public` で定義します。
::

:read-more{to="/docs/guide/going-further/runtime-config"}

## ランタイム設定へのアクセス

ランタイム設定にアクセスするには、`useRuntimeConfig()` composable を使用できます:

```ts [server/api/test.ts]
export default defineEventHandler((event) => {
  const config = useRuntimeConfig(event)

  // パブリック変数にアクセス
  const result = await $fetch(`/test`, {
    baseURL: config.public.apiBase,
    headers: {
      // プライベート変数にアクセス（サーバーでのみ利用可能）
      Authorization: `Bearer ${config.apiSecret}`
    }
  })
  return result
}
```

この例では、`apiBase` が `public` 名前空間内で定義されているため、サーバーとクライアントの両方で普遍的にアクセス可能ですが、`apiSecret` は**サーバーサイドでのみアクセス可能**です。

## 環境変数

`NUXT_` で始まる一致する環境変数名を使用してランタイム設定値を更新することが可能です。

:read-more{to="/docs/guide/going-further/runtime-config"}

### `.env` ファイルの使用

`.env` ファイル内で環境変数を設定し、**開発**および**ビルド/生成**中にアクセスできるようにすることができます。

```ini [.env]
NUXT_PUBLIC_API_BASE = "https://api.localhost:5555"
NUXT_API_SECRET = "123"
```

::note
`.env` ファイル内で設定された環境変数は、**開発**および**ビルド/生成**中に Nuxt アプリで `process.env` を使用してアクセスされます。
::

::warning
**プロダクションランタイム**では、プラットフォームの環境変数を使用すべきであり、`.env` は使用されません。
::

:read-more{to="/docs/guide/directory-structure/env"}

## `app` 名前空間

Nuxt は runtime-config で `app` 名前空間を使用し、`baseURL` や `cdnURL` などのキーを含みます。環境変数を設定することで、ランタイムにそれらの値をカスタマイズできます。

::note
これは予約された名前空間です。`app` 内に追加のキーを導入しないでください。
::

### `app.baseURL`

デフォルトでは、`baseURL` は `'/'` に設定されています。

しかし、`NUXT_APP_BASE_URL` を環境変数として設定することで、ランタイムに `baseURL` を更新できます。

その後、`config.app.baseURL` を使用してこの新しいベース URL にアクセスできます:

```ts [/plugins/my-plugin.ts]
export default defineNuxtPlugin((NuxtApp) => {
  const config = useRuntimeConfig()

  // baseURL を普遍的にアクセス
  const baseURL = config.app.baseURL
})
```

### `app.cdnURL`

この例では、カスタム CDN URL を設定し、`useRuntimeConfig()` を使用してアクセスする方法を示しています。

`NUXT_APP_CDN_URL` 環境変数を使用して、`.output/public` 内の静的アセットを提供するためにカスタム CDN を使用できます。

その後、`config.app.cdnURL` を使用して新しい CDN URL にアクセスできます。

```ts [server/api/foo.ts]
export default defineEventHandler((event) => {
  const config = useRuntimeConfig(event)

  // cdnURL を普遍的にアクセス
  const cdnURL = config.app.cdnURL
})
```

:read-more{to="/docs/guide/going-further/runtime-config"}
