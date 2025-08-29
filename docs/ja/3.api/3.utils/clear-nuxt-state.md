---
title: 'clearNuxtState'
description: useState のキャッシュされた state を削除します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/state.ts
    size: xs
---

::note
このメソッドは、`useState` の state を無効化したい場合に便利です。
::

## 型

```ts
clearNuxtState (keys?: string | string[] | ((key: string) => boolean)): void
```

## パラメーター

- `keys`: [`useState`](/docs/api/composables/use-state) で使用されるキーの単一または配列で、それらのキャッシュされた state を削除します。キーが提供されない場合、**すべての state** が無効化されます。
