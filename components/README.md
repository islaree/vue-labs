## components

- コンポーネントの基本
- コンポーネントの詳細

### コンポーネントの基本

#### コンポーネントの使用

```javascript
// components/HelloWorld.vue

<template>
  <h1>hello world</h1>
</template>

```

```javascript
// App.vue

<script setup>
  import HelloWorld from "./components/HelloWorld.vue";
</script>

<template>
  <HelloWorld />
</template>

```

#### propsの受け渡し

##### 文字列や数値を渡す場合

```javascript
// App.vue

<script setup>
  import HelloWorld from "./components/HelloWorld.vue";
</script>

<template>
  <HelloWorld title="hello world" />
</template>
```

```javascript
// components/HelloWorld.vue

// setup構文を使用したパターン
<script setup>
  defineProps(["title"])
</script>

<template>
  <h1>{{ title }}</h1>
</template>
```
> [!TIP]
> `setup`構文を使用しているため、`defineProps`で定義した`props`を直接変数のように使用できます。
> `defineProps`は`setup`構文内でしか使用できません。

```javascript
// components/HelloWorld.vue

// setup構文を使用しないパターン
<script>
export default {
  props: {
    title: String,
  },
  setup(props) {
    return { title: props.title };
  },
};
</script>

<template>
  <h1>{{ title }}</h1>
</template>

```

##### オブジェクトを渡す場合

```javascript
// App.vue

<script setup>
  const posts = [
    { id: 1, title: "My journey with Vue" },
    { id: 2, title: "Blogging with Vue" },
    { id: 3, title: "Why Vue is so fun" },
  ];
</script>

<template>
  <BlogPosts v-for="post in posts" :key="post.id" :title="post.title" />
</template>
```

```javascript
// components/BlogPosts.vue

<script setup>
defineProps(["title"]);
</script>

<template>
  <h4>{{ title }}</h4>
</template>

```

### イベントの購読

```javascript
// App.vue

<script setup>
import { ref } from "vue";
import BlogPosts from "./components/BlogPosts.vue";

const posts = ref([
  { id: 1, title: "My journey with Vue" },
  { id: 2, title: "Blogging with Vue" },
  { id: 3, title: "Why Vue is so fun" },
]);

const postFontSize = ref(1);
</script>

<template>
  <HelloWorld title="hello world" />
  <div :style="{ fontSize: postFontSize + 'em' }">
    <BlogPosts
      v-for="post in posts"
      :key="post.id"
      :title="post.title"
      @enlarge-text="postFontSize += 0.1"
    />
  </div>
</template>
```

```javascript
// components/BlogPost.vue

<script setup>
const props = defineProps(["title"]);
const emit = defineEmits(["enlarge-text"]);

const handleClick = () => {
  emit("enlarge-text");
};
</script>

<template>
  <h4>{{ title }}</h4>
  <button @click="handleClick">Enlarge text</button>
</template>

```
