---
title: "navigateTo"
description: navigateTo は、ユーザーをプログラマティックにナビゲートするヘルパー関数です。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/router.ts
    size: xs
---

## 使用方法

`navigateTo` は、サーバーサイドとクライアントサイドの両方で利用できます。[Nuxt context](/docs/guide/going-further/nuxt-app#the-nuxt-context) 内で、または直接使用してページナビゲーションを実行できます。

::warning
`navigateTo` を呼び出すときは、必ず結果に `await` または `return` を使用してください。
::

::note
`navigateTo` は Nitro ルート内では使用できません。Nitro ルートでサーバーサイドリダイレクトを実行するには、代わりに [`sendRedirect`](https://h3.dev/utils/response#sendredirectevent-location-code) を使用してください。
::

### Vue コンポーネント内

```vue
<script setup lang="ts">
// 'to' を文字列として渡す
await navigateTo('/search')

// ... またはルートオブジェクトとして
await navigateTo({ path: '/search' })

// ... またはクエリパラメーターを含むルートオブジェクトとして
await navigateTo({
  path: '/search',
  query: {
    page: 1,
    sort: 'asc'
  }
})
</script>
```

### ルートミドルウェア内

```ts
export default defineNuxtRouteMiddleware((to, from) => {
  if (to.path !== '/search') {
    // リダイレクトコードを '301 Moved Permanently' に設定
    return navigateTo('/search', { redirectCode: 301 })
  }
})
```

ルートミドルウェア内で `navigateTo` を使用する際は、ミドルウェアの実行フローが正常に動作するよう、**その結果を return する**必要があります。

例えば、以下の実装は**期待どおりに動作しません**:

```ts
export default defineNuxtRouteMiddleware((to, from) => {
  if (to.path !== '/search') {
    // ❌ これは期待どおりに動作しません
    navigateTo('/search', { redirectCode: 301 })
    return
  }
})
```

この場合、`navigateTo` は実行されますが return されないため、予期しない動作を引き起こす可能性があります。

:read-more{to="/docs/guide/directory-structure/middleware"}

### 外部 URL へのナビゲーション

`navigateTo` の `external` パラメーターは、URL へのナビゲーションがどのように処理されるかに影響します:

- **`external: true` なし**:
  - 内部 URL は期待どおりにナビゲーションされます。
  - 外部 URL はエラーをスローします。

- **`external: true` あり**:
  - 内部 URL はフルページリロードでナビゲーションされます。
  - 外部 URL は期待どおりにナビゲーションされます。

#### 例

```vue
<script setup lang="ts">
// エラーがスローされます;
// 外部 URL へのナビゲーションはデフォルトでは許可されていません
await navigateTo('https://nuxt.com')

// 'external' パラメーターを 'true' に設定すると正常にリダイレクトされます
await navigateTo('https://nuxt.com', {
  external: true
})
</script>
```

### 新しいタブでページを開く

```vue
<script setup lang="ts">
// 'https://nuxt.com' を新しいタブで開きます
await navigateTo('https://nuxt.com', {
  open: {
    target: '_blank',
    windowFeatures: {
      width: 500,
      height: 500
    }
  }
})
</script>
```

## 型

```ts
function navigateTo(
  to: RouteLocationRaw | undefined | null,
  options?: NavigateToOptions
) => Promise<void | NavigationFailure | false> | false | void | RouteLocationRaw 

interface NavigateToOptions {
  replace?: boolean
  redirectCode?: number
  external?: boolean
  open?: OpenOptions
}

type OpenOptions = {
  target: string
  windowFeatures?: OpenWindowFeatures
}

type OpenWindowFeatures = {
  popup?: boolean
  noopener?: boolean
  noreferrer?: boolean
} & XOR<{ width?: number }, { innerWidth?: number }>
  & XOR<{ height?: number }, { innerHeight?: number }>
  & XOR<{ left?: number }, { screenX?: number }>
  & XOR<{ top?: number }, { screenY?: number }>
```

## パラメーター

### `to`

**型**: [`RouteLocationRaw`](https://router.vuejs.org/api/interfaces/RouteLocationOptions.html#Interface-RouteLocationOptions) | `undefined` | `null`

**デフォルト**: `'/'`

`to` は、リダイレクト先のプレーンな文字列またはルートオブジェクトです。`undefined` または `null` として渡された場合、デフォルトで `'/'` になります。

#### 例

```ts
// URL を直接渡すと '/blog' ページにリダイレクトされます
await navigateTo('/blog')

// ルートオブジェクトを使用すると、'blog' という名前のルートにリダイレクトされます
await navigateTo({ name: 'blog' })

// ルートオブジェクトを使用してパラメーター (id = 1) を渡しながら、'product' ルートにリダイレクトします。
await navigateTo({ name: 'product', params: { id: 1 } })
```

### `options` (オプション)

**型**: `NavigateToOptions`

以下のプロパティを受け取るオブジェクト:

- `replace`

  - **型**: `boolean`
  - **デフォルト**: `false`
  - デフォルトでは、`navigateTo` はクライアントサイドで指定されたルートを Vue Router のインスタンスにプッシュします。

    この動作は `replace` を `true` に設定することで変更でき、指定されたルートが置き換えられることを示します。

- `redirectCode`

  - **型**: `number`
  - **デフォルト**: `302`

  - `navigateTo` は指定されたパスにリダイレクトし、サーバーサイドでリダイレクションが発生するときにデフォルトでリダイレクトコードを [`302 Found`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302) に設定します。

    このデフォルトの動作は、異なる `redirectCode` を提供することで変更できます。一般的に、永続的なリダイレクションには [`301 Moved Permanently`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/301) を使用できます。

- `external`

  - **型**: `boolean`
  - **デフォルト**: `false`

  - `true` に設定すると外部 URL へのナビゲーションが許可されます。そうでなければ、外部ナビゲーションはデフォルトで許可されていないため、`navigateTo` はエラーをスローします。

- `open`

  - **型**: `OpenOptions`
  - ウィンドウの [open()](https://developer.mozilla.org/en-US/docs/Web/API/Window/open) メソッドを使用して URL にナビゲーションすることを許可します。このオプションはクライアントサイドでのみ適用され、サーバーサイドでは無視されます。

    以下のプロパティを受け取るオブジェクト:

  - `target`

    - **型**: `string`
    - **デフォルト**: `'_blank'`

    - リソースが読み込まれるブラウジングコンテキストの名前を指定する、空白を含まない文字列。

  - `windowFeatures`

    - **型**: `OpenWindowFeatures`

    - 以下のプロパティを受け取るオブジェクト:

      | プロパティ | 型    | 説明 |
      |----------|---------|--------------|
      | `popup`  | `boolean` | 新しいタブの代わりに、ブラウザーが決定する UI 機能を持つ最小限のポップアップウィンドウを要求します。 |
      | `width` または `innerWidth`  | `number`  | スクロールバーを含むコンテンツエリアの幅を指定します（最小 100 ピクセル）。 |
      | `height` または `innerHeight` | `number`  | スクロールバーを含むコンテンツエリアの高さを指定します（最小 100 ピクセル）。 |
      | `left` または `screenX`   | `number`  | 画面の左端を基準とした新しいウィンドウの水平位置を設定します。 |
      | `top` または `screenY`   | `number`  | 画面の上端を基準とした新しいウィンドウの垂直位置を設定します。 |
      | `noopener` | `boolean` | 新しいウィンドウが `window.opener` を介して元のウィンドウにアクセスすることを防ぎます。 |
      | `noreferrer` | `boolean` | Referer ヘッダーの送信を防ぎ、暗黙的に `noopener` を有効にします。 |

      **windowFeatures** プロパティの詳細については、[ドキュメント](https://developer.mozilla.org/en-US/docs/Web/API/Window/open#windowfeatures)を参照してください。
