---
title: 'defineLazyHydrationComponent'
description: '特定の戦略で lazy hydration コンポーネントを定義します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/components/plugins/lazy-hydration-macro-transform.ts
    size: xs
---

`defineLazyHydrationComponent` は、特定の lazy hydration 戦略でコンポーネントを作成するためのコンパイラーマクロです。Lazy hydration は、コンポーネントが表示されるまで、またはブラウザーがより重要なタスクを完了するまで hydration を遅延します。これにより、特に非本質的なコンポーネントにおいて、初期のパフォーマンスコストを大幅に削減できます。

## 使用方法

### 表示戦略

コンポーネントがビューポート内で表示されたときに hydrate します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'visible',
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!-- 
      要素がビューポートに入る 100px 手前に
      hydration がトリガーされます。
    -->
    <LazyHydrationMyComponent :hydrate-on-visible="{ rootMargin: '100px' }" />
  </div>
</template>
```

`hydrateOnVisible` prop はオプションです。内部の `IntersectionObserver` の動作をカスタマイズするためにオブジェクトを渡すことができます。

::read-more{to="https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserver/IntersectionObserver" title="IntersectionObserver options"}
`hydrate-on-visible` のオプションについて詳しく読む。
::

::note
内部的には、Vue 組み込みの [`hydrateOnVisible` 戦略](https://vuejs.org/guide/components/async.html#hydrate-on-visible)を使用しています。
::

### アイドル戦略

ブラウザーがアイドル状態のときにコンポーネントを hydrate します。これは、コンポーネントをできるだけ早く読み込む必要があるが、重要なレンダリングパスをブロックしたくない場合に適しています。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'idle',
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!-- ブラウザーがアイドル状態のときまたは 2000ms 後に hydration がトリガーされます。 -->
    <LazyHydrationMyComponent :hydrate-on-idle="2000" />
  </div>
</template>
```

`hydrateOnIdle` prop はオプションです。最大タイムアウトを指定するために正の数値を渡すことができます。

アイドル戦略は、ブラウザーがアイドル状態のときに hydrate できるコンポーネント向けです。

::note
内部的には、Vue 組み込みの [`hydrateOnIdle` 戦略](https://vuejs.org/guide/components/async.html#hydrate-on-idle)を使用しています。
::

### インタラクション戦略

指定されたインタラクション（例: クリック、マウスオーバー）後にコンポーネントを hydrate します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'interaction',
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!--
      要素にポインターがホバーしたときに
      hydration がトリガーされます。
    -->
    <LazyHydrationMyComponent hydrate-on-interaction="mouseover" />
  </div>
</template>
```

`hydrateOnInteraction` prop はオプションです。イベントまたはイベントのリストを渡さない場合、デフォルトで `pointerenter`、`click`、`focus` で hydrate します。

::note
内部的には、Vue 組み込みの [`hydrateOnInteraction` 戦略](https://vuejs.org/guide/components/async.html#hydrate-on-interaction)を使用しています。
::

### メディアクエリ戦略

ウィンドウがメディアクエリにマッチしたときにコンポーネントを hydrate します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'mediaQuery',
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!--
      ウィンドウ幅が 768px 以上のときに
      hydration がトリガーされます。
    -->
    <LazyHydrationMyComponent hydrate-on-media-query="(min-width: 768px)" />
  </div>
</template>
```

::note
内部的には、Vue 組み込みの [`hydrateOnMediaQuery` 戦略](https://vuejs.org/guide/components/async.html#hydrate-on-media-query)を使用しています。
::

### 時間戦略

指定された遅延（ミリ秒単位）後にコンポーネントを hydrate します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'time', 
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!-- 1000ms 後に hydration がトリガーされます。 -->
    <LazyHydrationMyComponent :hydrate-after="1000" />
  </div>
</template>
```

時間戦略は、特定の時間だけ待つことができるコンポーネント向けです。

### If 戦略

ブール条件に基づいてコンポーネントを hydrate します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'if',
  () => import('./components/MyComponent.vue')
)

const isReady = ref(false)

function myFunction() {
  // カスタム hydration 戦略をトリガー...
  isReady.value = true
}
</script>

<template>
  <div>
    <!-- isReady が true になったときに hydration がトリガーされます。 -->
    <LazyHydrationMyComponent :hydrate-when="isReady" />
  </div>
</template>
```

If 戦略は、常に hydrate する必要がない可能性のあるコンポーネントに最適です。

### 無効化

コンポーネントを絶対に hydrate しません。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'never',
  () => import('./components/MyComponent.vue')
)
</script>

<template>
  <div>
    <!-- このコンポーネントは Vue によって hydrate されることはありません。 -->
    <LazyHydrationMyComponent />
  </div>
</template>
```

### Hydration イベントのリスニング

すべての遅延 hydration コンポーネントは、hydrate されたときに `@hydrated` イベントを発行します。

```vue
<script setup lang="ts">
const LazyHydrationMyComponent = defineLazyHydrationComponent(
  'visible',
  () => import('./components/MyComponent.vue')
)

function onHydrate() {
  console.log("コンポーネントが hydrate されました!")
}
</script>

<template>
  <div>
    <LazyHydrationMyComponent
      :hydrate-on-visible="{ rootMargin: '100px' }"
      @hydrated="onHydrated"
    />
  </div>
</template>
```

## パラメーター

::warning
コンパイラーがこのマクロを正しく認識することを確実にするため、外部変数の使用を避けてください。以下のアプローチはマクロが正しく認識されない原因となります:

```vue
<script setup lang="ts">
const strategy = 'visible'
const source = () => import('./components/MyComponent.vue')
const LazyHydrationMyComponent = defineLazyHydrationComponent(strategy, source)
</script>
```
::

### `strategy`

- **Type**: `'visible' | 'idle' | 'interaction' | 'mediaQuery' | 'if' | 'time' | 'never'`
- **Required**: `true`

| 戦略      | 説明                                                    |
|---------------|----------------------------------------------------------------|
| `visible`     | コンポーネントがビューポート内で表示されたときに hydrate します。   |
| `idle`        | ブラウザーがアイドル状態のときまたは遅延後に hydrate します。            |
| `interaction` | ユーザーのインタラクション（例: クリック、ホバー）時に hydrate します。           |
| `mediaQuery`  | 指定されたメディアクエリ条件が満たされたときに hydrate します。      |
| `if`          | 指定されたブール条件が満たされたときに hydrate します。            |
| `time`        | 指定された時間遅延後に hydrate します。                         |
| `never`       | Vue がコンポーネントを hydrate することを防ぎます。                     |

### `source`

- **Type**: `() => Promise<Component>`
- **Required**: `true`
