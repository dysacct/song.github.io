---
title: ref标签隔离
date: 2025-08-31 14:29:28
categories: [vue]
---

# ref 标签隔离

### 示例

- person.vue

```vue
<script setup lang="ts">
import { ref } from "vue";
let title = ref(); // 用于存储ref标签内容
function shouLog() {
  console.log(title.value);
}
</script>

<template>
  <div class="person">
    <h1 ref="title">中国</h1>
    <button @click="shouLog">输出h1标签</button>
  </div>
</template>

<style scoped>
.person {
  background: #ccc;
  padding: 20px 100px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  /* margin: auto; */
}
</style>
```

- App.vue

```vue
<script setup lang="ts" name="App">
//name是给组件起一个名字，方便调试
import person from "./components/person.vue";
import { ref } from "vue";

let title = ref();
function shouLog() {
  console.log(title.value);
}
</script>

<template>
  <div class="app">
    <h1 ref="title">你好</h1>
    <button @click="shouLog">打印</button>
  </div>
  <person />
</template>

<style scoped>
.app {
  background: #ccc;
  padding: 20px 100px;
  box-shadow: 0 0 10px skyblue;
  border-radius: 10px;
  /* margin: auto; */
}
</style>
```
