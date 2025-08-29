---
title: "nuxt add"
description: "Nuxt アプリケーションにエンティティをスキャフォールドします。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/add.ts
    size: xs
---

<!--add-cmd-->
```bash [Terminal]
npx nuxt add <TEMPLATE> <NAME> [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--force]
```
<!--/add-cmd-->

### 引数

<!--add-args-->
引数 | 説明
--- | ---
`TEMPLATE` | 生成するテンプレートを指定します（オプション: <api\|plugin\|component\|composable\|middleware\|layout\|page\|layer>）
`NAME` | 生成されるファイルの名前を指定します
<!--/add-args-->

### オプション

<!--add-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` | `.` | 作業ディレクトリを指定します
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--force` | `false` | ファイルが既に存在する場合、強制的に上書きします
<!--/add-opts-->

**修飾子:**

一部のテンプレートは、名前にサフィックス（`.client` や `.get` など）を追加するための追加の修飾子フラグをサポートしています。

```bash [Terminal]
# `/plugins/sockets.client.ts` を生成します
npx nuxt add plugin sockets --client
```

## `nuxt add component`

* 修飾子フラグ: `--mode client|server` または `--client` または `--server`

```bash [Terminal]
# `components/TheHeader.vue` を生成します
npx nuxt add component TheHeader
```

## `nuxt add composable`

```bash [Terminal]
# `composables/foo.ts` を生成します
npx nuxt add composable foo
```

## `nuxt add layout`

```bash [Terminal]
# `layouts/custom.vue` を生成します
npx nuxt add layout custom
```

## `nuxt add plugin`

* 修飾子フラグ: `--mode client|server` または `--client` または `--server`

```bash [Terminal]
# `plugins/analytics.ts` を生成します
npx nuxt add plugin analytics
```

## `nuxt add page`

```bash [Terminal]
# `pages/about.vue` を生成します
npx nuxt add page about
```

```bash [Terminal]
# `pages/category/[id].vue` を生成します
npx nuxt add page "category/[id]"
```

## `nuxt add middleware`

* 修飾子フラグ: `--global`

```bash [Terminal]
# `middleware/auth.ts` を生成します
npx nuxt add middleware auth
```

## `nuxt add api`

* 修飾子フラグ: `--method`（`connect`、`delete`、`get`、`head`、`options`、`patch`、`post`、`put`、または `trace` を受け付けます）または直接 `--get`、`--post` などを使用できます。

```bash [Terminal]
# `server/api/hello.ts` を生成します
npx nuxt add api hello
```

## `nuxt add layer`

```bash [Terminal]
# `layers/subscribe/nuxt.config.ts` を生成します
npx nuxt add layer subscribe
```
