---
title: "usePreviewMode"
description: "usePreviewMode を使用して Nuxt でプレビューモードをチェック・制御します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/preview.ts
    size: xs
---

# `usePreviewMode`

プレビューモードを使用すると、ユーザーに見られることなく、変更がライブサイトでどのように表示されるかを確認できます。

組み込みの `usePreviewMode` composable を使用して、Nuxt でプレビュー状態にアクセスし、制御できます。この composable がプレビューモードを検出すると、プレビューコンテンツを再レンダリングするために [`useAsyncData`](/docs/api/composables/use-async-data) と [`useFetch`](/docs/api/composables/use-fetch) に必要な更新を自動的に強制します。

```js
const { enabled, state } = usePreviewMode()
```

## オプション

### カスタム `enable` チェック

プレビューモードを有効にするためのカスタム方法を指定できます。デフォルトでは、`usePreviewMode` composable は URL に `true` と等しい `preview` パラメータがある場合にプレビューモードを有効にします（例: `http://localhost:3000?preview=true`）。使用法全体でオプションの一貫性を保ち、エラーを防ぐために、`usePreviewMode` をカスタム composable でラップできます。

```js
export function useMyPreviewMode () {
  return usePreviewMode({
    shouldEnable: () => {
      return !!route.query.customPreview
    }
  });
}
```

### デフォルト状態の変更

`usePreviewMode` は URL の `token` パラメータの値を状態に保存しようとします。この状態を変更でき、すべての [`usePreviewMode`](/docs/api/composables/use-preview-mode) 呼び出しで利用できます。

```js
const data1 = ref('data1')

const { enabled, state } = usePreviewMode({
  getState: (currentState) => {
    return { data1, data2: 'data2' }
  }
})
```

::note
`getState` 関数は返された値を現在の状態に追加するため、重要な状態を誤って上書きしないよう注意してください。
::

### `onEnable` と `onDisable` コールバックのカスタマイズ

デフォルトでは、`usePreviewMode` が有効になると、サーバーからすべてのデータを再取得するために `refreshNuxtData()` を呼び出します。

プレビューモードが無効になると、この composable は後続のルーターナビゲーション後に実行する `refreshNuxtData()` を呼び出すコールバックを添付します。

`onEnable` と `onDisable` オプションに独自の関数を提供することで、トリガーするカスタムコールバックを指定できます。

```js
const { enabled, state } = usePreviewMode({
  onEnable: () => {
    console.log('プレビューモードが有効になりました')
  },
  onDisable: () => {
    console.log('プレビューモードが無効になりました')
  }
})
```

## 例

以下の例では、コンテンツの一部がプレビューモードでのみレンダリングされるページを作成します。

```vue [pages/some-page.vue]
<script setup>
const { enabled, state } = usePreviewMode()

const { data } = await useFetch('/api/preview', {
  query: {
    apiKey: state.token
  }
})
</script>

<template>
  <div>
    ベースコンテンツ
    <p v-if="enabled">
      プレビューのみのコンテンツ: {{ state.token }}
      <br>
      <button @click="enabled = false">
        プレビューモードを無効にする
      </button>
    </p>
  </div>
</template>
```

サイトを生成して提供できます:

```bash [Terminal]
npx nuxt generate
npx nuxt preview
```

次に、見たいページの最後にクエリパラメータ `preview` を追加することで、プレビューページを見ることができます:

```js
?preview=true
```

::note
`usePreviewMode` は `nuxt dev` ではなく `nuxt generate` してから `nuxt preview` でローカルにテストする必要があります。（[preview コマンド](/docs/api/commands/preview)はプレビューモードとは関係ありません。）
::
