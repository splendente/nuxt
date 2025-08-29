---
title: 'showError'
description: Nuxt は、必要に応じて全画面エラーページを表示する迅速で簡単な方法を提供します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/composables/error.ts
    size: xs
---

[Nuxt context](/docs/guide/going-further/nuxt-app#the-nuxt-context) 内では、`showError` を使用してエラーを表示できます。

**パラメーター:**

- `error`: `string | Error | Partial<{ cause, data, message, name, stack, statusCode, statusMessage }>`

```ts
showError("😱 Oh no, an error has been thrown.")
showError({
  statusCode: 404,
  statusMessage: "Page Not Found"
})
```

エラーは [`useError()`](/docs/api/composables/use-error) を使用して状態に設定され、コンポーネント間でリアクティブで SSR フレンドリーな共有エラー状態を作成します。

::tip
`showError` は `app:error` フックを呼び出します。
::

:read-more{to="/docs/getting-started/error-handling"}
