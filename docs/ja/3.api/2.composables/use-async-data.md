---
title: 'useAsyncData'
description: useAsyncData は SSR に優しい composable で非同期に解決されるデータへのアクセスを提供します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/asyncData.ts
    size: xs
---

ページ、コンポーネント、プラグイン内で useAsyncData を使用して、非同期に解決されるデータにアクセスできます。

::note
[`useAsyncData`](/docs/api/composables/use-async-data) は [Nuxt コンテキスト](/docs/guide/going-further/nuxt-app#the-nuxt-context)内で直接呼び出すことを意図した composable です。リアクティブな composable を返し、Nuxt payload にレスポンスを追加する処理を行い、ページが hydrate される際に **クライアントサイドでデータを再取得することなく**サーバーからクライアントに渡すことができます。
::

## 使用方法

```vue [pages/index.vue]
<script setup lang="ts">
const { data, status, error, refresh, clear } = await useAsyncData(
  'mountains',
  () => $fetch('https://api.nuxtjs.dev/mountains')
)
</script>
```

::warning
カスタムの useAsyncData ラッパーを使用している場合は、composable 内でそれを await しないでください。予期しない動作を引き起こす可能性があります。カスタム非同期データフェッチャーの作り方についての詳細は、[このレシピ](/docs/guide/recipes/custom-usefetch#custom-usefetch)を参照してください。
::

::note
`data`、`status`、`error` は Vue refs であり、`<script setup>` 内で使用する際は `.value` でアクセスしてください。一方、`refresh`/`execute` と `clear` は通常の関数です。
::

### Watch パラメーター

組み込みの `watch` オプションは、変更が検出された時に fetcher 関数を自動的に再実行できます。

```vue [pages/index.vue]
<script setup lang="ts">
const page = ref(1)
const { data: posts } = await useAsyncData(
  'posts',
  () => $fetch('https://fakeApi.com/posts', {
    params: {
      page: page.value
    }
  }), {
    watch: [page]
  }
)
</script>
```

### リアクティブキー

キーとして computed ref、通常の ref、または getter 関数を使用でき、キーが変更された時に自動的に更新される動的なデータフェッチを行うことができます:

```vue [pages/[id\\].vue]
<script setup lang="ts">
const route = useRoute()
const userId = computed(() => `user-${route.params.id}`)

// ルートが変更されて userId が更新されると、データは自動的に再取得される
const { data: user } = useAsyncData(
  userId,
  () => fetchUserById(route.params.id)
)
</script>
```

::warning
[`useAsyncData`](/docs/api/composables/use-async-data) はコンパイラによって変換される予約関数名であるため、独自の関数に [`useAsyncData`](/docs/api/composables/use-async-data) という名前を付けるべきではありません。
::

:read-more{to="/docs/getting-started/data-fetching#useasyncdata"}

## パラメーター

- `key`: リクエスト間でデータフェッチが適切に重複排除されることを保証する一意のキー。キーを提供しない場合、`useAsyncData` のインスタンスのファイル名と行番号に固有のキーが生成されます。
- `handler`: truthy な値を返さなければならない非同期関数（例えば、`undefined` や `null` であってはいけません）。さもないとクライアントサイドでリクエストが重複する可能性があります。
::warning
`handler` 関数は SSR と CSR hydration 中の予測可能な動作を保証するために **副作用がない** べきです。副作用をトリガーする必要がある場合は、[`callOnce`](/docs/api/utils/call-once) ユーティリティを使用してください。
::
- `options`:
  - `server`: サーバーでデータをフェッチするかどうか（デフォルトは `true`）
  - `lazy`: クライアントサイドナビゲーションをブロックする代わりに、ルート読み込み後に非同期関数を解決するかどうか（デフォルトは `false`）
  - `immediate`: `false` に設定すると、リクエストが即座に発火されることを防ぐ。（デフォルトは `true`）
  - `default`: 非同期関数が解決される前に `data` のデフォルト値を設定するファクトリ関数 - `lazy: true` や `immediate: false` オプションで有用
  - `transform`: 解決後に `handler` 関数の結果を変更するために使用できる関数
  - `getCachedData`: キャッシュされたデータを返す関数を提供します。`null` や `undefined` の戻り値はフェッチをトリガーします。デフォルトでは:
    ```ts
    const getDefaultCachedData = (key, nuxtApp, ctx) => nuxtApp.isHydrating 
      ? nuxtApp.payload.data[key] 
      : nuxtApp.static.data[key]
    ```
    これは `nuxt.config` の `experimental.payloadExtraction` が有効な場合のみデータをキャッシュします。
  - `pick`: `handler` 関数の結果からこの配列で指定されたキーのみを選択
  - `watch`: 自動リフレッシュのためのリアクティブソースを監視
  - `deep`: データを deep ref オブジェクトで返す。デフォルトでは `false` で shallow ref オブジェクトでデータを返し、データが深いリアクティブである必要がない場合にパフォーマンスを向上させる。
  - `dedupe`: 同じキーを一度に一回以上フェッチすることを避ける（デフォルトは `cancel`）。可能なオプション:
    - `cancel` - 新しいリクエストが作成されたときに既存のリクエストをキャンセル
    - `defer` - 保留中のリクエストがある場合は新しいリクエストを一切作成しない

::note
内部的に、`lazy: false` は `<Suspense>` を使用してデータがフェッチされる前にルートの読み込みをブロックします。より軽快なユーザーエクスペリエンスのために、`lazy: true` を使用し、代わりにローディング状態を実装することを検討してください。
::

::read-more{to="/docs/api/composables/use-lazy-async-data"}
`useAsyncData` で `lazy: true` と同じ動作をする `useLazyAsyncData` を使用できます。
::

:video-accordion{title="Watch a video from Alexander Lichter about client-side caching with getCachedData" videoId="aQPR0xn-MMk"}

### 共有状態とオプションの一貫性

複数の `useAsyncData` 呼び出しで同じキーを使用する場合、同じ `data`、`error`、`status` refs を共有します。これはコンポーネント間の一貫性を保証しますが、オプションの一貫性が必要です。

以下のオプションは同じキーを持つすべての呼び出しで **一貫している必要があります**:
- `handler` 関数
- `deep` オプション
- `transform` 関数
- `pick` 配列
- `getCachedData` 関数
- `default` 値

以下のオプションは警告をトリガーすることなく **異なることができます**:
- `server`
- `lazy`
- `immediate`
- `dedupe`
- `watch`

```ts
// ❌ これは開発警告をトリガーします
const { data: users1 } = useAsyncData('users', () => $fetch('/api/users'), { deep: false })
const { data: users2 } = useAsyncData('users', () => $fetch('/api/users'), { deep: true })

// ✅ これは許可されています
const { data: users1 } = useAsyncData('users', () => $fetch('/api/users'), { immediate: true })
const { data: users2 } = useAsyncData('users', () => $fetch('/api/users'), { immediate: false })
```

## 戻り値

- `data`: 渡された非同期関数の結果。
- `refresh`/`execute`: `handler` 関数によって返されたデータをリフレッシュするために使用できる関数。
- `error`: データフェッチが失敗した場合のエラーオブジェクト。
- `status`: データリクエストのステータスを示す文字列:
  - `idle`: リクエストが開始されていない場合、例えば:
    - `execute` がまだ呼び出されておらず、`{ immediate: false }` が設定されている場合
    - サーバーで HTML をレンダリングしており、`{ server: false }` が設定されている場合
  - `pending`: リクエストが進行中
  - `success`: リクエストが正常に完了
  - `error`: リクエストが失敗
- `clear`: `data` を `undefined`（または提供されている場合 `options.default()` の値）に設定し、`error` を `undefined` に設定し、`status` を `idle` に設定し、現在保留中のリクエストをキャンセルとしてマークするために使用できる関数。

デフォルトでは、Nuxt は `refresh` が完了するまで待ってから再度実行されます。

::note
サーバーでデータをフェッチしていない場合（例えば `server: false` ）、hydration が完了するまでデータはフェッチ _されません_。つまり、クライアントサイドで [`useAsyncData`](/docs/api/composables/use-async-data) を await しても、`data` は `<script setup>` 内で `undefined` のままです。
::

## 型

```ts [Signature]
function useAsyncData<DataT, DataE>(
  handler: (nuxtApp?: NuxtApp) => Promise<DataT>,
  options?: AsyncDataOptions<DataT>
): AsyncData<DataT, DataE>
function useAsyncData<DataT, DataE>(
  key: MaybeRefOrGetter<string>,
  handler: (nuxtApp?: NuxtApp) => Promise<DataT>,
  options?: AsyncDataOptions<DataT>
): Promise<AsyncData<DataT, DataE>>

type AsyncDataOptions<DataT> = {
  server?: boolean
  lazy?: boolean
  immediate?: boolean
  deep?: boolean
  dedupe?: 'cancel' | 'defer'
  default?: () => DataT | Ref<DataT> | null
  transform?: (input: DataT) => DataT | Promise<DataT>
  pick?: string[]
  watch?: MultiWatchSources | false
  getCachedData?: (key: string, nuxtApp: NuxtApp, ctx: AsyncDataRequestContext) => DataT | undefined
}

type AsyncDataRequestContext = {
  /** このデータリクエストの理由 */
  cause: 'initial' | 'refresh:manual' | 'refresh:hook' | 'watch'
}

type AsyncData<DataT, ErrorT> = {
  data: Ref<DataT | undefined>
  refresh: (opts?: AsyncDataExecuteOptions) => Promise<void>
  execute: (opts?: AsyncDataExecuteOptions) => Promise<void>
  clear: () => void
  error: Ref<ErrorT | undefined>
  status: Ref<AsyncDataRequestStatus>
};

interface AsyncDataExecuteOptions {
  dedupe?: 'cancel' | 'defer'
}

type AsyncDataRequestStatus = 'idle' | 'pending' | 'success' | 'error'
```

:read-more{to="/docs/getting-started/data-fetching"}
