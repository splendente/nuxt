---
title: "create nuxt"
description: init コマンドは、新しい Nuxt プロジェクトを初期化します。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/init.ts
    size: xs
---

<!--init-cmd-->
```bash [Terminal]
npm create nuxt@latest [DIR] [--cwd=<directory>] [-t, --template] [-f, --force] [--offline] [--preferOffline] [--no-install] [--gitInit] [--shell] [--packageManager]
```
<!--/init-cmd-->

`create-nuxt` コマンドは、[unjs/giget](https://github.com/unjs/giget) を使用して新しい Nuxt プロジェクトを初期化します。

## 引数

<!--init-args-->
引数 | 説明
--- | ---
`DIR=""` | プロジェクトディレクトリ
<!--/init-args-->

## オプション

<!--init-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` | `.` | 作業ディレクトリを指定します
`-t, --template` |  | テンプレート名
`-f, --force` |  | 既存のディレクトリを上書きします
`--offline` |  | オフラインモードを強制します
`--preferOffline` |  | オフラインモードを優先します
`--no-install` |  | 依存関係のインストールをスキップします
`--gitInit` |  | git リポジトリを初期化します
`--shell` |  | プロジェクトディレクトリでのインストール後にシェルを開始します
`--packageManager` |  | パッケージマネージャーの選択（npm、pnpm、yarn、bun）
<!--/init-opts-->

## 環境変数

- `NUXI_INIT_REGISTRY`: カスタムテンプレートレジストリに設定します。（[詳細はこちら](https://github.com/unjs/giget#custom-registry)）。
  - デフォルトのレジストリは [nuxt/starter/templates](https://github.com/nuxt/starter/tree/templates/templates) から読み込まれます
