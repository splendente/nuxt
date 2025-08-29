---
title: "nuxt build"
description: "Nuxt アプリケーションをビルドします。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/build.ts
    size: xs
---

<!--build-cmd-->
```bash [Terminal]
npx nuxt build [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--prerender] [--preset] [--dotenv] [--envName]
```
<!--/build-cmd-->

`build` コマンドは、アプリケーション、サーバー、および依存関係すべてがプロダクション対応となった `.output` ディレクトリを作成します。

## 引数

<!--build-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/build-args-->

## オプション

<!--build-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--prerender` |  | Nuxt をビルドして静的ルートをプリレンダリングします
`--preset` |  | Nitro サーバープリセット
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`--envName` |  | 設定オーバーライドを解決する際に使用する環境（ビルド時のデフォルトは `production`、dev サーバー実行時のデフォルトは `development`）
<!--/build-opts-->

::note
このコマンドは `process.env.NODE_ENV` を `production` に設定します。
::

::note
`--prerender` は常に `preset` を `static` に設定します
::
