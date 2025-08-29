---
title: 'useFetch'
description: 'SSR に優しい composable で API エンドポイントからデータをフェッチします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/fetch.ts
    size: xs
---

この composable は [`useAsyncData`](/docs/api/composables/use-async-data) と [`$fetch`](/docs/api/utils/dollarfetch) の便利なラッパーを提供します。
URL とフェッチオプションに基づいてキーを自動生成し、サーバールートに基づいてリクエスト URL の型ヒントを提供し、API レスポンスタイプを推論します。

::note
`useFetch` は setup 関数、プラグイン、またはルートミドルウェア内で直接呼び出すことを意図した composable です。リアクティブな composables を返し、Nuxt payload へのレスポンスの追加を処理して、ページが hydrate される際にクライアントサイドでデータを再フェッチすることなくサーバーからクライアントに渡すことができます。
::

## 使用方法

```vue [pages/modules.vue]
<script setup lang="ts">
const { data, status, error, refresh, clear } = await useFetch('/api/modules', {
  pick: ['title']
})
</script>
```

::warning
カスタムの useFetch ラッパーを使用している場合は、composable 内でそれを await しないでください。予期しない動作を引き起こす可能性があります。カスタム非同期データフェッチャーの作り方についての詳細は、[このレシピ](/docs/guide/recipes/custom-usefetch#custom-usefetch)を参照してください。
::

::note
`data`、`status`、`error` は Vue refs であり、`<script setup>` 内で使用する際は `.value` でアクセスしてください。一方、`refresh`/`execute` と `clear` は通常の関数です。
::

`query` オプションを使用して、クエリに検索パラメーターを追加できます。このオプションは [unjs/ofetch](https://github.com/unjs/ofetch) から拡張され、[unjs/ufo](https://github.com/unjs/ufo) を使用して URL を作成します。オブジェクトは自動的に文字列化されます。

```ts
const param1 = ref('value1')
const { data, status, error, refresh } = await useFetch('/api/modules', {
  query: { param1, param2: 'value2' }
})
```

上記の例では `https://api.nuxt.com/modules?param1=value1&param2=value2` になります。

[インターセプター](https://github.com/unjs/ofetch#%EF%B8%8F-interceptors)も使用できます:

```ts
const { data, status, error, refresh, clear } = await useFetch('/api/auth/login', {
  onRequest({ request, options }) {
    // リクエストヘッダーを設定
    // これは ofetch >= 1.4.0 に依存します - lockfile の更新が必要な場合があります
    options.headers.set('Authorization', '...')
  },
  onRequestError({ request, options, error }) {
    // リクエストエラーを処理
  },
  onResponse({ request, response, options }) {
    // レスポンスデータを処理
    localStorage.setItem('token', response._data.token)
  },
  onResponseError({ request, response, options }) {
    // レスポンスエラーを処理
  }
})
```

### リアクティブキーと共有状態

URL として computed ref または通常の ref を使用でき、URL が変更されたときに自動的に更新される動的なデータフェッチが可能です:

```vue [pages/[id\\].vue]
<script setup lang="ts">
const route = useRoute()
const id = computed(() => route.params.id)

// ルートが変更されて id が更新されると、データは自動的に再フェッチされます
const { data: post } = await useFetch(() => `/api/posts/${id.value}`)
</script>
```

複数のコンポーネントで同じ URL とオプションで `useFetch` を使用する場合、同じ `data`、`error`、`status` refs を共有します。これによりコンポーネント間の一貫性が保証されます。

::warning
`useFetch` はコンパイラによって変換される予約関数名であるため、独自の関数に `useFetch` という名前を付けるべきではありません。
::

::warning
If you encounter the `data` variable destructured from a `useFetch` returns a string and not a JSON parsed object then make sure your component doesn't include an import statement like `import { useFetch } from '@vueuse/core`.
::

:video-accordion{title="Watch the video from Alexander Lichter to avoid using useFetch the wrong way" videoId="njsGVmcWviY"}

:read-more{to="/docs/getting-started/data-fetching"}

## 型

```ts [Signature]
function useFetch<DataT, ErrorT>(
  url: string | Request | Ref<string | Request> | (() => string | Request),
  options?: UseFetchOptions<DataT>
): Promise<AsyncData<DataT, ErrorT>>

type UseFetchOptions<DataT> = {
  key?: MaybeRefOrGetter<string>
  method?: string
  query?: SearchParams
  params?: SearchParams
  body?: RequestInit['body'] | Record<string, any>
  headers?: Record<string, string> | [key: string, value: string][] | Headers
  baseURL?: string
  server?: boolean
  lazy?: boolean
  immediate?: boolean
  getCachedData?: (key: string, nuxtApp: NuxtApp, ctx: AsyncDataRequestContext) => DataT | undefined
  deep?: boolean
  dedupe?: 'cancel' | 'defer'
  default?: () => DataT
  transform?: (input: DataT) => DataT | Promise<DataT>
  pick?: string[]
  $fetch?: typeof globalThis.$fetch
  watch?: MultiWatchSources | false
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
}

interface AsyncDataExecuteOptions {
  dedupe?: 'cancel' | 'defer'
}

type AsyncDataRequestStatus = 'idle' | 'pending' | 'success' | 'error'
```

## パラメーター

- `URL` (`string | Request | Ref<string | Request> | () => string | Request`): フェッチする URL またはリクエスト。文字列、Request オブジェクト、Vue ref、または文字列/Request を返す関数であることができます。動的エンドポイントのためのリアクティビティをサポートします。

- `options` (object): フェッチリクエストの設定。[unjs/ofetch](https://github.com/unjs/ofetch) オプションと [`AsyncDataOptions`](/docs/api/composables/use-async-data#params) を拡張します。すべてのオプションは静的値、`ref`、または computed 値にすることができます。

| オプション | 型 | デフォルト | 説明 |
| ---| --- | --- | --- |
| `key` | `MaybeRefOrGetter<string>` | auto-gen | 重複排除用の一意キー。提供されない場合、URL とオプションから生成されます。 |
| `method` | `string` | `'GET'` | HTTP リクエストメソッド。 |
| `query` | `object` | - | URL に追加するクエリ/検索パラメーター。エイリアス: `params`。refs/computed をサポート。 |
| `params` | `object` | - | `query` のエイリアス。 |
| `body` | `RequestInit['body'] \| Record<string, any>` | - | リクエストボディ。オブジェクトは自動的に文字列化されます。refs/computed をサポート。 |
| `headers` | `Record<string, string> \| [key, value][] \| Headers` | - | リクエストヘッダー。 |
| `baseURL` | `string` | - | リクエストのベース URL。 |
| `timeout` | `number` | - | リクエストを中止するまでのタイムアウト（ミリ秒）。 |
| `cache` | `boolean \| string` | - | キャッシュ制御。Boolean でキャッシュを無効化、または Fetch API の値を使用: `default`、`no-store` など。 |
| `server` | `boolean` | `true` | サーバーでフェッチするかどうか。 |
| `lazy` | `boolean` | `false` | true の場合、ルート読み込み後に解決します（ナビゲーションをブロックしません）。 |
| `immediate` | `boolean` | `true` | false の場合、リクエストが即座に発火されることを防います。 |
| `default` | `() => DataT` | - | 非同期解決前の `data` のデフォルト値のファクトリ。 |
| `transform` | `(input: DataT) => DataT \| Promise<DataT>` | - | 解決後に結果を変換する関数。 |
| `getCachedData`| `(key, nuxtApp, ctx) => DataT \| undefined` | - | キャッシュされたデータを返す関数。デフォルトは以下を参照。 |
| `pick` | `string[]` | - | 結果から指定されたキーのみを選択。 |
| `watch` | `MultiWatchSources \| false` | - | 監視して自動リフレッシュするリアクティブソースの配列。`false` で監視を無効化。 |
| `deep` | `boolean` | `false` | データを deep ref オブジェクトで返す。 |
| `dedupe` | `'cancel' \| 'defer'` | `'cancel'` | 同じキーを一度に一回以上フェッチすることを避ける。 |
| `$fetch` | `typeof globalThis.$fetch` | - | カスタム $fetch 実装。 |

::note
すべてのフェッチオプションに `computed` または `ref` 値を渡すことができます。これらは監視され、更新された場合は新しい値で自動的に新しいリクエストが作成されます。
::

**getCachedData デフォルト:**

```ts
const getDefaultCachedData = (key, nuxtApp, ctx) => nuxtApp.isHydrating 
 ? nuxtApp.payload.data[key] 
 : nuxtApp.static.data[key]
```
これは `nuxt.config` の `experimental.payloadExtraction` が有効な場合のみデータをキャッシュします。

## 戻り値

| 名前 | 型 | 説明 |
| --- | --- |--- |
| `data` | `Ref<DataT \| undefined>` | 非同期フェッチの結果。 |
| `refresh` | `(opts?: AsyncDataExecuteOptions) => Promise<void>` | データを手動でリフレッシュする関数。デフォルトでは、Nuxt は `refresh` が完了するまで待ってから再度実行されます。 |
| `execute` | `(opts?: AsyncDataExecuteOptions) => Promise<void>` | `refresh` のエイリアス。 |
| `error` | `Ref<ErrorT \| undefined>` | データフェッチが失敗した場合のエラーオブジェクト。 |
| `status` | `Ref<'idle' \| 'pending' \| 'success' \| 'error'>` | データリクエストのステータス。可能な値は以下を参照。 |
| `clear` | `() => void` | `data` を `undefined`（または提供されている場合 `options.default()` の値）にリセットし、`error` を `undefined` に設定し、`status` を `idle` に設定し、保留中のリクエストをキャンセルします。 |

### ステータス値

- `idle`: リクエストが開始されていない（例: `{ immediate: false }` またはサーバーレンダリングで `{ server: false }`）
- `pending`: リクエストが進行中
- `success`: リクエストが正常に完了
- `error`: リクエストが失敗

::note
サーバーでデータをフェッチしていない場合（例えば `server: false`）、hydration が完了するまでデータはフェッチ _されません_。つまり、クライアントサイドで `useFetch` を await しても、`data` は `<script setup>` 内で null のままです。
::

### 例

:link-example{to="/docs/examples/advanced/use-custom-fetch-composable"}

:link-example{to="/docs/examples/features/data-fetching"}
