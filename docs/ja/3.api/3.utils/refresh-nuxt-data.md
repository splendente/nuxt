---
title: 'refreshNuxtData'
description: Nuxt ですべてまたは特定の asyncData インスタンスを更新します
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

`refreshNuxtData` は、[`useAsyncData`](/docs/api/composables/use-async-data)、[`useLazyAsyncData`](/docs/api/composables/use-lazy-async-data)、[`useFetch`](/docs/api/composables/use-fetch)、[`useLazyFetch`](/docs/api/composables/use-lazy-fetch) からのものを含む、すべてまたは特定の `asyncData` インスタンスを再取得するために使用されます。

::note
コンポーネントが `<KeepAlive>` によってキャッシュされ、非アクティブ状態になった場合、コンポーネントがアンマウントされるまで、コンポーネント内の `asyncData` は引き続き再取得されます。
::

## 型

```ts
refreshNuxtData(keys?: string | string[])
```

## パラメーター

* `keys`: データの取得に使用される `keys` として、単一の文字列または文字列の配列。このパラメーターは**オプション**です。`keys` が明示的に指定されていない場合、すべての [`useAsyncData`](/docs/api/composables/use-async-data) と [`useFetch`](/docs/api/composables/use-fetch) のキーが再取得されます。

## 戻り値

`refreshNuxtData` は Promise を返し、すべてまたは特定の `asyncData` インスタンスが更新されたときに解決されます。

## 例

### すべてのデータを更新

以下の例は、Nuxt アプリケーションで `useAsyncData` と `useFetch` を使用して取得されているすべてのデータを更新します。

```vue [pages/some-page.vue]
<script setup lang="ts">
const refreshing = ref(false)

async function refreshAll () {
  refreshing.value = true
  try {
    await refreshNuxtData()
  } finally {
    refreshing.value = false
  }
}
</script>

<template>
  <div>
    <button :disabled="refreshing" @click="refreshAll">
      すべてのデータを再取得
    </button>
  </div>
</template>
```

### 特定のデータを更新

以下の例は、キーが `count` と `user` に一致するデータのみを更新します。

```vue [pages/some-page.vue]
<script setup lang="ts">
const refreshing = ref(false)

async function refresh () {
  refreshing.value = true
  try {
    // 複数のデータを更新するために、キーの配列を渡すこともできます
    await refreshNuxtData(['count', 'user'])
  } finally {
    refreshing.value = false
  }
}
</script>

<template>
  <div v-if="refreshing">
    読み込み中
  </div>
  <button @click="refresh">更新</button>
</template>
```

::note
`asyncData` インスタンスにアクセスできる場合は、データを再取得する推奨方法として、その `refresh` または `execute` メソッドを使用することをお勧めします。
::

:read-more{to="/docs/getting-started/data-fetching"}
