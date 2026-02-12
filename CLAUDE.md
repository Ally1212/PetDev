# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

PetVoice — 宠物情感识别与翻译的移动端 App 原型。当前为**单文件应用**，整个应用完全包含在 `index.html`（~3400行）中，无构建系统。

## 运行方式

直接在浏览器中打开 `index.html` 即可运行，无需安装依赖或构建步骤：
```bash
open index.html
# 或使用本地服务器
python3 -m http.server 8000
```

## 技术栈

- **React 18**（CDN UMD 生产构建）+ **Babel Standalone**（JSX 转换）
- **Tailwind CSS**（CDN 引入）
- **CSS 自定义属性**：完整设计令牌系统（颜色、阴影、动画）
- **字体**：Nunito（Google Fonts，wght 400-800）
- 无 npm/package.json、无打包工具、无 TypeScript

## 架构

### 单文件结构
`index.html` 包含所有内容：HTML 骨架 → `<style>` CSS 变量/动画 → Tailwind 配置 → `<script type="text/babel">` React 组件。

### 组件层级
```
App (路由 + 全局状态)
├── HomePage        # 首页：宠物心情、语音/拍照/视频三模式、波形可视化、翻译结果
├── ProfilePage     # 宠物档案：理解度评分、翻译历史时间线
├── CollectionPage  # 图鉴页（占位符）
├── HealthPage      # 健康监测：健康评分环、声音事件、7日趋势SVG图表、分离焦虑报告
├── SettingsPage    # 设置：用户信息、会员、通知、隐私
├── AddPetModal     # 添加宠物底部抽屉
├── BottomNav       # 底部导航栏
└── Toast           # 全局轻提示
```

### 共享组件
- **ProgressRing** — SVG 圆形进度条
- **BottomSheet** — iOS 风格底部抽屉
- **Toast** — 轻提示

### 状态管理
全部状态在 App 组件中通过 `useState` 管理，通过 props 向下传递。核心状态：
- `activeTab`: 当前页签（home/profile/collection/health/settings）
- `selectedPet`: 当前选中宠物索引
- `inputMode`: 输入模式（voice/photo/video）
- `recordingState`: 录音状态机（idle → recording → translating → done）

### 图标系统
38 个内联 SVG 图标函数（`icons` 对象），如 `icons.mic`、`icons.camera`、`icons.pawprint` 等。

## 设计令牌

| 色彩 | 值 | 用途 |
|------|------|------|
| Parchment | #FAF6F0 | 主背景 |
| Bark | #3D2B1F | 主文本 |
| Honey | #D4923A | CTA/强调 |
| Sage | #6B9E74 | 成功/积极 |
| Coral | #D97755 | 警告/录制中 |
| Sky | #5B8FB9 | 信息/次要 |

## 关键交互流

**录音流程**：点击录制 → 3秒录音（波形动画）→ 1.5秒翻译 → 展示结果卡片 → 30%概率弹出 AI 学习提示

## 开发注意事项

- 所有修改都在 `index.html` 单文件中进行
- 移动端适配：最大宽度 430px，iOS Safe Area，禁用缩放
- 当前所有数据为模拟数据（Mock），无真实 API
- 动画使用 CSS `@keyframes` + `transform/opacity`（GPU 加速）
- 代码注释和 UI 文案使用中文
