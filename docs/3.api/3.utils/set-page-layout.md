---
title: 'setPageLayout'
description: setPageLayout は、ページのレイアウトを動的に変更することを可能にします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

::important
`setPageLayout` は、ページのレイアウトを動的に変更することを可能にします。Nuxt context へのアクセスに依存するため、[Nuxt context](/docs/guide/going-further/nuxt-app#the-nuxt-context) 内でのみ呼び出すことができます。
::

```ts [middleware/custom-layout.ts]
export default defineNuxtRouteMiddleware((to) => {
  // ナビゲーション「先」のルートにレイアウトを設定
  setPageLayout('other')
})
```

::note
サーバーサイドで動的にレイアウトを設定することを選択した場合、ハイドレーションミスマッチを避けるため、Vue によってレイアウトがレンダリングされる前（つまり、プラグインまたはルートミドルウェア内で）に設定する「必要があります」。
::
