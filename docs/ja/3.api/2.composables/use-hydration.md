---
title: 'useHydration'
description: 'hydration サイクルを完全に制御して、サーバーからデータを設定および受信できます。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/hydrate.ts
    size: xs
---

::note
これは高度な composable で、主にプラグイン内での使用を目的として設計され、ほとんどの場合 Nuxt モジュールによって使用されます。
::

::note
`useHydration` は **SSR 中の状態同期と復元を保証する** ように設計されています。Nuxt で SSR に優しいグローバルなリアクティブ状態を作成する必要がある場合は、[`useState`](/docs/api/composables/use-state) が推奨される選択肢です。
::

`useHydration` は、新しい HTTP リクエストが作成されるたびにサーバーサイドでデータを設定し、クライアントサイドでそのデータを受信する方法を提供する組み込み composable です。このように `useHydration` は hydration サイクルを完全に制御できます。

サーバー上の `get` 関数から返されたデータは、`useHydration` の第一パラメーターとして提供された一意キーの下で `nuxtApp.payload` に保存されます。hydration 中、このデータはクライアントで取得され、無駄な計算や API 呼び出しを防います。

## 使用方法

::code-group

```ts [useHydration を使用しない場合]
export default defineNuxtPlugin((nuxtApp) => {
  const myStore = new MyStore()

  if (import.meta.server) {
    nuxt.hooks.hook('app:rendered', () => {
      nuxtApp.payload.myStoreState = myStore.getState()
    })
  }

  if (import.meta.client) {
    nuxt.hooks.hook('app:created', () => {
      myStore.setState(nuxtApp.payload.myStoreState)
    })
  }
})
```

```ts [useHydration を使用する場合]
export default defineNuxtPlugin((nuxtApp) => {
  const myStore = new MyStore()

  useHydration(
    'myStoreState', 
    () => myStore.getState(), 
    (data) => myStore.setState(data)
  )
})
```
::

## 型

```ts [signature]
useHydration <T> (key: string, get: () => T, set: (value: T) => void) => void
```

## パラメーター

- `key`: Nuxt アプリケーション内でデータを識別する一意キー。
- `get`: **サーバーでのみ** 実行される関数（SSR レンダリングが完了したときに呼び出される）で、初期値を設定します。
- `set`: **クライアントでのみ** 実行される関数（初期 Vue インスタンスが作成されたときに呼び出される）で、データを受信します。
