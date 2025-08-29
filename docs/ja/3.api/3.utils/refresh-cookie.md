---
title: "refreshCookie"
description: "クッキーが変更されたときに useCookie の値を手動で更新します"
navigation:
  badge: New
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/cookie.ts
    size: xs
---

::important
このユーティリティは [Nuxt v3.10](/blog/v3-10) から利用可能です。
::

## 目的

`refreshCookie` 関数は、`useCookie` によって返されるクッキー値を更新するために設計されています。

これは、ブラウザで新しいクッキー値が設定されたことがわかっているときに、`useCookie` ref を更新するのに有用です。

## 使用方法

```vue [app.vue]
<script setup lang="ts">
const tokenCookie = useCookie('token')

const login = async (username, password) => {
  const token = await $fetch('/api/token', { ... }) // レスポンスで `token` クッキーを設定
  refreshCookie('token')
}

const loggedIn = computed(() => !!tokenCookie.value)
</script>
```

::note{to="/docs/guide/going-further/experimental-features#cookiestore"}
実験的な `cookieStore` オプションを有効にすると、ブラウザでクッキーが変更されたときに `useCookie` の値が自動的に更新されます。
::

## 型

```ts
refreshCookie(name: string): void
```
