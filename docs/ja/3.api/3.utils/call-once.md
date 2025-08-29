---
title: "callOnce"
description: "SSR または CSR 中に指定された関数またはコードブロックを 1 回実行します。"
navigation:
  badge: New
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/once.ts
    size: xs
---

::important
このユーティリティは [Nuxt v3.9](/blog/v3-9) 以降で利用できます。
::

## 目的

`callOnce` 関数は、以下の間に指定された関数またはコードブロックを 1 回だけ実行するように設計されています：
- サーバーサイドレンダリング中（hydration ではない）
- クライアントサイドナビゲーション中

これは、イベントのログ記録やグローバル状態の設定など、1 回だけ実行すべきコードに便利です。

## 使用方法

`callOnce` のデフォルトモードはコードを 1 回だけ実行することです。たとえば、コードがサーバーで実行された場合、クライアントで再度実行されることはありません。また、クライアントで `callOnce` を複数回呼び出しても（たとえばこのページに戻った場合）再度実行されることはありません。

```vue [app.vue]
<script setup lang="ts">
const websiteConfig = useState('config')

await callOnce(async () => {
  console.log('このメッセージは 1 回だけログ出力されます')
  websiteConfig.value = await $fetch('https://my-cms.com/api/website-config')
})
</script>
```

初期のサーバー/クライアントの二重読み込みを避けながら、すべてのナビゲーションで実行することも可能です。このために `navigation` モードを使用できます:

```vue [app.vue]
<script setup lang="ts">
const websiteConfig = useState('config')

await callOnce(async () => {
  console.log('このメッセージは 1 回だけログ出力され、その後はすべてのクライアントサイドナビゲーションでログ出力されます')
  websiteConfig.value = await $fetch('https://my-cms.com/api/website-config')
}, { mode: 'navigation' })
</script>
```

::important
`navigation` モードは [Nuxt v3.15](/blog/v3-15) 以降で利用できます。
::

::tip{to="/docs/getting-started/state-management#usage-with-pinia"}
`callOnce` は [Pinia モジュール](/modules/pinia)と組み合わせてストアアクションを呼び出すのに便利です。
::

:read-more{to="/docs/getting-started/state-management"}

::warning
`callOnce` は何も返しません。SSR 中にデータフェッチを行いたい場合は、[`useAsyncData`](/docs/api/composables/use-async-data) または [`useFetch`](/docs/api/composables/use-fetch) を使用すべきです。
::

::note
`callOnce` はセットアップ関数、プラグイン、またはルートミドルウェアで直接呼び出されることを意図した composable です。ページが hydrate したときにクライアントで関数を再呼び出ししないように、Nuxt ペイロードにデータを追加する必要があるためです。
::

## 型

```ts
callOnce (key?: string, fn?: (() => any | Promise<any>), options?: CallOnceOptions): Promise<void>
callOnce(fn?: (() => any | Promise<any>), options?: CallOnceOptions): Promise<void>

type CallOnceOptions = {
  /**
   * callOnce 関数の実行モード
   * @default 'render'
   */
  mode?: 'navigation' | 'render'
}
```

## パラメーター

- `key`: コードが 1 回実行されることを保証する一意のキー。キーを提供しない場合、`callOnce` のインスタンスのファイルと行番号に固有のキーが生成されます。
- `fn`: 1 回実行する関数。非同期でも構いません。
- `options`: モードを設定し、ナビゲーション時に再実行する（`navigation`）か、アプリのライフタイムで 1 回だけ（`render`）実行するかを選択します。デフォルトは `render` です。
  - `render`: 初期レンダー中に 1 回実行（SSR または CSR） - デフォルトモード
  - `navigation`: 初期レンダー中に 1 回、その後のクライアントサイドナビゲーションごとに 1 回実行
