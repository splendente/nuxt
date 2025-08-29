---
title: "nuxt preview"
description: preview コマンドは、build コマンドの後にアプリケーションをプレビューするためのサーバーを起動します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/preview.ts
    size: xs
---

<!--preview-cmd-->
```bash [Terminal]
npx nuxt preview [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--envName] [--dotenv] [-p, --port]
```
<!--/preview-cmd-->

`preview` コマンドは、`build` コマンドを実行した後に Nuxt アプリケーションをプレビューするためのサーバーを起動します。`start` コマンドは `preview` のエイリアスです。本番環境でアプリケーションを実行する場合は、[デプロイメント](/docs/getting-started/deployment)のセクションを参照してください。

## 引数

<!--preview-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/preview-args-->

## オプション

<!--preview-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--envName` |  | 設定オーバーライドを解決する際に使用する環境（ビルド時のデフォルトは `production`、dev サーバー実行時のデフォルトは `development`）
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`-p, --port` |  | リスンするポート（デフォルト: `NUXT_PORT \|\| NITRO_PORT \|\| PORT`）
<!--/preview-opts-->

このコマンドは `process.env.NODE_ENV` を `production` に設定します。これを上書きするには、`.env` ファイルまたはコマンドライン引数で `NODE_ENV` を定義してください。

::note
便利なため、プレビューモードでは、[`.env`](/docs/guide/directory-structure/env) ファイルが `process.env` に読み込まれます。（ただし、本番環境では、環境変数を自分で設定する必要があります。例えば、Node.js 20+ では、`node --env-file .env .output/server/index.mjs` を実行してサーバーを起動することでこれを行うことができます。）
::
