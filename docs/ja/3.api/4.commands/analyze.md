---
title: "nuxt analyze"
description: "Nuxt アプリケーションのプロダクションバンドルを解析します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/analyze.ts
    size: xs
---

<!--analyze-cmd-->
```bash [Terminal]
npx nuxt analyze [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--dotenv] [--name=<name>] [--no-serve]
```
<!--/analyze-cmd-->

`analyze` コマンドは Nuxt をビルドし、プロダクションバンドルを解析します（実験的）。

## 引数

<!--analyze-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/analyze-args-->

## オプション

<!--analyze-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`--name=<name>` | `default` | 解析の名前
`--no-serve` |  | 解析結果の配信をスキップします
<!--/analyze-opts-->

::note
このコマンドは `process.env.NODE_ENV` を `production` に設定します。
::
