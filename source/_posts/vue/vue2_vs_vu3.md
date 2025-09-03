---
title: Vue2 vs Vue3
date: 2025-08-31 11:48:25
categories:
  - Vue
---

# Vue 2 → Vue 3 迁移手册（Vite 版）

> 目标：把一个 Vue 2 项目（多半基于 webpack/CLI）迁到 Vue 3 + Vite，形成一份可复用的清单。文档按“先搭骨架 → 再替换语法 → 最后清扫”的顺序组织，配有对照、示例与常见坑。

---

## 0. 一句话总览（Checklist）

1. **创建新骨架**：用 Vite 初始化 Vue 3 项目 → 复制业务代码与静态资源。
2. **替换生态**：`vue-router@4`、状态管理推荐 **Pinia**（或 `vuex@4` 保持 API 兼容）、`@vue/test-utils@2`、图标/组件库升级到 Vue 3 版。
3. **全局 API 改造**：`new Vue()` → `createApp()`；`Vue.use()` → `app.use()`；`Vue.prototype` → `app.config.globalProperties`；**过滤器（filters）移除**。
4. **模板与语法**：`v-model`、插槽、事件与 `emits`、异步组件、`Teleport`/`Suspense`、`<script setup>`。
5. **Composition API 上车**：`setup()`、`ref/reactive`、`computed`、`watch`、生命周期钩子对照表。
6. **构建与资源**：Vite 配置、别名、环境变量、CSS 预处理、SVG、图片与静态资源、按需引入。
7. **类型与质量**：TypeScript（可选）、ESLint+Prettier、测试改造。
8. **收尾**：删除废弃 API、兼容性确认、生产部署与性能体检。

---

## 1. 初始化与目录结构

### 1.1 用 Vite 创建项目

```bash
# Node 版本建议 ≥ 18
npm create vite@latest my-app -- --template vue-ts   # TS 模板（推荐）
# 或：--template vue  走 JS 模板
cd my-app
npm install
npm run dev
```

### 1.2 目录差异对照

- **Vue CLI（webpack）**：`src/main.js` + `public/index.html` + `vue.config.js`；环境文件 `.env.*`；`assets` 由 webpack 处理。
- **Vite**：零配置即开箱，入口 `index.html` 在项目根；配置文件 `vite.config.ts`；静态资源由原生 ESM 处理，`public/` 下资源原样拷贝。

> 迁移策略：保留 Vue 3 + Vite 新项目结构，将旧项目 `src/` 的 **业务代码** 分模块迁入；第三方库先留空，逐个替换。

---

## 2. 生态版本与依赖

### 2.1 核心依赖

```json
{
  "dependencies": {
    "vue": "^3.x",
    "vue-router": "^4.x",
    "pinia": "^2.x" // 或使用 vuex@4 保持风格
  },
  "devDependencies": {
    "vite": "^5.x",
    "@vitejs/plugin-vue": "^5.x",
    "typescript": "^5.x" // 若用 TS
  }
}
```

- **依赖安装位置**：全部在项目根目录的 `node_modules/`，版本记录在 `package.json` 与 `package-lock.json`/`pnpm-lock.yaml`。

### 2.2 生态替换速查

| 分类   | Vue 2                         | Vue 3（Vite）                                      |
| ------ | ----------------------------- | -------------------------------------------------- |
| 路由   | `vue-router@3`                | `vue-router@4`（路由表语法小差异，`createRouter`） |
| 状态   | `vuex@3`                      | **Pinia**（推荐）或 `vuex@4`                       |
| 测试   | `@vue/test-utils@1`           | `@vue/test-utils@2`                                |
| 组件库 | Element UI, iView, …          | Element Plus, Naive UI, Arco, Ant Design Vue\@3    |
| 图标   | `vue-awesome`, `font-awesome` | `unplugin-icons`、`@vicons/*`、`Iconify`           |

---

## 3. 全局启动方式与 API 变更

### 3.1 启动方式

**Vue 2**

```js
// main.js
import Vue from "vue";
import App from "./App.vue";
new Vue({ render: (h) => h(App) }).$mount("#app");
```

**Vue 3**

```js
// main.ts
import { createApp } from "vue";
import App from "./App.vue";
createApp(App).mount("#app");
```

### 3.2 插件与全局对象

- `Vue.use(plugin)` → `app.use(plugin)`
- `Vue.prototype.$xxx` → `app.config.globalProperties.$xxx`
- `Vue.mixin()` 仍可用，但推荐以 **composable** 取代（见 §5.4）。
- **Filters 移除**：模板内 `{{ msg | upper }}` 不再支持。替代：计算属性或全局方法。

### 3.3 生产提示与调试

```ts
const app = createApp(App);
app.config.errorHandler = (err, vm, info) => {
  /* ... */
};
app.config.performance = import.meta.env.DEV;
```

---

## 4. 模板与指令变化

### 4.1 `v-model`

- **Vue 2**：组件自定义 `v-model` 依赖 `value` + `input` 事件。
- **Vue 3**：统一为 `modelValue` + `update:modelValue`，支持多 `v-model`。

```vue
<!-- 父组件使用 -->
<MyInput v-model="username" />
<!-- 子组件 props/emit -->
<script setup lang="ts">
const props = defineProps<{ modelValue: string }>();
const emit = defineEmits<{ (e: "update:modelValue", v: string): void }>();
function onInput(v: string) {
  emit("update:modelValue", v);
}
</script>
```

### 4.2 事件与 `emits`

- 新增 `emits` 显式声明，提高类型与可维护性。

```ts
const emit = defineEmits<{
  (e: "save"): void;
  (e: "change", id: number): void;
}>();
```

### 4.3 插槽

- 具名与作用域插槽语法更统一：`<template #name="{ data }">`。

### 4.4 异步组件

```ts
import { defineAsyncComponent } from "vue";
const Foo = defineAsyncComponent(() => import("./Foo.vue"));
```

### 4.5 新内置：`Teleport` / `Suspense`

- `Teleport`：将子内容渲染到 body/任意容器。
- `Suspense`：为异步组件提供加载/兜底 UI（配合 `<script setup async>`）。

---

## 5. Composition API 与 `<script setup>`

### 5.1 生命周期对照

| Vue 2                    | Vue 3                |
| ------------------------ | -------------------- |
| `beforeCreate`/`created` | **合并进 `setup()`** |
| `beforeMount`            | `onBeforeMount`      |
| `mounted`                | `onMounted`          |
| `beforeUpdate`           | `onBeforeUpdate`     |
| `updated`                | `onUpdated`          |
| `beforeDestroy`          | `onBeforeUnmount`    |
| `destroyed`              | `onUnmounted`        |

### 5.2 响应式基础

```ts
import { ref, reactive, computed, watch } from "vue";
const count = ref(0);
const state = reactive({ items: [] as string[] });
const double = computed(() => count.value * 2);
watch(
  () => state.items.length,
  (n) => console.log(n)
);
```

### 5.3 `<script setup>` 极简范式

```vue
<script setup lang="ts">
import { ref } from "vue";
const msg = ref("hello");
function greet() {
  msg.value = "hi";
}
</script>
<template>
  <button @click="greet">{{ msg }}</button>
</template>
```

- 自动 import `defineProps/defineEmits/defineExpose`；更少样板代码。

### 5.4 从 Mixins 迁到 Composables

- 将通用逻辑写成函数模块：`useXxx()`，在多处复用且避免命名冲突。

```ts
// useFetch.ts
export function useFetch<T>(url: string) {
  /* ...返回 data/error/loading */
}
```

---

## 6. 路由、状态与请求

### 6.1 `vue-router@4`

```ts
import { createRouter, createWebHistory } from "vue-router";
import Home from "@/views/Home.vue";
const routes = [{ path: "/", component: Home }];
export const router = createRouter({ history: createWebHistory(), routes });
// main.ts
createApp(App).use(router).mount("#app");
```

- `beforeRouteEnter` 等路由守卫用法基本一致；元信息/懒加载保持原思路。

### 6.2 状态：Pinia（推荐）

```ts
// stores/user.ts
import { defineStore } from "pinia";
export const useUser = defineStore("user", {
  state: () => ({ name: "" }),
  actions: {
    setName(n: string) {
      this.name = n;
    },
  },
});
```

- 若沿用 Vuex：升级到 `vuex@4`，API 与 Vue 2 相近。

### 6.3 请求：Axios 等库原封不动；建议以 **插件** 注入到 `app.config.globalProperties` 或以 composable 封装。

---

## 7. Vite 配置与资源处理

### 7.1 基础配置（`vite.config.ts`）

```ts
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import path from "path";
export default defineConfig({
  plugins: [vue()],
  resolve: { alias: { "@": path.resolve(__dirname, "src") } },
  server: { port: 5173, open: true },
  build: { sourcemap: false, outDir: "dist" },
});
```

### 7.2 环境变量

- 文件命名：`.env`、`.env.development`、`.env.production`
- 访问：`import.meta.env.VITE_API_BASE`

### 7.3 CSS 与预处理器

- 直接安装 `sass/less/stylus` 即可使用；全局样式可在 `vite.config.ts` 使用 `css.preprocessorOptions` 注入变量。

### 7.4 资源与 SVG

- `public/` 下资源以绝对路径 `/xxx` 引用；
- 导入式资源：`import logo from '@/assets/logo.png'`；
- SVG 建议用 `vite-svg-loader` 或 `unplugin-icons` 进行组件化。

---

## 8. TypeScript 与校验/测试

### 8.1 TS 配置

- `tsconfig.json`：`"moduleResolution": "bundler"`（Vite 5+），`"types": ["vite/client"]`。

### 8.2 ESLint + Prettier

- 使用 `@vue/eslint-config-typescript`、`@vue/eslint-config-prettier`；Vite 不做代码质量，交给 ESLint。

### 8.3 测试

- 单元测试：`vitest` + `@vue/test-utils@2`；
- 端到端：Playwright/Cypress。

---

## 9. 常见坑与对照

1. **过滤器不再支持** → 用计算属性/方法取代；或注册全局函数。
2. **自定义 `v-model` 名称变更** → `value/input` → `modelValue/update:modelValue`。
3. **`this` 不可用**（在 `setup` 中） → 使用闭包变量与 `ref/reactive`。
4. **全局事件总线**（`new Vue()` 作为 bus）→ 使用 `mitt` 或基于 `pinia`/路由通信。
5. **`$listeners/$attrs` 变化** → 使用 `useAttrs()`、`emits` 与 `defineProps`。
6. **`scopedSlots` 改名** → 统一为 `slots`；模板中使用 `#` 语法。
7. **异步组件写法**：`Vue.component('Async', () => import(...))` → `defineAsyncComponent`。
8. **`v-if` 与 `v-for` 同层**：保持先后顺序，尽量避免同一元素并用；必要时包一层。
9. **IE 不再支持**：如需旧浏览器兼容，使用现代构建 + 条件降级方案。
10. **第三方库不兼容**：优先寻找 Vue 3 分支或替代品；无法替换时考虑保留微前端/iframe 隔离。

---

## 10. 迁移示例（组件级）

### 10.1 `v-model` 组件从 Vue 2 → Vue 3

**Vue 2**（节选）

```vue
<template>
  <input :value="value" @input="$emit('input', $event.target.value)" />
</template>
<script>
export default { props: { value: String } };
</script>
```

**Vue 3**

```vue
<template>
  <input
    :value="modelValue"
    @input="(e) => emit('update:modelValue', e.target.value)" />
</template>
<script setup lang="ts">
const props = defineProps<{ modelValue: string }>();
const emit = defineEmits<{ (e: "update:modelValue", v: string): void }>();
</script>
```

### 10.2 Mixins → Composable

**Vue 2 Mixin**

```js
export default {
  created() {
    this.fetch();
  },
  methods: { async fetch() {} },
};
```

**Vue 3 Composable**

```ts
export function useLoad() {
  const loading = ref(false);
  async function fetch() {
    loading.value = true;
    /* ... */ loading.value = false;
  }
  onMounted(fetch);
  return { loading, fetch };
}
```

---

## 11. 打包与部署（Vite）

```bash
npm run build   # 产物默认在 dist/
```

- 默认生成 **原生 ESM** 静态资源，部署到任意静态服务器（Nginx/Netlify/Vercel）即可。
- 若有 **后端接口**：

  - 开发期用 `server.proxy` 解决跨域；
  - 生产期由网关/Nginx 反向代理。

---

## 12. YAML/CI（可选）

- **GitHub Actions**：构建脚本样例

```yaml
name: build
on: [push]
jobs:
  web:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20 }
      - run: npm ci
      - run: npm run build
      - uses: actions/upload-artifact@v4
        with: { name: dist, path: dist }
```

---

## 13. 迁移节奏建议

1. **先能跑**：空壳 + 路由 + 首页。
2. **先公共后业务**：先迁公共组件与基础设施（路由、状态、请求封装），再迁页面功能。
3. **边迁边对照**：每迁完一个模块就编译、路由走一遍、记录改造点。
4. **留后手**：对不兼容的第三方包可以先隔离或临时降级，完成功能再替换。

---

## 14. 附录：关键对照表

### 14.1 全局 API 对照

| Vue 2                | Vue 3                              |
| -------------------- | ---------------------------------- |
| `new Vue()`          | `createApp()`                      |
| `Vue.use()`          | `app.use()`                        |
| `Vue.mixin()`        | `app.mixin()`                      |
| `Vue.directive()`    | `app.directive()`                  |
| `Vue.component()`    | `app.component()`                  |
| `Vue.prototype.$xxx` | `app.config.globalProperties.$xxx` |

### 14.2 生命周期钩子对照（再次汇总）

`created` → `setup()`；`mounted` → `onMounted`；`destroyed` → `onUnmounted` 等。

---

### 结束语

迁移并不神秘：**建新骨架 → 按清单替换 → 逐步验证**。这份文档可做你团队的“过线标准”，每次迁完一个模块就对照打勾，稳扎稳打。
