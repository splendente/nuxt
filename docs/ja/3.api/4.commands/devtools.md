---
title: "nuxt devtools"
description: devtools コマンドを使用すると、プロジェクトごとに Nuxt DevTools を有効または無効にできます。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/devtools.ts
    size: xs
---

<!--devtools-cmd-->
```bash [Terminal]
npx nuxt devtools <COMMAND> [ROOTDIR] [--cwd=<directory>]
```
<!--/devtools-cmd-->

`nuxt devtools enable` を実行すると、Nuxt DevTools がグローバルにインストールされ、使用している特定のプロジェクト内でも有効になります。これはユーザーレベルの `.nuxtrc` に設定として保存されます。特定のプロジェクトで devtools サポートを削除したい場合は、`nuxt devtools disable` を実行できます。

## 引数

<!--devtools-args-->
引数 | 説明
--- | ---
`COMMAND` | 実行するコマンド（オプション: <enable\|disable>）
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/devtools-args-->

## オプション

<!--devtools-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
<!--/devtools-opts-->

::read-more{icon="i-simple-icons-nuxtdotjs" to="https://devtools.nuxt.com" target="\_blank"}
**Nuxt DevTools** についてもっと読む。
::
