---
title: 'useNuxtData'
description: 'データフェッチ composable の現在キャッシュされている値にアクセスします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

::note
`useNuxtData` は、明示的に提供されたキーを使用して [`useAsyncData`](/docs/api/composables/use-async-data) 、[`useLazyAsyncData`](/docs/api/composables/use-lazy-async-data) 、[`useFetch`](/docs/api/composables/use-fetch) 、[`useLazyFetch`](/docs/api/composables/use-lazy-fetch) の現在キャッシュされている値へのアクセスを提供します。
::

## 使用方法

`useNuxtData` composable は、`useAsyncData`、`useLazyAsyncData`、`useFetch`、`useLazyFetch` などのデータフェッチ composable の現在キャッシュされている値にアクセスするために使用されます。データフェッチ中に使用されたキーを提供することで、キャッシュされたデータを取得し、必要に応じて使用できます。

これは、すでにフェッチされたデータを再利用してパフォーマンスを最適化したり、楽観的更新やカスケードデータ更新などの機能を実装したりするのに特に有用です。

`useNuxtData` を使用するには、データフェッチ composable（`useFetch`、`useAsyncData` など）が明示的に提供されたキーで呼び出されていることを確認してください。

:video-accordion{title="Watch a video from LearnVue about useNuxtData" videoId="e-_u6swXRWk"}

## パラメーター

- `key`: キャッシュされたデータを識別する一意のキー。このキーは、元のデータフェッチ時に使用されたものと一致する必要があります。

## 戻り値

- `data`: 提供されたキーに関連付けられたキャッシュされたデータへのリアクティブな参照。キャッシュされたデータが存在しない場合、値は `null` になります。この `Ref` はキャッシュされたデータが変更された場合に自動的に更新され、コンポーネントでシームレスなリアクティビティを可能にします。

## 例

以下の例では、最新のデータがサーバーからフェッチされている間に、キャッシュされたデータをプレースホルダーとして使用する方法を示しています。

```vue [pages/posts.vue]
<script setup lang="ts">
// 後で 'posts' キーを使用して同じデータにアクセスできます
const { data } = await useFetch('/api/posts', { key: 'posts' })
</script>
```

```vue [pages/posts/[id\\].vue]
<script setup lang="ts">
// posts.vue（親ルート）の useFetch のキャッシュされた値にアクセス
const { data: posts } = useNuxtData('posts')

const route = useRoute()

const { data } = useLazyFetch(`/api/posts/${route.params.id}`, {
  key: `post-${route.params.id}`,
  default() {
    // キャッシュから個々の投稿を見つけて、それをデフォルト値として設定します。
    return posts.value.find(post => post.id === route.params.id)
  }
})
</script>
```

## 楽観的更新

以下の例では、useNuxtData を使用して楽観的更新を実装する方法を示しています。

楽観的更新は、サーバー操作が成功すると仮定してユーザーインターフェースを即座に更新する技術です。操作が最終的に失敗した場合、UI は以前の状態にロールバックされます。

```vue [pages/todos.vue]
<script setup lang="ts">
// 後で 'todos' キーを使用して同じデータにアクセスできます
const { data } = await useAsyncData('todos', () => $fetch('/api/todos'))
</script>
```

```vue [components/NewTodo.vue]
<script setup lang="ts">
const newTodo = ref('')
let previousTodos = []

// todos.vue の useAsyncData のキャッシュされた値にアクセス
const { data: todos } = useNuxtData('todos')

async function addTodo () {
  return $fetch('/api/addTodo', {
    method: 'post',
    body: {
      todo: newTodo.value
    },
    onRequest () {
      // フェッチが失敗した場合に復元するため、以前にキャッシュされた値を保存します。
      previousTodos = todos.value

      // todos を楽観的に更新します。
      todos.value = [...todos.value, newTodo.value]
    },
    onResponseError () {
      // リクエストが失敗した場合、データをロールバックします。
      todos.value = previousTodos
    },
    async onResponse () {
      // リクエストが成功した場合、バックグラウンドで todos を無効化します。
      await refreshNuxtData('todos')
    }
  })
}
</script>
```

## Type

```ts
useNuxtData<DataT = any> (key: string): { data: Ref<DataT | undefined> }
```
