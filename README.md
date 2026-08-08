# 🐍 Ouroboros Vue

> **《圈一圈（Ouroboros）》**
>
> 2026 BOKE VIBE JAM 一等奖作品

一款以“衔尾蛇”为灵感设计的动作游戏。

玩家通过控制不断延伸的蛇身形成闭环，在循环中完成战斗与净化。游戏围绕“连接、循环与成长”的主题展开，将简单的环形机制转化为核心玩法体验。

本项目基于 **Vue 3 + Phaser + TypeScript** 开发，采用模块化架构设计，支持 Web、Windows 与 Android 多端运行。

## ✨ 游戏特色

- 🔄 独特的闭环战斗机制
- ⚔️ Boss 战与阶段式挑战
- 🎵 音频反馈与动画演出
- 🖥️ Web / Windows / Android 多端支持
- 👥 团队协作开发，完整工程化流程

---

## 🛠 技术栈

- Vue 3
- TypeScript
- Phaser
- Vite
- Electron
- Capacitor

## 启动

要求 Node.js `>=22.13.0`。

```bash
npm install
npm run dev
```

开发环境需要直接检查 Boss 时，可打开 `http://localhost:5173/?boss-preview=1`，点击开始后会跳过积分积累并进入噬环者入场流程。使用 `?boss-preview=telegraph` 可直接检查冲撞红闪，`?boss-preview=defeat` 可直接检查击败演出；这些参数只在 Vite 开发模式生效。

常用质量检查：

```bash
npm run check
npm test
npm run build
npm run verify
```

## 原生打包

Web 构建仍是唯一游戏产物，Electron 与 Capacitor 只负责装载 `dist/`，不会建立第二套玩法或 UI。

```bash
# macOS Apple Silicon .app
npm run package:mac

# Windows x64 单文件便携 .exe
npm run package:win

# Android debug APK（要求 JDK 21 与 Android SDK 36）
npm run package:android
```

默认输出：

```text
release/desktop/mac-arm64/Ouroboros.app
release/desktop/Ouroboros-1.0.0-Windows-x64.exe
release/android/Ouroboros-1.0.0-debug.apk
```

`.app` 是包含符号链接的目录，不应直接通过聊天工具按文件夹发送；先使用 `ditto` 或 CI 压成 ZIP。Electron 自带 Chromium/WebGL 运行时，因此解压后的 `.app` 明显大于游戏资源本身。

Android 工程已提交到 `android/`。首次更换图标后运行 `npm run android:assets`，Web 或插件变更后运行 `npm run android:sync`。Debug APK 可安装测试，但商店发布前必须用团队密钥生成并验证签名 Release APK。

完整工具链、签名和验收说明见 [docs/PACKAGING.md](docs/PACKAGING.md)。

## 目录

```text
public/
└── assets/ouroboros/             # 游戏运行时美术成品
src/
├── app/                         # 应用入口，只负责装配
├── features/ouroboros/
│   ├── audio/                   # 音频偏好与安全持久化
│   ├── components/              # Vue 展示组件
│   ├── composables/             # Vue 与 Phaser 生命周期协调
│   ├── engine/                  # 纯 TypeScript 玩法规则
│   ├── input/                   # 键盘到语义动作的映射
│   └── phaser/                  # Scene、渲染、碰撞与资源清单
├── shared/
│   └── components/              # 与玩法无关的基础组件
└── styles/                      # 全局令牌与基础样式
```

更详细的依赖方向和分工约定见 [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)。

美工交付路径、尺寸规范和待替换资源清单见 [docs/ART_ASSETS.md](docs/ART_ASSETS.md)。

## 操作

- 鼠标点击或拖动、触屏拖动：连续转向
- `WASD` / 方向键：四方向转向
- `Space` / `Esc` / `P`：暂停或继续
- 暂停菜单：继续游戏、结束本局、调整总音量或静音
- 净化积分首次达到 15 分：噬环者登场；闭环捕获外置核心以破坏护甲
- Boss 战：出场后切换专属背景音乐；冲刺、核心破甲和击败均有独立声音反馈

浏览器生产构建输出到 `dist/`；最终交付使用上述 `.app`、`.exe` 与 `.apk`。
