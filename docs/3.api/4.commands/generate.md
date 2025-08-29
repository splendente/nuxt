---
title: "nuxt generate"
description: アプリケーションのすべてのルートを事前レンダリングし、結果をプレーンな HTML ファイルに保存します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/generate.ts
    size: xs
---

<!--generate-cmd-->
```bash [Terminal]
npx nuxt generate [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--preset] [--dotenv] [--envName]
```
<!--/generate-cmd-->

`generate` コマンドは、アプリケーションのすべてのルートを事前レンダリングし、結果を任意の静的ホスティングサービスにデプロイできるプレーンな HTML ファイルに保存します。このコマンドは `prerender` 引数を `true` に設定して `nuxt build` コマンドをトリガーします

## 引数

<!--generate-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/generate-args-->

## オプション

<!--generate-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--preset` |  | Nitro サーバープリセット
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`--envName` |  | 設定オーバーライドを解決する際に使用する環境（ビルド時のデフォルトは `production`、dev サーバー実行時のデフォルトは `development`）
<!--/generate-opts-->

::read-more{to="/docs/getting-started/deployment#static-hosting"}
事前レンダリングと静的ホスティングについてもっと読む。
::
