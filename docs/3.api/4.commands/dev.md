---
title: 'nuxt dev'
description: dev コマンドは http://localhost:3000 でホットモジュール置換を使用した開発サーバーを起動します
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/nuxt/cli/blob/main/packages/nuxi/src/commands/dev.ts
    size: xs
---

<!--dev-cmd-->
```bash [Terminal]
npx nuxt dev [ROOTDIR] [--cwd=<directory>] [--logLevel=<silent|info|verbose>] [--dotenv] [--envName] [--no-clear] [--no-fork] [-p, --port] [-h, --host] [--clipboard] [-o, --open] [--https] [--publicURL] [--qr] [--public] [--tunnel] [--sslCert] [--sslKey]
```
<!--/dev-cmd-->

`dev` コマンドは [http://localhost:3000](https://localhost:3000) でホットモジュール置換を使用した開発サーバーを起動します

## 引数

<!--dev-args-->
引数 | 説明
--- | ---
`ROOTDIR="."` | 作業ディレクトリを指定します（デフォルト: `.`）
<!--/dev-args-->

## オプション

<!--dev-opts-->
オプション | デフォルト | 説明
--- | --- | ---
`--cwd=<directory>` |  | 作業ディレクトリを指定します。これは ROOTDIR より優先されます（デフォルト: `.`）
`--logLevel=<silent\|info\|verbose>` |  | ビルド時のログレベルを指定します
`--dotenv` |  | 読み込む `.env` ファイルのパス（ルートディレクトリからの相対パス）
`--envName` |  | 設定オーバーライドを解決する際に使用する環境（ビルド時のデフォルトは `production`、dev サーバー実行時のデフォルトは `development`）
`--no-clear` |  | 再起動時にコンソールクリアを無効にします
`--no-fork` |  | フォークモードを無効にします
`-p, --port` |  | リスンするポート（デフォルト: `NUXT_PORT \|\| NITRO_PORT \|\| PORT \|\| nuxtOptions.devServer.port`）
`-h, --host` |  | リスンするホスト（デフォルト: `NUXT_HOST \|\| NITRO_HOST \|\| HOST \|\| nuxtOptions._layers?.[0]?.devServer?.host`）
`--clipboard` | `false` | URL をクリップボードにコピーします
`-o, --open` | `false` | ブラウザで URL を開きます
`--https` |  | HTTPS を有効にします
`--publicURL` |  | 表示されるパブリック URL（QR コードに使用）
`--qr` |  | パブリック URL の QR コードを利用可能な場合に表示します
`--public` |  | すべてのネットワークインターフェースをリスンします
`--tunnel` |  | https://github.com/unjs/untun を使用してトンネルを開きます
`--sslCert` |  | （非推奨）代わりに `--https.cert` を使用してください。
`--sslKey` |  | （非推奨）代わりに `--https.key` を使用してください。
<!--/dev-opts-->

ポートとホストは、NUXT_PORT、PORT、NUXT_HOST または HOST 環境変数でも設定できます。

上記のオプションに加えて、`@nuxt/cli` は `listhen` にオプションを渡すことができます。例えば `--no-qr` で dev サーバーの QR コードを無効にできます。`listhen` オプションのリストは [unjs/listhen](https://github.com/unjs/listhen) ドキュメントで確認できます。

このコマンドは `process.env.NODE_ENV` を `development` に設定します。

::note
開発で自己署名証明書を使用している場合、環境で `NODE_TLS_REJECT_UNAUTHORIZED=0` を設定する必要があります。
::
