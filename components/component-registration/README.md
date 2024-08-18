## component registration - コンポーネント登録

https://ja.vuejs.org/guide/components/registration

### グローバル登録

#### 開発中のアプリケーション

```javascript
import { createApp } from "vue"

const app = createApp({})

app.component({
  // 登録名
  "MyComponent",
  {
    // 実装
  }
})
```

#### SFC

```javascript
import { createApp } from "vue"
import MyComponent from "./App.vue"

const app = createApp({})

app.component("MyComponent", MyComponent)
```

`.component`はメソッドチェーンにできます。

```javascript
app
  .component('ComponentA', ComponentA)
  .component('ComponentB', ComponentB)
  .component('ComponentC', ComponentC)
```

グローバル登録したコンポーネントは、アプリケーション内の任意のコンポーネントのテンプレートで使用できます:

```vue
<ComponentA />
<ComponentB />
<ComponentC />
```

#### グローバル登録のデメリット

- グローバル登録では、未使用のコンポーネントを削除してくれるビルドシステムの処理（いわゆる「ツリーシェイク」）が阻害されます。グローバル登録したコンポーネントは、最後までアプリのどこにも用いなかった場合でも、最終的なバンドルには含まれてしまいます。
- グローバル登録では、大規模なアプリケーションでの依存関係の分かりやすさが低下します。グローバル登録では、子コンポーネントを使っている親コンポーネントから、子コンポーネントの実装部分を探し出すことが難しくなります。きわめて多くのグローバル変数が使われている状況と同じように、これは長期的な保守性に影響を与える可能性があります。

### ローカル登録

#### SFC を `<script setup>` と共に使用する場合、インポートしたコンポーネントを登録なしでローカルに使用できるようになります:

```vue
<script setup>
import ComponentA from './ComponentA.vue'
</script>

<template>
  <ComponentA />
</template>
```

#### `<script setup>` を用いない場合は、以下のように components オプションを使用する必要があります:

```javascript
import ComponentA from './ComponentA.js'

export default {
  components: {
    ComponentA
  },
  setup() {
    // ...
  }
}
```

components オブジェクトのプロパティそれぞれについて、キーがコンポーネントの登録名になります。そして、値にコンポーネントの実装が保持されます。上の例では ES2015 のプロパティの省略記法を使っていて、これは次の表記と等価です:

```javascript
export default {
  components: {
    ComponentA: ComponentA
  }
  // ...
}
```

ただし、 ローカル登録されたコンポーネントが子孫のコンポーネントでも利用できるようにはならないことに注意してください。上の場合、ComponentA は現在のコンポーネントのみで利用可能になり、その子や子孫のコンポーネントで利用可能になるわけではありません。
