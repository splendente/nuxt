---
title: useHead
description: useHead は Nuxt アプリの個別ページの head プロパティをカスタマイズします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/unjs/unhead/blob/main/packages/vue/src/composables.ts
    size: xs
---

[`useHead`](/docs/api/composables/use-head) composable 関数は、[Unhead](https://unhead.unjs.io) を使用して head タグをプログラム的かつリアクティブな方法で管理できます。データがユーザーやその他の信頼できないソースから来る場合は、[`useHeadSafe`](/docs/api/composables/use-head-safe) を確認することをお勧めします。

:read-more{to="/docs/getting-started/seo-meta"}

## 型

```ts
useHead(meta: MaybeComputedRef<MetaObject>): void
```

以下は [`useHead`](/docs/api/composables/use-head) の非リアクティブ型です。

```ts
interface MetaObject {
  title?: string
  titleTemplate?: string | ((title?: string) => string)
  base?: Base
  link?: Link[]
  meta?: Meta[]
  style?: Style[]
  script?: Script[]
  noscript?: Noscript[]
  htmlAttrs?: HtmlAttributes
  bodyAttrs?: BodyAttributes
}
```

より詳細な型については [@unhead/vue](https://github.com/unjs/unhead/blob/main/packages/vue/src/types/schema.ts) を参照してください。

::note
`useHead` のプロパティは動的であり、`ref`、`computed`、`reactive` プロパティを受け付けます。`meta` パラメーターは、オブジェクト全体をリアクティブにするためにオブジェクトを返す関数を受け付けることもできます。
::

## パラメーター

### `meta`

**Type**: `MetaObject`

以下の head メタデータを受け付けるオブジェクト:

- `meta`: 配列の各要素は新しく作成された `<meta>` タグにマッピングされ、オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Array<Record<string, any>>`
- `link`: 配列の各要素は新しく作成された `<link>` タグにマッピングされ、オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Array<Record<string, any>>`
- `style`: 配列の各要素は新しく作成された `<style>` タグにマッピングされ、オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Array<Record<string, any>>`
- `script`: 配列の各要素は新しく作成された `<script>` タグにマッピングされ、オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Array<Record<string, any>>`
- `noscript`: 配列の各要素は新しく作成された `<noscript>` タグにマッピングされ、オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Array<Record<string, any>>`
- `titleTemplate`: 個別ページのページタイトルをカスタマイズするための動的テンプレートを設定します。
  - **Type**: `string` | `((title: string) => string)`
- `title`: 個別ページの静的ページタイトルを設定します。
  - **Type**: `string`
- `bodyAttrs`: `<body>` タグの属性を設定します。各オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Record<string, any>`
- `htmlAttrs`: `<html>` タグの属性を設定します。各オブジェクトプロパティは対応する属性にマッピングされます。
  - **Type**: `Record<string, any>`
