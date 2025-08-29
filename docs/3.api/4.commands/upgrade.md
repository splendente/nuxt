---
title: "nuxt upgrade"
description: upgrade コマンドは Nuxt を最新バージョンにアップグレードします。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/upgrade.ts
    size: xs
---

<!--upgrade-cmd-->
```bash [Terminal]
npx nuxt upgrade [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--dedupe] [-f, --force] [-ch, --channel=<stable|nightly>]
```
<!--/upgrade-cmd-->

`upgrade` コマンドは Nuxt を最新バージョンにアップグレードします。

## 引数

<!--upgrade-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/upgrade-args-->

## オプション

<!--upgrade-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--dedupe` |  | 依存関係を重複排除しますが、ロックファイルは再作成しません
`-f, --force` |  | ロックファイルと node_modules を再作成するために強制的にアップグレードします
`-ch, --channel=<stable\|nightly>` | `stable` | インストール元のチャンネルを指定します（デフォルト: stable）
<!--/upgrade-opts-->
