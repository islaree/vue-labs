### single file components (SFC)

`*.vue`ファイル

vueコンポーネントのテンプレート、ロジック、スタイルをカプセル化できる特別なファイル形式

```vue
<script setup>
import { ref } from 'vue'
const greeting = ref('Hello World!')
</script>

<template>
  <p class="greeting">{{ greeting }}</p>
</template>

<style>
.greeting {
  color: red;
  font-weight: bold;
}
</style>
```
