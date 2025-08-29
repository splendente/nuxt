---
title: "useState"
description: useState composable はリアクティブで SSR フレンドリーな共有状態を作成します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/state.ts
    size: xs
---

## 使用方法

```ts
// リアクティブな状態を作成し、デフォルト値を設定
const count = useState('counter', () => Math.round(Math.random() * 100))
```

:read-more{to="/docs/getting-started/state-management"}

::important
`useState` 内のデータは JSON にシリアライズされるため、クラス、関数、シンボルなど、シリアライズできないものを含まないことが重要です。
::

::warning
`useState` はコンパイラによって変換される予約関数名であるため、独自の関数に `useState` という名前を付けるべきではありません。
::

:video-accordion{title="Watch a video from Alexander Lichter about why and when to use useState" videoId="mv0WcBABcIk"}

## `shallowRef` の使用

状態を深くリアクティブにする必要がない場合、`useState` を [`shallowRef`](https://vuejs.org/api/reactivity-advanced.html#shallowref) と組み合わせることができます。これは、状態が大きなオブジェクトや配列を含む場合にパフォーマンスを向上させることができます。

```ts
const state = useState('my-shallow-state', () => shallowRef({ deep: 'リアクティブではない' }))
// isShallow(state) === true
```

## 型

```ts
useState<T>(init?: () => T | Ref<T>): Ref<T>
useState<T>(key: string, init?: () => T | Ref<T>): Ref<T>
```

- `key`: リクエスト間でデータフェッチが適切に重複排除されることを保証する一意のキー。キーを提供しない場合、[`useState`](/docs/api/composables/use-state) のインスタンスのファイルと行番号に固有のキーが生成されます。
- `init`: 状態が初期化されていないときに状態の初期値を提供する関数。この関数は `Ref` を返すこともできます。
- `T`: (TypeScript のみ) 状態の型を指定
