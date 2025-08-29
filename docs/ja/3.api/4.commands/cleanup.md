---
title: 'nuxt cleanup'
description: '一般的に生成される Nuxt ファイルとキャッシュを削除します。'
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/cleanup.ts
    size: xs
---

<!--cleanup-cmd-->
```bash [Terminal]
npx nuxt cleanup [ROOTDIR] [--cwd=<directory>]
```
<!--/cleanup-cmd-->

`cleanup` コマンドは、以下を含む一般的に生成される Nuxt ファイルとキャッシュを削除します:

- `.nuxt`
- `.output`
- `node_modules/.vite`
- `node_modules/.cache`

## 引数

<!--cleanup-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/cleanup-args-->

## オプション

<!--cleanup-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
<!--/cleanup-opts-->
