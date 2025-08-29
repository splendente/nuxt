---
title: 'useLoadingIndicator'
description: この composable はアプリページの読み込み状態へのアクセスを提供します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/loading-indicator.ts
    size: xs
---

## 説明

ページの読み込み状態を返す composable です。[`<NuxtLoadingIndicator>`](/docs/api/components/nuxt-loading-indicator) によって使用され、制御可能です。
[`page:loading:start`](/docs/api/advanced/hooks#app-hooks-runtime) と [`page:loading:end`](/docs/api/advanced/hooks#app-hooks-runtime) にフックして状態を変更します。

## パラメーター

- `duration`: 読み込みバーの継続時間、ミリ秒単位（デフォルト `2000`）。
- `throttle`: 表示と非表示のスロットリング、ミリ秒単位（デフォルト `200`）。
- `estimatedProgress`: デフォルトでは Nuxt は 100% に近づくとバックオフします。プログレス推定をカスタマイズするためのカスタム関数を提供できます。これは読み込みバーの継続時間（上記）と経過時間を受け取る関数です。0 から 100 の値を返す必要があります。

## プロパティ

### `isLoading`

- **type**: `Ref<boolean>`
- **description**: 読み込み状態

### `error`

- **type**: `Ref<boolean>`
- **description**: エラー状態

### `progress`

- **type**: `Ref<number>`
- **description**: プログレス状態。`0` から `100` まで。

## メソッド

### `start()`

`isLoading` を true に設定し、`progress` 値の増加を開始します。`start` は `{ force: true }` オプションを受け入れ、インターバルをスキップして即座に読み込み状態を表示します。

### `set()`

`progress` 値を特定の値に設定します。`set` は `{ force: true }` オプションを受け入れ、インターバルをスキップして即座に読み込み状態を表示します。

### `finish()`

`progress` 値を `100` に設定し、すべてのタイマーとインターバルを停止し、`500` ms 後に読み込み状態をリセットします。`finish` は `{ force: true }` オプションで状態リセット前のインターバルをスキップし、`{ error: true }` で読み込みバーの色を変更して error プロパティを true に設定します。

### `clear()`

`finish()` によって使用されます。composable によって使用されるすべてのタイマーとインターバルをクリアします。

## 例

```vue
<script setup lang="ts">
  const { progress, isLoading, start, finish, clear } = useLoadingIndicator({
    duration: 2000,
    throttle: 200,
    // これはデフォルトのプログレス計算方法です
    estimatedProgress: (duration, elapsed) => (2 / Math.PI * 100) * Math.atan(elapsed / duration * 100 / 50)
  })
</script>
```

```vue
<script setup lang="ts">
  const { start, set } = useLoadingIndicator()
  // set(0, { force: true }) と同じ
  // プログレスを 0 に設定し、即座に読み込みを表示
  start({ force: true })
</script>
```
