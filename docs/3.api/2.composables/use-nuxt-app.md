---
title: 'useNuxtApp'
description: 'Nuxt アプリケーションの共有ランタイムコンテキストにアクセスします。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/nuxt.ts
    size: xs
---

`useNuxtApp` は、[Nuxt コンテキスト](/docs/guide/going-further/nuxt-app#the-nuxt-context)としても知られる Nuxt の共有ランタイムコンテキストにアクセスする方法を提供する組み込み composable です。これはクライアントとサーバーの両方で使用できます（ただし Nitro ルート内では使用できません）。Vue アプリインスタンス、ランタイムフック、ランタイム設定変数、`ssrContext` や `payload` などの内部状態にアクセスするのに役立ちます。

```vue [app.vue]
<script setup lang="ts">
const nuxtApp = useNuxtApp()
</script>
```

ランタイムコンテキストがスコープ内で利用できない場合、`useNuxtApp` は呼び出されると例外をスローします。`nuxtApp` を必要としない composable の場合、または例外なしにコンテキストが利用可能かどうかを単純に確認したい場合は、代わりに [`tryUseNuxtApp`](#tryusenuxtapp) を使用できます。

<!--
note
By default, the shared runtime context of Nuxt is namespaced under the [`buildId`](/docs/api/nuxt-config#buildid) option. It allows the support of multiple runtime contexts.

## Params

- `appName`: an optional application name. If you do not provide it, the Nuxt `buildId` option is used. Otherwise, it must match with an existing `buildId`. -->

## メソッド

### `provide (name, value)`

`nuxtApp` は [Nuxt プラグイン](/docs/guide/directory-structure/plugins)を使用して拡張できるランタイムコンテキストです。`provide` 関数を使用して Nuxt プラグインを作成し、Nuxt アプリケーション全体のすべての composable とコンポーネントで値とヘルパーメソッドを利用可能にできます。

`provide` 関数は `name` と `value` パラメーターを受け取ります。

```js
const nuxtApp = useNuxtApp()
nuxtApp.provide('hello', (name) => `Hello ${name}!`)

// "Hello name!" を出力
console.log(nuxtApp.$hello('name'))
```

上記の例でわかるように、`$hello` は `nuxtApp` コンテキストの新しいカスタム部分になり、`nuxtApp` がアクセス可能なすべての場所で利用できます。

### `hook(name, cb)`

`nuxtApp` で利用可能なフックは、Nuxt アプリケーションのランタイム側面をカスタマイズできます。Vue.js composables や [Nuxt プラグイン](/docs/guide/directory-structure/plugins)でランタイムフックを使用して、レンダリングライフサイクルにフックできます。

`hook` 関数は、レンダリングライフサイクルの特定のポイントにフックしてカスタムロジックを追加するのに便利です。`hook` 関数は主に Nuxt プラグインを作成するときに使用されます。

Nuxt によって呼び出される利用可能なランタイムフックについては、[ランタイムフック](/docs/api/advanced/hooks#app-hooks-runtime)を参照してください。

```ts [plugins/test.ts]
export default defineNuxtPlugin((nuxtApp) => {
  nuxtApp.hook('page:start', () => {
    /* あなたのコードをここに書きます */
  })
  nuxtApp.hook('vue:error', (..._args) => {
    console.log('vue:error')
    // if (import.meta.client) {
    //   console.log(..._args)
    // }
  })
})
```

### `callHook(name, ...args)`

`callHook` は既存のフックのいずれかで呼び出されたときに Promise を返します。

```ts
await nuxtApp.callHook('my-plugin:init')
```

## プロパティ

`useNuxtApp()` は、アプリを拡張およびカスタマイズし、状態、データ、変数を共有するために使用できる以下のプロパティを公開します。

### `vueApp`

`vueApp` は `nuxtApp` を通じてアクセスできるグローバルな Vue.js [アプリケーションインスタンス](https://vuejs.org/api/application.html#application-api)です。

いくつかの便利なメソッド:
- [`component()`](https://vuejs.org/api/application.html#app-component) - 名前文字列とコンポーネント定義の両方を渡すとグローバルコンポーネントを登録し、名前のみを渡すとすでに登録されているものを取得します。
- [`directive()`](https://vuejs.org/api/application.html#app-directive) - 名前文字列とディレクティブ定義の両方を渡すとグローバルカスタムディレクティブを登録し、名前のみを渡すとすでに登録されているものを取得します[(例)](/docs/guide/directory-structure/plugins#vue-directives)。
- [`use()`](https://vuejs.org/api/application.html#app-use) - **[Vue.js プラグイン](https://vuejs.org/guide/reusability/plugins.html)**をインストールします[(例)](/docs/guide/directory-structure/plugins#vue-plugins)。

:read-more{icon="i-simple-icons-vuedotjs" to="https://vuejs.org/api/application.html#application-api"}

### `ssrContext`

`ssrContext` はサーバーサイドレンダリング中に生成され、サーバーサイドでのみ利用可能です。

Nuxt は `ssrContext` を通じて以下のプロパティを公開します:
- `url` (string) - 現在のリクエスト URL。
- `event` ([h3js/h3](https://github.com/h3js/h3) リクエストイベント) - 現在のルートのリクエストとレスポンスにアクセス。
- `payload` (object) - NuxtApp payload オブジェクト。

### `payload`

`payload` はサーバーサイドからクライアントサイドにデータと状態変数を公開します。以下のキーは、サーバーサイドから渡された後、クライアントで利用可能になります:

- `serverRendered` (boolean) - レスポンスがサーバーサイドレンダリングされているかどうかを示します。
- `data` (object) - [`useFetch`](/docs/api/composables/use-fetch) または [`useAsyncData`](/docs/api/composables/use-async-data) を使用して API エンドポイントからデータをフェッチすると、結果の payload に `payload.data` からアクセスできます。このデータはキャッシュされ、同じリクエストが複数回行われた場合に同じデータをフェッチすることを防ぐのに役立ちます。

  ::code-group
  ```vue [app.vue]
  <script setup lang="ts">
  const { data } = await useAsyncData('count', () => $fetch('/api/count'))
  </script>
  ```
  ```ts [server/api/count.ts]
  export default defineEventHandler(event => {
    return { count: 1 }
  })
  ```
  ::

  上記の例で [`useAsyncData`](/docs/api/composables/use-async-data) を使用して `count` の値をフェッチした後、`payload.data` にアクセスすると、そこに `{ count: 1 }` が記録されているのを確認できます。

  [`ssrcontext`](#ssrcontext) から同じ `payload.data` にアクセスすると、サーバーサイドでも同じ値にアクセスできます。

- `state` (object) - Nuxt で [`useState`](/docs/api/composables/use-state) composable を使用して共有状態を設定すると、この状態データに `payload.state.[name-of-your-state]` を通じてアクセスできます。

  ```ts [plugins/my-plugin.ts]
  export const useColor = () => useState<string>('color', () => 'pink')

  export default defineNuxtPlugin((nuxtApp) => {
    if (import.meta.server) {
      const color = useColor()
    }
  })
  ```

  It is also possible to use more advanced types, such as `ref`, `reactive`, `shallowRef`, `shallowReactive` and `NuxtError`.

  [Nuxt v3.4](https://nuxt.com/blog/v3-4#payload-enhancements) 以降、Nuxt でサポートされていない型に対して独自の reducer/reviver を定義することができます。

  :video-accordion{title="Watch a video from Alexander Lichter about serializing payloads, especially with regards to classes" videoId="8w6ffRBs8a4"}

  以下の例では、payload プラグインを使用して [Luxon](https://moment.github.io/luxon/#/) DateTime クラスの reducer（またはシリアライザー）と reviver（またはデシリアライザー）を定義します。

  ```ts [plugins/date-time-payload.ts]
  /**
   * This kind of plugin runs very early in the Nuxt lifecycle, before we revive the payload.
   * You will not have access to the router or other Nuxt-injected properties.
   *
   * Note that the "DateTime" string is the type identifier and must
   * be the same on both the reducer and the reviver.
   */
  export default definePayloadPlugin((nuxtApp) => {
    definePayloadReducer('DateTime', (value) => {
      return value instanceof DateTime && value.toJSON()
    })
    definePayloadReviver('DateTime', (value) => {
      return DateTime.fromISO(value)
    })
  })
  ```

### `isHydrating`

`nuxtApp.isHydrating`（boolean）を使用して、Nuxt アプリがクライアントサイドで hydration 中かどうかを確認します。

```ts [components/nuxt-error-boundary.ts]
export default defineComponent({
  setup (_props, { slots, emit }) {
    const nuxtApp = useNuxtApp()
    onErrorCaptured((err) => {
      if (import.meta.client && !nuxtApp.isHydrating) {
        // ...
      }
    })
  }
})
```

### `runWithContext`

::note
「Nuxt instance unavailable」メッセージが表示されたためここにいる可能性があります。このメソッドは控えめに使用し、問題を引き起こしている例を報告してください。最終的にフレームワークレベルで解決できるようにするためです。
::

`runWithContext` メソッドは、関数を呼び出して明示的な Nuxt コンテキストを提供するために使用されます。通常、Nuxt コンテキストは暗黙的に受け渡され、これについて心配する必要はありません。ただし、ミドルウェア/プラグインで複雑な `async`/`await` シナリオを扱う場合、非同期呼び出し後に現在のインスタンスが未設定になる場合があります。

```ts [middleware/auth.ts]
export default defineNuxtRouteMiddleware(async (to, from) => {
  const nuxtApp = useNuxtApp()
  let user
  try {
    user = await fetchUser()
    // the Vue/Nuxt compiler loses context here because of the try/catch block.
  } catch (e) {
    user = null
  }
  if (!user) {
    // apply the correct Nuxt context to our `navigateTo` call.
    return nuxtApp.runWithContext(() => navigateTo('/auth'))
  }
})
```

#### 使用方法

```js
const result = nuxtApp.runWithContext(() => functionWithContext())
```

- `functionWithContext`: 現在の Nuxt アプリケーションのコンテキストを必要とする任意の関数。このコンテキストは自動的に正しく適用されます。

`runWithContext` は `functionWithContext` によって返されるものを返します。

#### コンテキストのより詳細な説明

Vue.js Composition API（および同様に Nuxt composable）は、暗黙的なコンテキストに依存して動作します。ライフサイクル中に、Vue は現在のコンポーネントの一時的なインスタンス（および Nuxt の nuxtApp の一時的なインスタンス）をグローバル変数に設定し、同じ tick で解除します。サーバーサイドでレンダリングする際、異なるユーザーからの複数のリクエストと nuxtApp が同じグローバルコンテキストで実行されます。このため、Nuxt と Vue は、2つのユーザーやコンポーネント間で共有参照がリークすることを避けるために、このグローバルインスタンスをすぐに解除します。

これが意味することは何でしょうか？ Composition API と Nuxt Composable は、ライフサイクル中および非同期操作前の同じ tick 内でのみ利用可能です：

```js
// --- Vue internal ---
const _vueInstance = null
const getCurrentInstance = () => _vueInstance
// ---

// Vue / Nuxt sets a global variable referencing to current component in _vueInstance when calling setup()
async function setup() {
  getCurrentInstance() // Works
  await someAsyncOperation() // Vue unsets the context in same tick before async operation!
  getCurrentInstance() // null
}
```

これに対する古典的な解決策は、最初の呼び出し時に現在のインスタンスを `const instance = getCurrentInstance()` のようなローカル変数にキャッシュし、次の composable 呼び出しでそれを使用することです。しかし問題は、ネストされた composable 呼び出しがすべて、明示的にインスタンスを引数として受け取る必要があり、composition-api の暗黙的なコンテキストに依存できないことです。これは composable の設計上の制限であり、それ自体が問題というわけではありません。

この制限を克服するため、Vue はアプリケーションコードをコンパイルする際に舞台裏で作業を行い、`<script setup>` の各呼び出し後にコンテキストを復元します：

```js
const __instance = getCurrentInstance() // Generated by Vue compiler
getCurrentInstance() // Works!
await someAsyncOperation() // Vue unsets the context
__restoreInstance(__instance) // Generated by Vue compiler
getCurrentInstance() // Still works!
```

Vue が実際に何を行うかについてのより良い説明については、[unjs/unctx#2 (comment)](https://github.com/unjs/unctx/issues/2#issuecomment-942193723) を参照してください。

#### 解決策

ここで `runWithContext` を使用して、`<script setup>` の動作と同様にコンテキストを復元できます。

Nuxt は内部的に [unjs/unctx](https://github.com/unjs/unctx) を使用して、プラグインやミドルウェアで Vue と同様の composable をサポートします。これにより、`navigateTo()` などの composable が `nuxtApp` を直接渡すことなく動作し、Composition API の DX およびパフォーマンスの利点を Nuxt フレームワーク全体にもたらします。

Nuxt composable は Vue Composition API と同じ設計を持っているため、この変換を魔法のように行うための同様の解決策が必要です。[unjs/unctx#2](https://github.com/unjs/unctx/issues/2)（提案）、[unjs/unctx#4](https://github.com/unjs/unctx/pull/4)（変換実装）、[nuxt/framework#3884](https://github.com/nuxt/framework/pull/3884)（Nuxt への統合）を確認してください。

Vue は現在、async/await 使用における `<script setup>` の非同期コンテキスト復元のみをサポートしています。Nuxt では、`defineNuxtPlugin()` と `defineNuxtRouteMiddleware()` の変換サポートが追加されました。これは、それらを使用すると Nuxt がコンテキスト復元で自動的に変換することを意味します。

#### 残りの問題

`unjs/unctx` の自動的なコンテキスト復元への変換は、`await` を含む `try/catch` 文でバグがあるようで、最終的に上記で提案された回避策の要件を削除するために解決される必要があります。

#### ネイティブ非同期コンテキスト

新しい実験的機能を使用して、[Node.js `AsyncLocalStorage`](https://nodejs.org/api/async_context.html#class-asynclocalstorage) と新しい unctx サポートを使用してネイティブ非同期コンテキストサポートを有効にし、変換や手動での引き渡し/コンテキストでの呼び出しを必要とせずに、**任意のネストされた非同期 composable** で非同期コンテキストを**ネイティブ**に利用可能にすることができます。

::tip
ネイティブ非同期コンテキストサポートは現在 Bun と Node で動作します。
::

:read-more{to="/docs/guide/going-further/experimental-features#asynccontext"}

## tryUseNuxtApp

この関数は `useNuxtApp` とまったく同じように動作しますが、例外をスローする代わりに、コンテキストが利用できない場合は `null` を返します。

`nuxtApp` を必要としない composable に使用したり、例外なしにコンテキストが利用可能かどうかを単純に確認したりできます。

使用例：

```ts [composable.ts]
export function useStandType() {
  // クライアントでは常に動作
  if (tryUseNuxtApp()) {
    return useRuntimeConfig().public.STAND_TYPE
  } else {
    return process.env.STAND_TYPE
  }
}
```

<!-- ### Params

- `appName`: an optional application name. If you do not provide it, the Nuxt `buildId` option is used. Otherwise, it must match with an existing `buildId`. -->
