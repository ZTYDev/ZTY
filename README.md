# Zest To You

Zest To You（简称 ZTY）是一款基于 Tauri 2 + Vue 3 开发的 Windows 桌面 Minecraft 启动器，覆盖版本安装、账号登录、模组管理、游戏启动与启动器个性化等日常使用场景。

- **当前版本**：3.8.7（见 `core/tauri.conf.json`）
- **应用标识**：`com.zty.launcher`
- **目标平台**：Windows 10 及以上（64 位）

---

## 主要功能

以下功能均对应仓库中已有的前端页面或后端命令，未完成或仍需配置的能力已单独注明。

### 版本与启动

- 从 Mojang 官方版本清单获取版本，支持原版下载、本地版本扫描与删除
- 安装 Forge / NeoForge / Fabric / Quilt 加载器；Forge / NeoForge 使用启动器内置的 processors 流程生成补丁，不依赖官方 installer
- 通过官方安装器启动 OptiFine 安装流程，并自动识别安装结果
- 版本复制、版本导出 / 导入、收藏与置顶、显示名称与备注
- 版本隔离：开启后该版本拥有独立的存档、模组与配置目录
- 逐版本设置 Java、内存、窗口尺寸 / 全屏、自动进服地址、启动前命令
- 启动前完整性校验（可在设置中关闭）、启动前自动备份存档
- 自动扫描并推荐 Java，缺失时可一键下载；启动过程分阶段显示进度并支持取消
- 进程资源监控、崩溃报告、启动历史记录

### 账号与皮肤

- 微软账号登录：OAuth 设备码流程与浏览器流程，令牌自动刷新
- 离线账号：自动生成 UUID
- 外置登录：LittleSkin、Yggdrasil（authlib-injector）
- 多账号保存、切换、删除与令牌刷新
- 离线皮肤导入、分配、头像与 3D 皮肤预览

### 模组与游戏资源

- 本地模组启用 / 禁用 / 删除，依赖校验与冲突检测
- 模组预设：保存、应用、删除一套模组组合
- 模组更新检测与批量更新
- Modrinth 在线搜索、详情、下载与自动安装依赖；配置 CurseForge API Key 后可使用 CurseForge 源
- 中文关键词搜索时自动翻译为英文
- 整合包导入 / 导出（zip / mrpack），支持拖拽导入
- 资源包、光影包、数据包管理
- 世界存档管理、收藏、游玩次数统计、导入导出
- 存档备份与还原，支持启动前自动备份及保留份数设置
- 服务器列表（`servers.dat`）管理

### 联机

- ZTY-Link 联机：创建 / 加入房间、P2P 打洞、内置信令与中继服务、局域网 IP 展示、NAT 类型检测
- 联机模块仍在持续迭代，不同网络环境下的连通性尚不稳定，暂不建议作为生产级联机方案使用
- 仓库内附带 `relay-server`（UDP 信令 / TCP 中继）与 `zty-relay`（WebSocket 中继）两个独立服务，可用于自建

### 界面与辅助功能

- 明暗主题与跟随系统、自定义背景图 / 背景视频
- 背景音乐播放器：仅支持 MP3，支持随机 / 循环 / 列表三种模式与音量调节
- 悬浮工具箱：全局快捷键唤起，显示 FPS 与游玩计时，支持截图
- 下载悬浮窗：跨页面展示下载 / 安装任务进度
- 日志页面：级别过滤、自动刷新、导出与清空
- 截图管理：缩略图浏览、系统打开、删除
- 内置 Minecraft Wiki 页面（Tauri 子 WebView）
- 离线帮助文档
- 回声洞：本地留言板（发布、点赞、删除，数据保存在本地）
- AI 助手与崩溃日志分析：需自行配置 OpenAI 兼容接口的地址、API Key 与模型名
- 启动器更新检查：在设置中填写 GitHub 仓库 `owner/repo` 后检查 Release，下载安装包后由用户手动安装
- 配置管理：均衡 / 高性能 / 省电预设切换，配置导入导出、自动备份与恢复、一键重置

---

## 技术栈与运行环境

| 层次 | 技术 |
| --- | --- |
| 后端 | Rust 2021 edition、Tauri 2、tokio、reqwest、rusqlite（bundled）、serde、tracing、parking_lot |
| 前端 | Vue 3、TypeScript、Vite 5、Pinia 2、Vue Router 4（hash 模式） |
| UI | PrimeVue 3、PrimeFlex 3、PrimeIcons 6、Tailwind CSS 3、skinview3d + three.js |
| 扩展能力 | tokio-tungstenite、kcp、igd-next（UPnP）、aes-gcm / hmac / pbkdf2 / ed25519-dalek、zip / tar / zstd / flate2 |

### 环境要求

- **操作系统**：Windows 10 / 11（64 位）
- **Node.js**：18 及以上（建议 20 LTS）
- **Rust**：稳定版工具链（建议 1.75 及以上）
- **Visual Studio Build Tools**：需勾选“使用 C++ 的桌面开发”并安装 Windows SDK
- **WebView2 Runtime**：Windows 10/11 通常已内置

---

## 安装依赖与启动

### 1. 安装依赖

在项目根目录执行：

```bash
npm install
```

根目录的 `package.json` 提供聚合脚本与 Tauri CLI；前端依赖位于 `ui` 子目录，需要单独安装：

```bash
cd ui
npm install
```

### 2. 开发模式

启动完整的 Tauri 应用（前端 + Rust 后端）：

```bash
npm run tauri:dev
```

也可以直接双击根目录的 `启动.bat`，效果相同。`npm run tauri:dev` 会先调用 `msvc-env.bat` 初始化 MSVC 编译环境，再在 `core` 目录启动 Tauri。

仅启动前端页面，不启动 Tauri 外壳：

```bash
npm run dev
```

或运行 `dev-ui.bat`。前端开发服务器固定监听 `http://localhost:5173`，与 `core/tauri.conf.json` 中的 `devUrl` 一致。

### 3. 编译检查

```bash
cd core
cargo check
```

或运行 `test_compile.bat`（会自动初始化 MSVC 环境）。

---

## 构建与打包

只构建前端（产物在 `ui/dist`）：

```bash
npm run build
```

完整打包（配置为 NSIS 安装包）：

```bash
npm run tauri:build
```

也可以使用根目录脚本：

- `build.bat`：初始化编译环境 → 构建前端 → 执行 `cargo tauri build`
- `打包.bat`：交互式打包器，可选择编译线程（1 / 2 / 4）、打包模式（正式 / 调试 / NSIS 便携版）以及是否修改版本号。脚本会检查管理员权限，需以管理员身份运行

构建产物位置：

| 产物 | 路径 |
| --- | --- |
| 可执行文件 | `core/target/release/zest-to-you.exe` |
| 安装包 | `core/target/release/bundle/` |

---

## 目录结构

```
Zest To You/
├── core/                      # Rust + Tauri 后端
│   ├── src/
│   │   ├── auth/              # 微软、Yggdrasil、LittleSkin 认证
│   │   ├── commands/          # Tauri 命令（前端 invoke 入口）
│   │   ├── managers/          # 业务逻辑：下载、启动、模组、联机、加载器安装等
│   │   ├── models/            # 数据模型与配置结构
│   │   ├── database/          # 数据库迁移
│   │   ├── utils/             # 日志、路径等工具
│   │   ├── lib.rs             # 应用初始化与命令注册
│   │   └── main.rs
│   ├── tauri.conf.json        # 生产环境 Tauri 配置
│   └── tauri.conf.dev.json    # 开发环境覆盖配置
├── ui/                        # Vue 3 前端
│   ├── src/
│   │   ├── api/               # Tauri invoke 封装
│   │   ├── components/        # 主布局、游戏启动器、悬浮窗、关于页组件
│   │   ├── composables/       # 组合式函数
│   │   ├── data/              # 静态数据
│   │   ├── router/            # 路由配置
│   │   ├── stores/            # Pinia 状态
│   │   ├── utils/             # 主题、背景视频、开发者模式等
│   │   └── views/             # 各功能页面
│   ├── overlay.html           # 悬浮工具箱入口
│   └── vite.config.ts
├── relay-server/              # ZTY 联机信令 / 中继服务（Rust）
├── zty-relay/                 # WebSocket 中继服务（Rust）
├── msvc-env.bat               # 自动查找并初始化 vcvars64.bat
├── 启动.bat                   # 开发模式启动
├── dev-ui.bat                 # 仅启动前端
├── build.bat                  # 基础构建
├── 打包.bat                   # 交互式打包
└── test_compile.bat           # Rust 编译检查
```

运行时的配置、版本、模组、日志、数据库等数据统一保存在启动器可执行文件同级的 `ZTY` 目录中，具体说明见 [ZTY目录说明.md](ZTY目录说明.md)。

---

## 常见问题

### 编译时报 `linker 'link.exe' not found`

说明 MSVC 编译环境未初始化或未安装。请安装 Visual Studio Build Tools，勾选“使用 C++ 的桌面开发”和 Windows SDK，然后通过 `npm run tauri:dev`、`test_compile.bat` 或 `打包.bat` 启动，这些脚本会自动调用 `msvc-env.bat` 查找 `vcvars64.bat`。

### 前端启动失败，提示 5173 端口被占用

`ui/vite.config.ts` 中设置了 `strictPort: true`，端口被占用时不会自动切换。请释放 5173 端口；如需更换端口，需同时修改 `ui/vite.config.ts` 的 `server.port` 与 `core/tauri.conf.json` 的 `build.devUrl`。

### 低内存机器打包时编译中断或系统不稳定

编译 `windows` 相关 crate 时内存占用较高。请使用 `打包.bat` 并选择 1 线程，脚本会设置 `CARGO_BUILD_JOBS`、`RUST_MIN_STACK=16777216` 等环境变量。

### 修改 `tauri.conf.json` 后打包失败

该文件必须保存为**无 BOM 的 UTF-8**。使用部分编辑器或 PowerShell 默认编码写入可能引入 BOM，导致 Tauri 解析失败。`打包.bat` 在修改版本号时已按无 BOM 方式写入。

### 联机功能连不上

ZTY-Link 仍在迭代，P2P 打洞成功率受 NAT 类型与网络环境影响。可以尝试使用内置信令 / 中继服务，或在同一局域网内连接；自建服务可参考 `relay-server` 与 `zty-relay`。

### AI 助手或崩溃分析不可用

这两项功能依赖外部模型服务，需要在设置中填写 OpenAI 兼容接口地址、API Key 与模型名。

---

## 开源状态

本项目目前**暂未开源**。

项目仍处于持续开发迭代阶段，功能、数据结构与命令接口尚未稳定，整体完成度和稳定性还不足以对外正式发布，因此暂不公开源代码。待核心功能稳定、代码结构与文档完善后，计划再行开源。

在正式开源前，未经作者明确许可，请勿复制、修改、分发本项目代码或将其用于商业用途。

---

## 版权声明

Copyright © 2026 ZTY. All rights reserved.

本项目及其全部源代码、文档与资源的著作权归作者所有。在开源前保留所有权利，不适用任何开源许可证。
