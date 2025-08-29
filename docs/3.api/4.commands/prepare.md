---
title: 'nuxt prepare'
description: prepare コマンドは、アプリケーションに .nuxt ディレクトリを作成し、型を生成します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/prepare.ts
    size: xs
---

<!--prepare-cmd-->
```bash [Terminal]
npx nuxt prepare [ROOTDIR] [--dotenv] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--envName]
```
<!--/prepare-cmd-->

`prepare` コマンドは、アプリケーションに [`.nuxt`](/docs/guide/directory-structure/nuxt) ディレクトリを作成し、型を生成します。これは CI 環境や [`package.json`](/docs/guide/directory-structure/package) の `postinstall` コマンドとして有用です。

## 引数

<!--prepare-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/prepare-args-->

## オプション

<!--prepare-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--envName` |  | 設定オーバーライドを解決する際に使用する環境（ビルド時のデフォルトは `production`、dev サーバー実行時のデフォルトは `development`）
<!--/prepare-opts-->
