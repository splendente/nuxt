---
title: 'definePageMeta'
description: 'ページコンポーネントのメタデータを定義します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/pages/runtime/composables.ts
    size: xs
---

`definePageMeta` は、[`pages/`](/docs/guide/directory-structure/pages) ディレクトリに配置された **ページ** コンポーネントにメタデータを設定するために使用できるコンパイラーマクロです（[別途設定](/docs/api/nuxt-config#pages)しない限り）。この方法で、Nuxt アプリケーションの各静的または動的ルートにカスタムメタデータを設定できます。

```vue [pages/some-page.vue]
<script setup lang="ts">
definePageMeta({
  layout: 'default'
})
</script>
```

:read-more{to="/docs/guide/directory-structure/pages#page-metadata"}

## 型

```ts
definePageMeta(meta: PageMeta) => void

interface PageMeta {
  validate?: (route: RouteLocationNormalized) => boolean | Promise<boolean> | Partial<NuxtError> | Promise<Partial<NuxtError>>
  redirect?: RouteRecordRedirectOption
  name?: string
  path?: string
  props?: RouteRecordRaw['props']
  alias?: string | string[]
  pageTransition?: boolean | TransitionProps
  layoutTransition?: boolean | TransitionProps
  viewTransition?: boolean | 'always'
  key?: false | string | ((route: RouteLocationNormalizedLoaded) => string)
  keepalive?: boolean | KeepAliveProps
  layout?: false | LayoutKey | Ref<LayoutKey> | ComputedRef<LayoutKey>
  middleware?: MiddlewareKey | NavigationGuard | Array<MiddlewareKey | NavigationGuard>
  scrollToTop?: boolean | ((to: RouteLocationNormalizedLoaded, from: RouteLocationNormalizedLoaded) => boolean)
  [key: string]: unknown
}
```

## パラメーター

### `meta`

- **Type**: `PageMeta`

  以下のページメタデータを受け入れるオブジェクト:

  **`name`**

  - **Type**: `string`

    このページのルートの名前を定義できます。デフォルトでは、[`pages/` ディレクトリ](/docs/guide/directory-structure/pages)内のパスに基づいて名前が生成されます。

  **`path`**

  - **Type**: `string`

    ファイル名で表現できるよりも複雑なパターンがある場合は、[カスタム正規表現](#using-a-custom-regular-expression)を定義できます。

  **`props`**
  
  - **Type**: [`RouteRecordRaw['props']`](https://router.vuejs.org/guide/essentials/passing-props)

    ルートの `params` をページコンポーネントに渡される props としてアテセスできるようにします。

  **`alias`**

  - **Type**: `string | string[]`

    レコードのエイリアス。レコードのコピーのように動作する追加パスを定義できます。`/users/:id` や `/u/:id` などのパスの短縮形を持つことができます。すべての `alias` と `path` の値は同じ params を共有する必要があります。

  **`keepalive`**

  - **Type**: `boolean` | [`KeepAliveProps`](https://vuejs.org/api/built-in-components.html#keepalive)

    ルート変更間でページ状態を保持したい場合は `true` に設定するか、きめ細かい制御のために [`KeepAliveProps`](https://vuejs.org/api/built-in-components.html#keepalive) を使用します。

  **`key`**

  - **Type**: `false` | `string` | `((route: RouteLocationNormalizedLoaded) => string)`

    `<NuxtPage>` コンポーネントが再レンダリングされるタイミングをより細かく制御したい場合は `key` 値を設定します。

  **`layout`**

  - **Type**: `false` | `LayoutKey` | `Ref<LayoutKey>` | `ComputedRef<LayoutKey>`

    各ルートのレイアウトの静的または動的な名前を設定します。デフォルトレイアウトを無効にする必要がある場合は `false` に設定できます。

  **`layoutTransition`**

  - **Type**: `boolean` | [`TransitionProps`](https://vuejs.org/api/built-in-components.html#transition)

    現在のレイアウトに適用するトランジションの名前を設定します。レイアウトトランジションを無効にするために、この値を `false` に設定することもできます。

  **`middleware`**

  - **Type**: `MiddlewareKey` | [`NavigationGuard`](https://router.vuejs.org/api/interfaces/NavigationGuard.html#navigationguard) | `Array<MiddlewareKey | NavigationGuard>`

    `definePageMeta` 内で直接匿名または名前付きミドルウェアを定義します。[ルートミドルウェア](/docs/guide/directory-structure/middleware)について詳しく学んでください。

  **`pageTransition`**

  - **Type**: `boolean` | [`TransitionProps`](https://vuejs.org/api/built-in-components.html#transition)

    現在のページに適用するトランジションの名前を設定します。ページトランジションを無効にするために、この値を `false` に設定することもできます。

  **`viewTransition`**

  - **Type**: `boolean | 'always'`

    **実験的機能、[nuxt.config ファイルで有効化](/docs/getting-started/transitions#view-transitions-api-experimental)した場合のみ利用可能**</br>
    現在のページの View Transitions を有効/無効にします。
    true に設定した場合、ユーザーのブラウザーが `prefers-reduced-motion: reduce` にマッチした場合、Nuxt はトランジションを適用しません（推奨）。`always` に設定した場合、Nuxt は常にトランジションを適用します。

  **`redirect`**

  - **Type**: [`RouteRecordRedirectOption`](https://router.vuejs.org/guide/essentials/redirect-and-alias.html#redirect-and-alias)

    ルートが直接マッチした場合のリダイレクト先。リダイレクトはナビゲーションガードの前に発生し、新しいターゲットロケーションで新しいナビゲーションをトリガーします。

  **`validate`**

  - **Type**: `(route: RouteLocationNormalized) => boolean | Promise<boolean> | Partial<NuxtError> | Promise<Partial<NuxtError>>`

    指定されたルートがこのページで有効にレンダリングできるかどうかを検証します。有効な場合は true、そうでない場合は false を返します。他のマッチが見つからない場合は 404 を意味します。`statusCode`/`statusMessage` を持つオブジェクトを直接返して、エラーで即座に応答することもできます（他のマッチはチェックされません）。

  **`scrollToTop`**

  - **Type**: `boolean | (to: RouteLocationNormalized, from: RouteLocationNormalized) => boolean`

    ページをレンダリングする前にトップにスクロールするかどうかを Nuxt に指示します。Nuxt のデフォルトスクロール動作を上書きしたい場合は、`~/router.options.ts` で行うことができます（詳細は [カスタムルーティング](/docs/guide/recipes/custom-routing#using-approuteroptions) を参照）。

  **`[key: string]`**

  - **Type**: `any`

    上記のプロパティ以外に、**カスタム** メタデータを設定することもできます。[`meta` オブジェクトの型を拡張](/docs/guide/directory-structure/pages/#typing-custom-metadata)して型安全な方法で行うことをお勧めします。

## 例

### 基本的な使用方法

以下の例は次のことを示しています:

- `key` が値を返す関数にできる方法
- `keepalive` プロパティが複数のコンポーネント間で切り替えるときに `<modal>` コンポーネントがキャッシュされないことを保証する方法
- カスタムプロパティとして `pageType` を追加する方法:

```vue [pages/some-page.vue]
<script setup lang="ts">
definePageMeta({
  key: (route) => route.fullPath,

  keepalive: {
    exclude: ['modal']
  },

  pageType: 'Checkout'
})
</script>
```

### ミドルウェアの定義

以下の例は、`definePageMeta` 内で直接 `function` を使用してミドルウェアを定義する方法、または `middleware/` ディレクトリにあるミドルウェアファイル名と一致する `string` として設定する方法を示しています:

```vue [pages/some-page.vue]
<script setup lang="ts">
definePageMeta({
  // ミドルウェアを関数として定義
  middleware: [
    function (to, from) {
      const auth = useState('auth')

      if (!auth.value.authenticated) {
          return navigateTo('/login')
      }

      if (to.path !== '/checkout') {
        return navigateTo('/checkout')
      }
    }
  ],

  // ... または文字列
  middleware: 'auth'

  // ... または複数の文字列
  middleware: ['auth', 'another-named-middleware']
})
</script>
```

### カスタム正規表現の使用

カスタム正規表現は、重複するルート間の競合を解決する良い方法です。例えば:

2つのルート「/test-category」と「/1234-post」が `[postId]-[postSlug].vue` と `[categorySlug].vue` の両方のページルートにマッチします。

`[postId]-[postSlug]` ルートで `postId` に数字のみ（`\d+`）をマッチさせることを確実にするために、`[postId]-[postSlug].vue` ページテンプレートに以下を追加できます:

```vue [pages/[postId\\]-[postSlug\\].vue]
<script setup lang="ts">
definePageMeta({
  path: '/:postId(\\d+)-:postSlug' 
})
</script>
```

さらなる例は [Vue Router のマッチング構文](https://router.vuejs.org/guide/essentials/route-matching-syntax.html) を参照してください。

### レイアウトの定義

（デフォルトで）[`layouts/` ディレクトリ](/docs/guide/directory-structure/layouts)にあるレイアウトのファイル名と一致するレイアウトを定義できます。`layout` を `false` に設定してレイアウトを無効にすることもできます:

```vue [pages/some-page.vue]
<script setup lang="ts">
definePageMeta({
  // カスタムレイアウトを設定
  layout: 'admin'

  // ... またはデフォルトレイアウトを無効化
  layout: false
})
</script>
```
