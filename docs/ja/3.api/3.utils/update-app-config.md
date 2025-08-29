---
title: 'updateAppConfig'
description: 'ランタイムで App Config を更新します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/nuxt/blob/main/packages/nuxt/src/app/config.ts
    size: xs
---

::note
深い割り当てを使用して [`app.config`](/docs/guide/directory-structure/app-config) を更新します。既存の（ネストされた）プロパティは保持されます。
::

## 使用方法

```js
const appConfig = useAppConfig() // { foo: 'bar' }

const newAppConfig = { foo: 'baz' }

updateAppConfig(newAppConfig)

console.log(appConfig) // { foo: 'baz' }
```

:read-more{to="/docs/guide/directory-structure/app-config"}
