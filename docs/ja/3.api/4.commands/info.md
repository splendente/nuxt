---
title: "nuxt info"
description: info コマンドは、現在または指定された Nuxt プロジェクトに関する情報をログ出力します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/info.ts
    size: xs
---

<!--info-cmd-->
```bash [Terminal]
npx nuxt info [ROOTDIR] [--cwd=<directory>]
```
<!--/info-cmd-->

`info` コマンドは、現在または指定された Nuxt プロジェクトに関する情報をログ出力します。

## 引数

<!--info-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/info-args-->

## オプション

<!--info-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
<!--/info-opts-->
