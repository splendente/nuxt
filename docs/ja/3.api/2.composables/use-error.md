---
title: "useError"
description: useError composable は処理されているグローバルな Nuxt エラーを返します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/error.ts
    size: xs
---

## 使用方法

`useError` composable は処理されているグローバルな Nuxt エラーを返し、クライアントとサーバーの両方で利用可能です。アプリ全体でリアクティブで SSR に優しいエラー状態を提供します。

```ts
const error = useError()
```

この composable をコンポーネント、ページ、またはプラグイン内で使用して、現在の Nuxt エラーにアクセスしたり反応したりできます。

## 型

```ts
interface NuxtError<DataT = unknown> {
  statusCode: number
  statusMessage: string
  message: string
  data?: DataT
  error?: true
}

export const useError: () => Ref<NuxtError | undefined>
```

## パラメーター

この composable はパラメーターを取りません。

## 戻り値

現在の Nuxt エラーを含む `Ref` を返します（エラーがない場合は `undefined`）。エラーオブジェクトはリアクティブで、エラー状態が変更されると自動的に更新されます。

## 例

```ts
<script setup lang="ts">
const error = useError()

if (error.value) {
  console.error('Nuxt エラー:', error.value)
}
</script>
```

:read-more{to="/docs/getting-started/error-handling"}
