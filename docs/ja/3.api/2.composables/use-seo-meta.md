---
title: 'useSeoMeta'
description: useSeoMeta composable を使用すると、サイトの SEO メタタグを完全な TypeScript サポートでフラットオブジェクトとして定義できます。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/unjs/unhead/blob/main/packages/vue/src/composables.ts
    size: xs
---

これは、`property` の代わりに `name` を使用するなどの一般的な間違やタイプミスを防ぐのに役立ちます - 100+ 以上のメタタグが完全に型付けされています。

::important
XSS 安全で完全な TypeScript サポートを持つため、これはサイトにメタタグを追加する推奨される方法です。
::

:read-more{to="/docs/getting-started/seo-meta"}

## 使用方法

```vue [app.vue]
<script setup lang="ts">
useSeoMeta({
  title: 'My Amazing Site',
  ogTitle: 'My Amazing Site',
  description: 'This is my amazing site, let me tell you all about it.',
  ogDescription: 'This is my amazing site, let me tell you all about it.',
  ogImage: 'https://example.com/image.png',
  twitterCard: 'summary_large_image',
})
</script>
```

リアクティブなタグを挿入するときは、computed getter 構文 (`() => value`) を使用すべきです:

```vue [app.vue]
<script setup lang="ts">
const title = ref('My title')

useSeoMeta({
  title,
  description: () => `これは ${title.value} ページの説明です`
})
</script>
```

## パラメーター

100 以上のパラメーターがあります。[ソースコードのパラメーターの完全なリスト](https://github.com/harlan-zw/zhead/blob/main/packages/zhead/src/metaFlat.ts#L1035)を参照してください。

:read-more{to="/docs/getting-started/seo-meta"}

## パフォーマンス

ほとんどの場合、SEO メタタグはリアクティブである必要がありません。検索エンジンロボットは主に初期ページ読み込みをスキャンするからです。

パフォーマンスを向上させるため、メタタグがリアクティブである必要がない場合、`useSeoMeta` 呼び出しをサーバーのみの条件でラップできます:

```vue [app.vue]
<script setup lang="ts">
if (import.meta.server) {
  // これらのメタタグはサーバーサイドレンダリング中のみ追加されます
  useSeoMeta({
    robots: 'index, follow',
    description: 'リアクティビティを必要としない静的な説明',
    ogImage: 'https://example.com/image.png',
    // その他の静的なメタタグ...
  })
}

const dynamicTitle = ref('My title')
// 必要な場合のみ条件の外でリアクティブなメタタグを使用
useSeoMeta({
  title: () => dynamicTitle.value,
  ogTitle: () => dynamicTitle.value,
})
</script>
```

以前は [`useServerSeoMeta`](/docs/api/composables/use-server-seo-meta) composable を使用していましたが、このアプローチが推奨され、非推奨となりました。
