---
title: 'useCookie'
description: useCookie はクッキーを読み書きする SSR に優しい composable です。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/cookie.ts
    size: xs
---

## 使用方法

ページ、コンポーネント、プラグイン内で `useCookie` を使用して、SSR に優しい方法でクッキーを読み書きできます。

```ts
const cookie = useCookie(name, options)
```

::note
`useCookie` は [Nuxt コンテキスト](/docs/guide/going-further/nuxt-app#the-nuxt-context)内でのみ動作します。
::

::tip
返される ref はクッキー値を自動的に JSON にシリアライズおよびデシリアライズします。
::

## 型

```ts [Signature]
import type { Ref } from 'vue'
import type { CookieParseOptions, CookieSerializeOptions } from 'cookie-es'

export interface CookieOptions<T = any> extends Omit<CookieSerializeOptions & CookieParseOptions, 'decode' | 'encode'> {
  decode?(value: string): T
  encode?(value: T): string
  default?: () => T | Ref<T>
  watch?: boolean | 'shallow'
  readonly?: boolean
}

export interface CookieRef<T> extends Ref<T> {}

export function useCookie<T = string | null | undefined>(
  name: string,
  options?: CookieOptions<T>
): CookieRef<T>
```

## パラメーター

`name`: クッキーの名前。

`options`: クッキーの動作を制御するオプション。オブジェクトは以下のプロパティを持つことができます:

ほとんどのオプションは [cookie](https://github.com/jshttp/cookie) パッケージに直接渡されます。

| プロパティ | 型 | デフォルト | 説明 |
| --- | --- | --- | --- |
| `decode` | `(value: string) => T` | `decodeURIComponent` + [destr](https://github.com/unjs/destr). | クッキー値をデコードするカスタム関数。クッキーの値は文字セットが限定されている（また単純な文字列でなければならない）ため、この関数を使用して事前にエンコードされたクッキー値を JavaScript 文字列やその他のオブジェクトにデコードできます。<br/> **注意:** この関数からエラーがスローされた場合、元のデコードされていないクッキー値がクッキーの値として返されます。 |
| `encode` | `(value: T) => string` | `JSON.stringify` + `encodeURIComponent` | クッキー値をエンコードするカスタム関数。クッキーの値は文字セットが限定されている（また単純な文字列でなければならない）ため、この関数を使用して値をクッキーの値に適した文字列にエンコードできます。 |
| `default` | `() => T \| Ref<T>` | `undefined` | クッキーが存在しない場合のデフォルト値を返す関数。関数は `Ref` も返すことができます。 |
| `watch` | `boolean \| 'shallow'` | `true`  | 変更を監視してクッキーを更新するかどうか。`true` は deep watch、`'shallow'` は shallow watch（つまりトップレベルプロパティのみのデータ変更）、`false` は無効。<br/> **注意:** クッキーが [`refreshCookie`](/docs/api/utils/refresh-cookie) で変更された場合は、`useCookie` の値を手動でリフレッシュしてください。 |
| `readonly` | `boolean` | `false` | `true` の場合、クッキーへの書き込みを無効にします。 |
| `maxAge` | `number` | `undefined` | クッキーの最大継続時間（秒）、つまり [`Max-Age` `Set-Cookie` 属性](https://tools.ietf.org/html/rfc6265#section-5.2.2)の値。指定された数値は切り捨てによって整数に変換されます。デフォルトでは最大継続時間は設定されません。 |
| `expires` | `Date` | `undefined` | クッキーの有効期限。デフォルトでは有効期限は設定されません。ほとんどのクライアントはこれを「非永続クッキー」とみなし、Web ブラウザアプリケーションの終了などの条件で削除します。<br/> **注意:** [クッキーストレージモデル仕様](https://tools.ietf.org/html/rfc6265#section-5.3)では、`expires` と `maxAge` の両方が設定されている場合、`maxAge` が優先されるとされていますが、すべてのクライアントがこれを遵守しない可能性があるため、両方が設定されている場合は同じ日時を指す必要があります！<br/>`expires` と `maxAge` のどちらも設定されていない場合、クッキーはセッションのみで、ユーザーがブラウザを閉じたときに削除されます。 |
| `httpOnly` | `boolean` | `false` | HttpOnly 属性を設定します。<br/> **注意:** これを `true` に設定する際は注意してください。互換性のあるクライアントでは、クライアントサイドの JavaScript が `document.cookie` でクッキーを見ることを許可しません。 |
| `secure` | `boolean` | `false` | [`Secure` `Set-Cookie` 属性](https://tools.ietf.org/html/rfc6265#section-5.2.5)を設定します。<br/>**注意:** これを `true` に設定する際は注意してください。互換性のあるクライアントでは、ブラウザに HTTPS 接続がない場合、今後クッキーをサーバーに送り返さないため、hydration エラーにつながる可能性があります。 |
| `partitioned` | `boolean` | `false` | [`Partitioned` `Set-Cookie` 属性](https://datatracker.ietf.org/doc/html/draft-cutler-httpbis-partitioned-cookies#section-2.1)を設定します。<br/>**注意:** これはまだ完全に標準化されていない属性で、将来変更される可能性があります。<br/>これはまた、多くのクライアントが理解するまでこの属性を無視する可能性があることを意味します。<br/>詳細情報は[提案](https://github.com/privacycg/CHIPS)で見つけることができます。 |
| `domain` | `string` | `undefined` | [`Domain` `Set-Cookie` 属性](https://tools.ietf.org/html/rfc6265#section-5.2.3)を設定します。デフォルトではドメインは設定されず、ほとんどのクライアントはクッキーを現在のドメインのみに適用することを検討します。 |
| `path` | `string` | `'/'` | [`Path` `Set-Cookie` 属性](https://tools.ietf.org/html/rfc6265#section-5.2.4)を設定します。デフォルトでは、パスは[「デフォルトパス」](https://tools.ietf.org/html/rfc6265#section-5.1.4)とみなされます。 |
| `sameSite` | `boolean \| string` | `undefined` | [`SameSite` `Set-Cookie` 属性](https://tools.ietf.org/html/draft-ietf-httpbis-rfc6265bis-03#section-4.1.2.7)を設定します。<br/>- `true` は `SameSite` 属性を `Strict` に設定し、厳格な同一サイト強制を行います。<br/>- `false` は `SameSite` 属性を設定しません。<br/>- `'lax'` は `SameSite` 属性を `Lax` に設定し、緩い同一サイト強制を行います。<br/>- `'none'` は `SameSite` 属性を `None` に設定し、明示的なクロスサイトクッキーを作成します。<br/>- `'strict'` は `SameSite` 属性を `Strict` に設定し、厳格な同一サイト強制を行います。 |

## 戻り値

クッキー値を表す Vue `Ref<T>` を返します。ref を更新するとクッキーが更新されます（`readonly` が設定されていない限り）。ref は SSR に優しく、クライアントとサーバーの両方で動作します。

## 例

### 基本的な使用方法

以下の例では `counter` というクッキーを作成します。クッキーが存在しない場合、初期値としてランダムな値が設定されます。`counter` 変数を更新するたびに、クッキーもそれに応じて更新されます。

```vue [app.vue]
<script setup lang="ts">
const counter = useCookie('counter')

counter.value = counter.value || Math.round(Math.random() * 1000)
</script>

<template>
  <div>
    <h1>Counter: {{ counter || '-' }}</h1>
    <button @click="counter = null">reset</button>
    <button @click="counter--">-</button>
    <button @click="counter++">+</button>
  </div>
</template>
```

### 読み取り専用クッキー

```vue
<script setup lang="ts">
const user = useCookie(
  'userInfo',
  {
    default: () => ({ score: -1 }),
    watch: false
  }
)

if (user.value) {
  // 実際の `userInfo` クッキーは更新されません
  user.value.score++
}
</script>

<template>
  <div>User score: {{ user?.score }}</div>
</template>
```

### 書き込み可能クッキー

```vue
<script setup lang="ts">
const list = useCookie(
  'list',
  {
    default: () => [],
    watch: 'shallow'
  }
)

function add() {
  list.value?.push(Math.round(Math.random() * 1000))
  // この変更では list クッキーは更新されません
}

function save() {
  if (list.value) {
    // 実際の `list` クッキーが更新されます
    list.value = [...list.value]
  }
}
</script>

<template>
  <div>
    <h1>List</h1>
    <pre>{{ list }}</pre>
    <button @click="add">Add</button>
    <button @click="save">Save</button>
  </div>
</template>
```

### API ルートでのクッキー

[`h3`](https://github.com/h3js/h3) パッケージの `getCookie` と `setCookie` を使用して、サーバー API ルートでクッキーを設定できます。

```ts [server/api/counter.ts]
export default defineEventHandler(event => {
  // counter クッキーを読み取り
  let counter = getCookie(event, 'counter') || 0

  // counter クッキーを 1 増やす
  setCookie(event, 'counter', ++counter)

  // JSON レスポンスを送信
  return { counter }
})
```

:link-example{to="/docs/examples/advanced/use-cookie"}
