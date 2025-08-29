---
title: "nuxt module"
description: "コマンドラインで Nuxt アプリケーションにモジュールを検索し追加します。"
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/module/
    size: xs
---

Nuxt は [Nuxt modules](/modules) をシームレスに扱うためのいくつかのユーティリティを提供します。

## nuxt module add

<!--module-add-cmd-->
```bash [Terminal]
npx nuxt module add <MODULENAME> [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--skipInstall] [--skipConfig] [--dev]
```
<!--/module-add-cmd-->

<!--module-add-args-->
引数 | 説明
--- | ---
`MODULENAME` | モジュール名
<!--/module-add-args-->

<!--module-add-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` | `.` | 作業ディレクトリを指定します
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--skipInstall` |  | npm install をスキップします
`--skipConfig` |  | nuxt.config.ts の更新をスキップします
`--dev` |  | モジュールを dev 依存関係としてインストールします
<!--/module-add-opts-->

このコマンドを使用すると、手動作業なしでアプリケーションに [Nuxt modules](/modules) をインストールできます。

コマンドを実行すると、以下が行われます:

- パッケージマネージャーを使用してモジュールを依存関係としてインストール
- [package.json](/docs/guide/directory-structure/package) ファイルに追加
- [`nuxt.config`](/docs/guide/directory-structure/nuxt-config) ファイルを更新

**例:**

[`Pinia`](/modules/pinia) モジュールをインストールする

```bash [Terminal]
npx nuxt module add pinia
```

## nuxt module search

<!--module-search-cmd-->
```bash [Terminal]
npx nuxt module search <QUERY> [--cwd=<directory>] [--nuxtVersion=<2|3>]
```
<!--/module-search-cmd-->

### 引数

<!--module-search-args-->
引数 | 説明
--- | ---
`QUERY` | 検索するキーワード
<!--/module-search-args-->

### オプション

<!--module-search-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` | `.` | 作業ディレクトリを指定します
`--nuxtVersion=<2\|3>` |  | Nuxt バージョンでフィルタリングし、互換性のあるモジュールのみを表示します（デフォルトで自動検出）
<!--/module-search-opts-->

このコマンドは、あなたのクエリにマッチし、Nuxt バージョンと互換性のある Nuxt モジュールを検索します。

**例:**

```bash [Terminal]
npx nuxt module search pinia
```
