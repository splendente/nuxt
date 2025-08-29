---
title: "onPrehydrate"
description: "onPrehydrate を使用して、Nuxt がページを hydrate する直前にクライアントでコールバックを実行します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/ssr.ts
    size: xs
---

::important
この composable は Nuxt v3.12+ で利用可能です。
::

`onPrehydrate` は、Nuxt がページを hydrate する直前にクライアントでコールバックを実行できる composable ライフサイクルフックです。
::note
これは高度なユーティリティで、注意して使用する必要があります。例えば、[`nuxt-time`](https://github.com/danielroe/nuxt-time/pull/251) と [`@nuxtjs/color-mode`](https://github.com/nuxt-modules/color-mode/blob/main/src/script.js) は hydration の不一致を避けるために DOM を操作します。
::

## 使用方法

Vue コンポーネントの setup 関数（例：`<script setup>` 内）またはプラグイン内で `onPrehydrate` を呼び出します。サーバー上で呼び出された時のみ効果があり、クライアントビルドには含まれません。

## 型

```ts [Signature]
export function onPrehydrate(callback: (el: HTMLElement) => void): void
export function onPrehydrate(callback: string | ((el: HTMLElement) => void), key?: string): undefined | string
```

## パラメーター

| パラメーター | 型 | 必須 | 説明 |
| ---- | --- | --- | --- |
| `callback` | `((el: HTMLElement) => void) \| string` | Yes | Nuxt が hydrate する前に実行する関数（または文字列化された関数）。文字列化されて HTML にインライン化されます。外部依存関係を持つべきでなく、コールバック外の変数を参照すべきではありません。Nuxt ランタイムが初期化される前に実行されるため、Nuxt や Vue のコンテキストに依存すべきではありません。 |
| `key` | `string` | No | （高度）prehydrate スクリプトを識別する一意のキー。複数のルートノードのような高度なシナリオで有用です。 |

## 戻り値

- コールバック関数のみで呼び出された場合は `undefined` を返します。
- コールバックとキーで呼び出された場合は文字列（prehydrate ID）を返し、高度なユースケースで `data-prehydrate-id` 属性を設定またはアクセスするために使用できます。

## 例

```vue twoslash [app.vue]
<script setup lang="ts">
declare const window: Window
// ---cut---
// Nuxt が hydrate する前にコードを実行
onPrehydrate(() => {
  console.log(window)
})

// ルート要素にアクセス
onPrehydrate((el) => {
  console.log(el.outerHTML)
  // <div data-v-inspector="app.vue:15:3" data-prehydrate-id=":b3qlvSiBeH:"> Hi there </div>
})

// 高度: 自分で `data-prehydrate-id` にアクセス/設定
const prehydrateId = onPrehydrate((el) => {})
</script>

<template>
  <div>
    Hi there
  </div>
</template>
```
