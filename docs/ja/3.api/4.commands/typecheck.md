---
title: "nuxt typecheck"
description: typecheck コマンドは vue-tsc を実行してアプリ全体の型をチェックします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/typecheck.ts
    size: xs
---

<!--typecheck-cmd-->
```bash [Terminal]
npx nuxt typecheck [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>]
```
<!--/typecheck-cmd-->

`typecheck` コマンドは [`vue-tsc`](https://github.com/vuejs/language-tools/tree/master/packages/tsc) を実行してアプリ全体の型をチェックします。

## 引数

<!--typecheck-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/typecheck-args-->

## オプション

<!--typecheck-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
<!--/typecheck-opts-->

::note
このコマンドは `process.env.NODE_ENV` を `production` に設定します。これを上書きするには、[`.env`](/docs/guide/directory-structure/env) ファイルまたはコマンドライン引数で `NODE_ENV` を定義してください。
::

::read-more{to="/docs/guide/concepts/typescript#type-checking"}
ビルド時や開発時に型チェックを有効にする方法についてもっと読む。
::
