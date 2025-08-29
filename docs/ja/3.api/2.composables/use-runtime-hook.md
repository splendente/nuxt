---
title: useRuntimeHook
description: Nuxt アプリケーションでランタイムフックを登録し、スコープが破棄されたときに適切に破棄されることを保証します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/runtime-hook.ts
    size: xs
---

::important
この composable は Nuxt v3.14+ で利用できます。
::

```ts [signature]
function useRuntimeHook<THookName extends keyof RuntimeNuxtHooks>(
  name: THookName,
  fn: RuntimeNuxtHooks[THookName] extends HookCallback ? RuntimeNuxtHooks[THookName] : never
): void
```

## 使用方法

### パラメーター

- `name`: 登録するランタイムフックの名前。[ランタイム Nuxt フックの完全なリストはこちら](/docs/api/advanced/hooks#app-hooks-runtime)で参照できます。
- `fn`: フックがトリガーされたときに実行されるコールバック関数。関数のシグネチャはフック名によって異なります。

### 戻り値

この composable は値を返しませんが、コンポーネントのスコープが破棄されたときに自動的にフックの登録を解除します。

## 例

```vue twoslash [pages/index.vue]
<script setup lang="ts">
// リンクがプリフェッチされるたびに実行されるフックを登録しますが、
// コンポーネントがアンマウントされたときに自動的にクリーンアップされます（再度呼ばれることはありません）
useRuntimeHook('link:prefetch', (link) => {
  console.log('Prefetching', link)
})
</script>
```
