---
title: 'useServerSeoMeta'
description: useServerSeoMeta composable を使用すると、サイトの SEO メタタグを完全な TypeScript サポートでフラットオブジェクトとして定義できます。
links:
  - label: Source
    icon: i-simple-icons-github
    to: https://github.com/unjs/unhead/blob/main/packages/vue/src/composables.ts
    size: xs
---

[`useSeoMeta`](/docs/api/composables/use-seo-meta) と同様に、`useServerSeoMeta` composable を使用すると、サイトの SEO メタタグを完全な TypeScript サポートでフラットオブジェクトとして定義できます。

:read-more{to="/docs/api/composables/use-seo-meta"}

ほとんどの場合、ロボットは初期読み込みのみをスキャンするため、メタはリアクティブである必要がありません。そのため、クライアントでは何もしない（または `head` オブジェクトを返さない）パフォーマンス重視のユーティリティとして [`useServerSeoMeta`](/docs/api/composables/use-server-seo-meta) を使用することを推奨します。

```vue [app.vue]
<script setup lang="ts">
useServerSeoMeta({
  robots: 'index, follow'
})
</script>
```

パラメーターは [`useSeoMeta`](/docs/api/composables/use-seo-meta) とまったく同じです

:read-more{to="/docs/getting-started/seo-meta"}
