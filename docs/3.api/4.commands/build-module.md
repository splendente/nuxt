---
title: 'nuxt build-module'
description: 'Nuxt モジュールを公開前にビルドするための Nuxt コマンドです。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/module-builder/blob/main/src/cli.ts
    size: xs
---

<!--build-module-cmd-->
```bash [Terminal]
npx nuxt build-module [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--build] [--stub] [--sourcemap] [--prepare]
```
<!--/build-module-cmd-->

`build-module` コマンドは `@nuxt/module-builder` を実行して、**nuxt-module** の完全なビルドを含む `dist` ディレクトリを `rootDir` 内に生成します。

## 引数

<!--build-module-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/build-module-args-->

## オプション

<!--build-module-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--build` | `false` | モジュールを配布用にビルドします
`--stub` | `false` | 開発用に実際にビルドする代わりに dist をスタブ化します
`--sourcemap` | `false` | ソースマップを生成します
`--prepare` | `false` | モジュールをローカル開発用に準備します
<!--/build-module-opts-->

::read-more{to="https://github.com/nuxt/module-builder" icon="i-simple-icons-github" target="\_blank"}
`@nuxt/module-builder` についてもっと読む。
::
