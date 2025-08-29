---
title: 'createError'
description: 追加のメタデータを持つエラーオブジェクトを作成します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/error.ts
    size: xs
---

この関数を使用して、追加のメタデータを持つエラーオブジェクトを作成できます。アプリの Vue と Nitro の両方の部分で使用でき、スローされることを意図しています。

## パラメーター

- `err`: `string | { cause, data, message, name, stack, statusCode, statusMessage, fatal }`

`createError` 関数には文字列またはオブジェクトのどちらかを渡すことができます。文字列を渡した場合、それはエラーの `message` として使用され、`statusCode` はデフォルトで `500` になります。オブジェクトを渡した場合、`statusCode`、`message`、その他のエラープロパティなど、エラーの複数のプロパティを設定できます。

## Vue App 内で

`createError` で作成したエラーをスローした場合:

- サーバーサイドでは、`clearError` でクリアできるフルスクリーンエラーページがトリガーされます。
- クライアントサイドでは、処理するための致命的でないエラーがスローされます。フルスクリーンエラーページをトリガーする必要がある場合は、`fatal: true` を設定することで実行できます。

### 例

```vue [pages/movies/[slug\\].vue]
<script setup lang="ts">
const route = useRoute()
const { data } = await useFetch(`/api/movies/${route.params.slug}`)
if (!data.value) {
  throw createError({ statusCode: 404, statusMessage: 'Page Not Found' })
}
</script>
```

## API ルート内で

サーバー API ルートでエラーハンドリングをトリガーするために `createError` を使用します。

### 例

```ts [server/api/error.ts]
export default eventHandler(() => {
  throw createError({
    statusCode: 404,
    statusMessage: 'Page Not Found'
  })
})
```

API ルートでは、クライアントサイドでアクセスできるため、短い `statusMessage` を持つオブジェクトを渡して `createError` を使用することが推奨されます。そうでなければ、API ルートで `createError` に渡された `message` はクライアントに伝播されません。代替手段として、`data` プロパティを使用してクライアントにデータを渡すことができます。いずれの場合も、潜在的なセキュリティ問題を避けるため、動的なユーザー入力をメッセージに含めることは常に避けることを検討してください。

:read-more{to="/docs/getting-started/error-handling"}
