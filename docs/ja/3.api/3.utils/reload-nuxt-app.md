---
title: 'reloadNuxtApp'
description: reloadNuxtApp はページのハードリロードを実行します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/chunk.ts
    size: xs
---

::note
`reloadNuxtApp` は、アプリのハードリロードを実行し、サーバーからページとその依存関係を再リクエストします。
::

デフォルトでは、アプリの現在の `state`（つまり、`useState` でアクセスできる任意の状態）も保存します。

::read-more{to="/docs/guide/going-further/experimental-features#restorestate" icon="i-lucide-star"}
`nuxt.config` ファイルで `experimental.restoreState` オプションを有効にすることで、この状態の実験的な復元を有効にできます。
::

## 型

```ts
reloadNuxtApp(options?: ReloadNuxtAppOptions)

interface ReloadNuxtAppOptions {
  ttl?: number
  force?: boolean
  path?: string
  persistState?: boolean
}
```

### `options` (オプション)

**型**: `ReloadNuxtAppOptions`

以下のプロパティを受け取るオブジェクト:

- `path` (オプション)

  **型**: `string`

  **デフォルト**: `window.location.pathname`

  リロードするパス（現在のパスがデフォルト）。これが現在のウィンドウロケーションと異なる場合、
  ナビゲーションをトリガーし、ブラウザ履歴にエントリを追加します。

- `ttl` (オプション)

  **型**: `number`

  **デフォルト**: `10000`

  今後のリロードリクエストを無視するミリ秒数。この期間内に再度呼び出された場合、
  `reloadNuxtApp` はアプリをリロードしません。リロードループを回避するためです。

- `force` (オプション)

  **型**: `boolean`

  **デフォルト**: `false`

  このオプションは、リロードループ保護を完全にバイパスし、以前に指定された TTL 内でリロードが発生した場合でも
  強制的にリロードを実行することを可能にします。

- `persistState` (オプション)

  **型**: `boolean`

  **デフォルト**: `false`

  現在の Nuxt 状態を sessionStorage（`nuxt:reload:state` として）にダンプするかどうか。デフォルトでは、
  `experimental.restoreState` も設定されていない限り、または状態の復元を自分で処理しない限り、リロード時に効果はありません。
