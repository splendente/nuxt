---
title: 'clearNuxtData'
description: useAsyncData と useFetch のキャッシュされたデータ、エラーステータス、実行中の Promise を削除します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

::note
このメソッドは、他のページのデータフェッチを無効化したい場合に便利です。
::

## 型

```ts
clearNuxtData (keys?: string | string[] | ((key: string) => boolean)): void
```

## パラメーター

* `keys`: [`useAsyncData`](/docs/api/composables/use-async-data) で使用されるキーの単一または配列で、それらのキャッシュされたデータを削除します。キーが提供されない場合、**すべてのデータ**が無効化されます。
