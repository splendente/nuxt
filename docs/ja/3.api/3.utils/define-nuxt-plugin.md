---
title: "defineNuxtPlugin"
description: defineNuxtPlugin() は Nuxt プラグインを作成するためのヘルパー関数です。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/nuxt.ts
    size: xs
---

`defineNuxtPlugin` は、機能強化と型安全性を備えた Nuxt プラグインを作成するためのヘルパー関数です。このユーティリティは、異なるプラグイン形式を、Nuxt のプラグインシステム内でシームレスに動作する一貫した構造に正規化します。

```ts twoslash [plugins/hello.ts]
export default defineNuxtPlugin((nuxtApp) => {
  // nuxtApp で何かを行う
})
```

:read-more{to="/docs/guide/directory-structure/plugins#creating-plugins"}

## 型

```ts
defineNuxtPlugin<T extends Record<string, unknown>>(plugin: Plugin<T> | ObjectPlugin<T>): Plugin<T> & ObjectPlugin<T>

type Plugin<T> = (nuxt: [NuxtApp](/docs/guide/going-further/internals#the-nuxtapp-interface)) => Promise<void> | Promise<{ provide?: T }> | void | { provide?: T }

interface ObjectPlugin<T> {
  name?: string
  enforce?: 'pre' | 'default' | 'post'
  dependsOn?: string[]
  order?: number
  parallel?: boolean
  setup?: Plugin<T>
  hooks?: Partial<[RuntimeNuxtHooks](/docs/api/advanced/hooks#app-hooks-runtime)>
  env?: {
    islands?: boolean
  }
}
```

## パラメーター

**plugin**: プラグインは2つの方法で定義できます:
1. **関数プラグイン**: [`NuxtApp`](/docs/guide/going-further/internals#the-nuxtapp-interface) インスタンスを受け取る関数で、[`NuxtApp`](/docs/guide/going-further/internals#the-nuxtapp-interface) インスタンス上でヘルパーを提供したい場合は、[`provide`](/docs/guide/directory-structure/plugins#providing-helpers) プロパティを持つオブジェクトと共に promise を返すことができます。
2. **オブジェクトプラグイン**: `name`、`enforce`、`dependsOn`、`order`、`parallel`、`setup`、`hooks`、`env` など、プラグインの動作を設定するための様々なプロパティを含めることができるオブジェクトです。

| プロパティ           | 型                                                                 | 必須 | 説明                                                                                                     |
| ------------------ | -------------------------------------------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
| `name` | `string` | `false` | プラグインのオプション名で、デバッグと依存関係管理に便利です。 |
| `enforce` | `'pre'` \| `'default'` \| `'post'` | `false` | 他のプラグインとの相対的な実行タイミングを制御します。 |
| `dependsOn` | `string[]` | `false` | このプラグインが依存するプラグイン名の配列。適切な実行順序を保証します。 |
| `order` | `number` | `false` | プラグインの順序をより細かく制御でき、上級ユーザーのみが使用すべきです。**`enforce` の値をオーバーライドし、プラグインのソートに使用されます。** |
| `parallel` | `boolean` | `false` | 他の並列プラグインと並行してプラグインを実行するかどうか。 |
| `setup` | `Plugin<T>`{lang="ts"}  | `false` | 関数プラグインに相当するメインプラグイン関数。 |
| `hooks` | `Partial<RuntimeNuxtHooks>`{lang="ts"}  | `false` | 直接登録する Nuxt アプリランタイムフック。 |
| `env` | `{ islands?: boolean }`{lang="ts"}  | `false` | サーバー専用または island コンポーネントをレンダリングするときにプラグインを実行したくない場合は、この値を `false` に設定します。 |

:video-accordion{title="Watch a video from Alexander Lichter about the Object Syntax for Nuxt plugins" videoId="2aXZyXB1QGQ"}

## 例

### 基本的な使用方法

以下の例は、グローバル機能を追加するシンプルなプラグインを示しています:

```ts twoslash [plugins/hello.ts]
export default defineNuxtPlugin((nuxtApp) => {
  // グローバルメソッドを追加
  return {
    provide: {
      hello: (name: string) => `Hello ${name}!`
    }
  }
})
```

### オブジェクト構文プラグイン

以下の例は、高度な設定でオブジェクト構文を示しています:

```ts twoslash [plugins/advanced.ts]
export default defineNuxtPlugin({
  name: 'my-plugin',
  enforce: 'pre',
  async setup (nuxtApp) {
    // プラグインセットアップロジック
    const data = await $fetch('/api/config')
    
    return {
      provide: {
        config: data
      }
    }
  },
  hooks: {
    'app:created'() {
      console.log('アプリが作成されました!')
    }
  },
})
```
