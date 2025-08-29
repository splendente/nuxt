---
title: "clearError"
description: "clearError composable は、すべての処理済みエラーをクリアします。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/error.ts
    size: xs
---

ページ、コンポーネント、プラグイン内で、`clearError` を使用してすべてのエラーをクリアし、ユーザーをリダイレクトできます。

**パラメーター:**

- `options?: { redirect?: string }`

リダイレクト先のオプションのパスを提供できます（例えば、「安全な」ページに移動したい場合）。

```js
// リダイレクトなし
clearError()

// リダイレクトあり
clearError({ redirect: '/homepage' })
```

エラーは [`useError()`](/docs/api/composables/use-error) を使用して state に設定されます。`clearError` composable はこの state をリセットし、提供されたオプションで `app:error:cleared` フックを呼び出します。

:read-more{to="/docs/getting-started/error-handling"}
