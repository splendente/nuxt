---
title: "defineNuxtComponent"
description: defineNuxtComponent() は Options API で型安全なコンポーネントを定義するためのヘルパー関数です。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/component.ts
    size: xs
---

::note
`defineNuxtComponent()` は [`defineComponent()`](https://vuejs.org/api/general.html#definecomponent) と同様に Options API を使用して型安全な Vue コンポーネントを定義するためのヘルパー関数です。`defineNuxtComponent()` ラッパーは、`asyncData` と `head` コンポーネントオプションのサポートも追加します。
::

::note
Nuxt で Vue コンポーネントを宣言するには `<script setup lang="ts">` を使用することが推奨されます。
::

:read-more{to=/docs/getting-started/data-fetching}

## `asyncData()`

アプリで `setup()` を使用しないことを選択した場合、コンポーネント定義内で `asyncData()` メソッドを使用できます:

```vue [pages/index.vue]
<script lang="ts">
export default defineNuxtComponent({
  async asyncData() {
    return {
      data: {
        greetings: 'hello world!'
      }
    }
  },
})
</script>
```

## `head()`

アプリで `setup()` を使用しないことを選択した場合、コンポーネント定義内で `head()` メソッドを使用できます:

```vue [pages/index.vue]
<script lang="ts">
export default defineNuxtComponent({
  head(nuxtApp) {
    return {
      title: 'My site'
    }
  },
})
</script>
```
