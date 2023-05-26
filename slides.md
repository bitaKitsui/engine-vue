---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## Slidev Starter Template
  Presentation slides for developers.

  Learn more at [Sli.dev](https://sli.dev)
drawings:
  persist: false
transition: slide-left
css: unocss
title: Welcome to Slidev
---

# Vue のアレコレ

---

# 自己紹介

<div style="display: flex; justify-content: space-between">
<div style="display: flex; flex-direction: column">
<h4>橘井 貴輝（PD9U - FE）</h4>
<ul>
<li>ギター部入ってます</li>
<li>サウスのヒップホップにお熱</li>
</ul>
</div>

<img src="usagi.png" alt="" style="height: 440px">
</div>


---

# Agenda

- Vue 3 の変遷
- 2系との大きな違い
- 困ること

---

# 案件でやっている（やった）こと

- Vue 3 + TypeScript
- Vue 2 + JavaScript
  - Vue 3 へのマイグレーション

---

# Steve Lacy

<div style="display: flex; gap: 40px">
<div>
<img alt="steve-lacy" src="steve-lacy.jpeg" style="width: 400px" />
</div>
<div>
<img alt="dark-red" src="dark-red.jpeg" style="width: 200px" />
<img alt="hive-mind" src="hive-mind.jpg" style="width: 200px" />
</div>
</div>

---

# Vue 3 の大まかな変遷

- 2020/09/19: v3.0.0 One Piece
  - 「誰でもすぐに習得できる親しみやすいフレームワーク」から「プログレッシブ・フレームワーク」へ
  - ユーザーが増えたことで、新たに増加した需要に対応しうる柔軟性
- 2021/08/05: v3.2.0 Quintessential Quintuplets
  - 多くの重要な新機能が搭載
- 2023/05/11: v3.3.0 Rurouni Kenshin

---

# 大きな違い

Options API => Composition API

Options API の場合

```vue
<script>
import { defineComponent } from 'vue';
export default defineComponent({
  data() {
    return {
      count: 0,
    };
  },
  methods: {
    increment() {
      this.count++;
    },
    decrement() {
      this.count--;
    },
  },
});
</script>
```

---

# 大きな違い

Options API => Composition API

Composition API の場合

```vue
<script>
import { defineComponent, ref } from 'vue';
export default defineComponent({
  setup() {
    const count = ref(0);
    const increment = () => {
      count.value++;
    };
    const decrement = () => {
      count.value--;
    };
    return { count, increment, decrement };
  },
});
</script>
```

---

# setup とは

- Vue コンポーネント生成時の一番初めに実行される
  - Options API 初期化よりも先
  - 一度だけ実行される
- オブジェクトの形で `template` タグへ公開
- コンポーネントインスタンス - `this` へアクセスできない

---

# script setup 構文

Composition API の糖衣構文

- Vue 特有の構文が減少
    - `export default` で Vue のオブジェクトを export する必要がない
    - オブジェクトの形で return する必要がない

```vue
<script setup>
import { ref } from 'vue';

const count = ref(0);
const increment = () => {
  count.value++;
};
const decrement = () => {
  count.value--;
};
</script>
```

---

# Composables

- リアクティブなロジックを再利用できる関数
  - React でいう Custom Hooks
  - 怪しい mixins からオサラバできる

```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

---

<div style="display: grid; place-items: center; height: 100%">
<p style="font-size: 1.85rem">「プログレッシブ・フレームワーク」として恥じぬ動きだ🐥</p>
</div>

---

# 困ること

- マイナーアップデートの影響が大きい
  - 後から「こっちの方がよかったので」という変更や追加が多い

```js
// BEFORE
const emit = defineEmits<{
  (event: 'foo', id: number): void
  (event: 'bar', name: string, ...rest: any[]): void
}>()

// AFTER
const emit = defineEmits<{
  foo: [id: number]
  bar: [name: string, ...rest: any[]]
}>()
```

---

# 困ること

- TypeScript 対応
  - `.vue` という独自のフォーマットに制限されている部分がある

```vue
// 3.3 でようやくできるようになった
<script setup lang="ts">
import type { Props } from './foo'

// imported + intersection type
defineProps<Props & { extraProp?: string }>()
</script>

<script setup lang="ts" generic="T">
defineProps<{
  items: T[]
  selected: T
}>()
</script>
```


