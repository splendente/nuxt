---
title: 'preloadRouteComponents'
description: preloadRouteComponents は、Nuxt アプリで個々のページを手動でプリロードすることを可能にします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/preload.ts
    size: xs
---

ルートのプリロードは、ユーザーが将来ナビゲーションする可能性がある指定されたルートのコンポーネントを読み込みます。これにより、コンポーネントがより早く利用可能になり、ナビゲーションをブロックする可能性が低くなり、パフォーマンスが向上します。

::tip{icon="i-lucide-rocket"}
`NuxtLink` コンポーネントを使用している場合、Nuxt は必要なルートを自動的にプリロードします。
::

:read-more{to="/docs/api/components/nuxt-link"}

## 例

`navigateTo` を使用するときにルートをプリロードします。

```ts
// レンダリングをブロックしないよう、この非同期関数を await しません
// このコンポーネントのセットアップ関数
preloadRouteComponents('/dashboard')

const submit = async () => {
  const results = await $fetch('/api/authentication')

  if (results.token) {
    await navigateTo('/dashboard')
  }
}
```

:read-more{to="/docs/api/utils/navigate-to"}

::note
サーバーでは、`preloadRouteComponents` は何の効果もありません。
::
