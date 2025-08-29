---
title: 'useRouteAnnouncer'
description: この composable はページタイトルの変更を監視し、それに応じてアナウンサーメッセージを更新します。
navigation:
  badge: New
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/route-announcer.ts
    size: xs
---

::important
この composable は Nuxt v3.12+ で利用できます。
::

## 説明

ページタイトルの変更を監視し、それに応じてアナウンサーメッセージを更新する composable です。[`<NuxtRouteAnnouncer>`](/docs/api/components/nuxt-route-announcer) で使用され、制御できます。
Unhead の [`dom:rendered`](https://unhead.unjs.io/docs/typescript/head/api/hooks/dom-rendered) にフックしてページのタイトルを読み取り、アナウンサーメッセージとして設定します。

## パラメーター

- `politeness`: スクリーンリーダーアナウンスの緊急度を設定：`off`（アナウンスを無効化）、`polite`（無音を待つ）、または `assertive`（即座に中断）。（デフォルト `polite`）。

## プロパティ

### `message`

- **type**: `Ref<string>`
- **description**: アナウンスするメッセージ

### `politeness`

- **type**: `Ref<string>`
- **description**: スクリーンリーダーアナウンスの緊急度レベル `off`、`polite`、または `assertive`

## メソッド

### `set(message, politeness = "polite")`

緊急度レベルとともにアナウンスするメッセージを設定します。

### `polite(message)`

`politeness = "polite"` でメッセージを設定します

### `assertive(message)`

`politeness = "assertive"` でメッセージを設定します

## 例

```vue [pages/index.vue]
<script setup lang="ts">
  const { message, politeness, set, polite, assertive } = useRouteAnnouncer({
    politeness: 'assertive'
  })
</script>
```
