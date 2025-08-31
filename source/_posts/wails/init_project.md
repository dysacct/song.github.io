---
title: Idrac-console-gui 项目架构
date: 2025-08-26 10:00:00
categories:
  - wails
---

# 导航
- [一、环境准备/检查](#一环境准备检查)
- [二、初始化「Go + Wails + Vue3 + TypeScript」项目](#二初始化go-wails-vue3-typescript项目)
- [三、安装前端依赖（含 Axios）](#三安装前端依赖含-axios)
- [四、运行开发环境](#四运行开发环境)
- [五、项目架构](#五项目架构)
- [六、后端：用 Go 暴露方法给前端（Wails “桥”）](#六后端用-go-暴露方法给前端wails桥)
- [七、前端：在 Vue 里调用 Go 方法 + 使用 Axios](#七前端在-vue-里调用-go-方法使用-axios)
- [八、路由与页面（可选但强烈建议）](#八路由与页面可选但强烈建议)

## 项目架构
/project-root
  ├─ frontend/            # 前端（vite + vue3）
  │   ├─ src/
  │   │   ├─ assets/
  │   │   │   └─ images/  # 你的 svg 放这里（例如: idrac.svg、dell.svg、huawei.svg、langchao.svg、log.svg）
  │   │   ├─ components/
  │   │   └─ App.vue
  │   └─ package.json
  └─ backend(go)/...

### 一、环境准备/检查
1. 安装/检查 Go 与 Node

Go ≥ 1.22（go version）

Node ≥ 18（node -v / npm -v）

2. 安装 Wails v2 CLI
```bash
go install github.com/wailsapp/wails/v2/cmd/wails@latest
wails doctor
```

`wails doctor` 会检查前端打包器、WebView2 等是否齐全。官方快速开始 & 模板说明参考这里（确认 Vue 模板可用）：[wails](https://wails.io/docs/gettingstarted/firstproject/?utm_source=chatgpt.com) [go packages](https://pkg.go.dev/github.com/wailsapp/wails/v2/pkg/templates/templates/vue-ts?utm_source=chatgpt.com)



### 二、初始化「Go + Wails + Vue3 + TypeScript」项目

> 我们直接用官方 Vue-TS 模板，最省心。
```bash
# 在你想放项目的目录执行
wails init -n idrac-console-ui -t vue-ts
cd idrac-console-gui
```

>说明

>- 这条命令会生成一个包含 Go 后端 + Vue3 前端的完整骨架。

>- 之所以选 vue-ts，是因为 TypeScript 对新手更安全（有类型提示）。

>- Wails 官方文档确认提供 Vue 模板与初始化流程。

>- [Wails](https://wails.io/docs/gettingstarted/firstproject/?utm_source=chatgpt.com)


### 三、安装前端依赖（含 Axios）
进入项目根目录（里面有 frontend/ 与 go.mod）：
```bash
cd frontend
npm i                 # 安装模板自带依赖
npm i axios           # 安装 axios
# 新手友好建议：可加路由+状态管理（可选）
npm i vue-router pinia
cd idrac-console-ui/frontend && npm install typescript@^5.0.0 --save-dev # 如果安装失败，可以尝试使用npm安装更新ts版本
# vite vue不支持直接引入svg，需要安装vite-svg-loader
npm install vite-svg-loader -D
cd ..
```
>依赖存放位置：

> - 前端依赖：frontend/node_modules

> - Go 依赖：模块缓存（Linux/macOS 通常在 ~/go/pkg/mod；Windows 通常在 %USERPROFILE%\go\pkg\mod），项目本地记录在 go.mod/go.sum。

> - Wails 生成的前端绑定代码（前端调用 Go 的“桥”）：frontend/wailsjs/*（运行 wails dev 或 wails build 后会生成/更新）。

### 四、运行开发环境
```bash
wails dev
```
- 这会同时跑起 Go 后端与前端开发服务器，并把 Vue 打进桌面窗口。

- 首次运行会生成 frontend/wailsjs 目录，里面是 TypeScript 的“桥接”代码（如何从 Vue 调 Go 方法）。这点在 Wails 模板/项目生成文档里也有说明。


### 项目架构
```bash
idrac-console-gui/
├─ go.mod / go.sum                # Go 依赖
├─ main.go                        # 程序入口，注册窗口、绑定后端对象
├─ app/                           # (建议) 放你的 Go 业务逻辑
│  └─ app.go                      # 暴露给前端调用的方法
├─ wails.json                     # Wails 项目配置
└─ frontend/                      # Vue3 + TS 前端
   ├─ package.json
   ├─ node_modules/               # 前端依赖安装目录
   ├─ src/
   │  ├─ main.ts                  # Vue 应用入口
   │  ├─ App.vue                  # 根组件
   │  ├─ api/axios.ts             # （我们要新建）Axios 实例
   │  ├─ pages/...                # 你的页面
   │  ├─ router/index.ts          # （可选）Vue Router
   │  └─ stores/...               # （可选）Pinia
   └─ wailsjs/                    # 自动生成的 Go 调用桥
      └─ go/main/App.ts
```

### 六、后端：用 Go 暴露方法给前端（Wails “桥”）
创建 `app/app.go`（如果目录不存在就建一个），示例：
```go
// 文件: app/app.go
package app

import "context"

// App 是暴露给前端的对象（注意: 导出字段/方法需大写）
type App struct {
    ctx context.Context
}

// NewApp 创建实例（Wails 会在 main.go 里用到）
func NewApp() *App {
    return &App{}
}

// Startup 在应用启动时被调用，可保存上下文
func (a *App) Startup(ctx context.Context) {
    a.ctx = ctx
}

// Greet 示例：前端可通过 wailsjs 调用这个方法
func (a *App) Greet(name string) string {
    if name == "" {
        name = "朋友"
    }
    return "你好，" + name + "！这是来自 Go 后端的问候。"
}
```
`main.go` 里把这个对象挂到 Wails 应用上（模板已有类似代码，我们补齐关键点）：
```go
// 文件: main.go（示例关键片段）
package main

import (
    "embed"
    "log"

    "github.com/wailsapp/wails/v2"
    "github.com/wailsapp/wails/v2/pkg/options"
    "idrac-console-ui/app" // 引入你刚创建的包
)

//go:embed frontend/dist
var assets embed.FS

func main() {
    myApp := app.NewApp()

    err := wails.Run(&options.App{
        Title:     "idrac-console-ui",
        Width:     1200,
        Height:    800,
        AssetServer: &options.AssetServer{
            Assets: assets,
        },
        OnStartup: myApp.Startup,
        Bind:      []interface{}{ myApp }, // 绑定到前端
    })
    if err != nil {
        log.Fatal(err)
    }
}
```

### 七、前端：在 Vue 里调用 Go 方法 + 使用 Axios
1.新建一个 Axios 实例（frontend/src/api/axios.ts）：
```ts
// 文件: frontend/src/api/axios.ts
// 作用：集中创建和配置 Axios 实例（统一 baseURL、超时、拦截器）
import axios from "axios";

const api = axios.create({
  // 如果你要访问外部 REST 服务，配置它的地址
  baseURL: "http://127.0.0.1:8080", // 示例：外部服务地址（不是 Wails 内置 Go）
  timeout: 10000,
});

// 请求拦截器（附加认证头等）
api.interceptors.request.use((config) => {
  // 示例：如果你有 token，可以这样加
  // const token = localStorage.getItem("token")
  // if (token) config.headers.Authorization = `Bearer ${token}`
  return config;
});

// 响应拦截器（统一错误弹窗/日志）
api.interceptors.response.use(
  (resp) => resp,
  (err) => {
    console.error("API Error:", err);
    // 这里可以根据 err.response?.status 做统一处理
    return Promise.reject(err);
  }
);

export default api;
```
2.在 Vue 组件里：既可以调用 Go，也可以调用 Axios
假设在 frontend/src/pages/Home.vue：
```vue
<script setup lang="ts">
// 1) 调用 Go 后端（通过 wailsjs 自动生成的桥）
// 路径以你的模块名为准：通常在 frontend/wailsjs/go/main/App.ts
import { Greet } from "@/wailsjs/go/main/App";

// 2) 调用外部 REST（通过 Axios）
import api from "@/src/api/axios";
import { ref } from "vue";

const hello = ref("");
const serverList = ref<any[]>([]);

// 从 Go 获取一段问候
async function callGoGreet() {
  hello.value = await Greet("zeromiss");
}

// 从外部 API 获取数据（只是示例）
async function fetchServers() {
  const { data } = await api.get("/api/servers");
  serverList.value = data;
}
</script>

<template>
  <div class="p-4">
    <h1>首页</h1>
    <button @click="callGoGreet">调用 Go: Greet</button>
    <p>{{ hello }}</p>

    <button @click="fetchServers">请求外部API: /api/servers</button>
    <pre>{{ serverList }}</pre>
  </div>
</template>

<style scoped>
.p-4 { padding: 16px; }
</style>
```
>重要理解：

> - 前端 ↔ Go（应用内）：通过 Wails 生成的 wailsjs，像调用本地函数一样。

> - 前端 ↔ 外部 REST（例如你将来要调 iDRAC 相关网关服务）：通过 Axios，走 HTTP 请求。

> 两者互不冲突，你可以把敏感逻辑放在 Go 里（更安全），Vue 负责界面和交互。

### 八、路由与页面（可选但强烈建议）
创建 frontend/src/router/index.ts：
```ts
import { createRouter, createWebHashHistory, RouteRecordRaw } from "vue-router";
import Home from "@/src/pages/Home.vue";

const routes: RouteRecordRaw[] = [
  { path: "/", name: "home", component: Home },
  // { path: "/idrac/:host", component: () => import("@/src/pages/IdracConsole.vue") }
];

export default createRouter({
  history: createWebHashHistory(), // 桌面应用用 hash 更省心
  routes,
});
```
在 frontend/src/main.ts 里挂上：
```ts
import { createApp } from "vue";
import App from "./App.vue";
import router from "./router";

createApp(App).use(router).mount("#app");
```