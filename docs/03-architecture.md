# 03 · 技术架构

---

## 技术栈选型

| 层级 | 技术 | 理由 |
|------|------|------|
| 前端框架 | Vue 3 + Vite | 组件化、开发体验好、构建快 |
| 状态管理 | Pinia | 轻量直观，管理当前系列 / 照片状态 |
| 路由 | Vue Router 4 | 首页 / 系列页 / 关于页跳转 |
| 样式 | Tailwind CSS | 快速布局，减少手写 CSS 时间 |
| 动效 | GSAP + motion-v | 页面级过渡用 GSAP，组件级微动效用 motion-v |
| UI 特效组件 | **Inspira UI** | Vue 3 原生，包含粒子/极光/3D卡片/文字动效等 |
| 图片懒加载 | VueUse `useIntersectionObserver` | 轻量，无需额外依赖 |
| 后续扩展 | 移动端 H5 / 小程序 | 现阶段只做 PC |

> **Inspira UI 是本项目视觉特效的核心来源**，基于 Vue 3 原生实现，与项目技术栈完全契合。  
> Aceternity UI / Magic UI 的设计语言作为视觉参考，相应效果通过 Inspira UI 对应组件实现。

---

## 项目目录结构

```
ruby-love/
├── docs/                          # 项目文档
├── public/
│   └── favicon.ico
├── src/
│   ├── main.js                    # 应用入口
│   ├── App.vue                    # 根组件
│   │
│   ├── router/
│   │   └── index.js               # 路由配置
│   │
│   ├── stores/
│   │   └── galleryStore.js        # 系列数据 & 当前浏览状态
│   │
│   ├── data/
│   │   └── collections.js         # 所有系列 & 照片的静态数据配置
│   │
│   ├── pages/
│   │   ├── HomePage.vue           # 首页：封面 + 系列入口
│   │   ├── CollectionPage.vue     # 系列展览页：沉浸式照片浏览
│   │   └── AboutPage.vue          # 关于页（P1）
│   │
│   ├── components/
│   │   ├── home/
│   │   │   ├── HeroCover.vue      # 首页全屏封面
│   │   │   └── CollectionGrid.vue # 系列卡片网格
│   │   │
│   │   ├── collection/
│   │   │   ├── PhotoViewer.vue    # 照片全屏浏览器（核心）
│   │   │   ├── PhotoSlide.vue     # 单张照片展示（含文字说明）
│   │   │   ├── PhotoGrid.vue      # 系列内照片网格概览
│   │   │   └── NavArrows.vue      # 上一张 / 下一张导航
│   │   │
│   │   └── common/
│   │       ├── LazyImage.vue      # 带懒加载的图片组件
│   │       ├── PageTransition.vue # 页面切换过渡
│   │       └── LoadingPlaceholder.vue # 图片加载占位
│   │
│   ├── composables/
│   │   ├── usePhotoNavigation.js  # 照片切换逻辑（键盘/滚轮/点击）
│   │   └── useFullscreen.js       # 全屏查看逻辑（P1）
│   │
│   ├── assets/
│   │   ├── photos/                # 照片资源（按系列分目录）
│   │   │   ├── collection-01/
│   │   │   ├── collection-02/
│   │   │   └── collection-03/
│   │   └── styles/
│   │       └── global.css
│   │
│   └── utils/
│       └── imageHelper.js         # 图片路径处理 / 尺寸工具
│
├── index.html
├── vite.config.js
└── package.json
```

---

## 路由设计

| 路径 | 页面 | 说明 |
|------|------|------|
| `/` | HomePage | 首页封面 + 系列导航 |
| `/collection/:id` | CollectionPage | 某个系列的展览页 |
| `/about` | AboutPage | 关于摄影师（P1） |

---

## 数据结构

所有照片信息以静态 JS 配置文件维护，不需要后端。

```javascript
// src/data/collections.js

export const collections = [
  {
    id: 'city-night',
    title: '城市之夜',
    subtitle: 'City at Night',
    description: '夜晚的城市有另一种语言。',
    cover: '/src/assets/photos/city-night/cover.jpg',
    photos: [
      {
        id: 'cn-01',
        src: '/src/assets/photos/city-night/01.jpg',
        caption: '霓虹倒映在空无一人的街面上',   // 可选
        width: 4000,   // 原始尺寸，用于计算宽高比
        height: 2667
      },
      // ...更多照片
    ]
  },
  {
    id: 'mountain',
    title: '山与远方',
    subtitle: 'Mountains & Beyond',
    description: '在高处，呼吸变得更清晰。',
    cover: '/src/assets/photos/mountain/cover.jpg',
    photos: [ /* ... */ ]
  }
]
```

---

## 数据流向

```
collections.js（静态配置）
    ↓
galleryStore（Pinia 状态）
    ↓
CollectionPage 读取当前系列数据
    ↓
PhotoViewer 渲染照片列表
    ↓
用户操作（键盘 / 滚轮 / 点击）→ usePhotoNavigation → 切换当前照片
```

---

## 性能考虑

| 问题 | 方案 |
|------|------|
| 图片体积大 | 构建时压缩，提供 WebP 格式，原图仅全屏查看时加载 |
| 首屏白屏 | 封面图优先加载，其余图片懒加载 |
| 切换卡顿 | 预加载相邻 1～2 张图片 |
| 布局抖动 | 图片占位用正确宽高比的骨架，避免加载后跳动 |

---

## 部署方案

| 阶段 | 方案 |
|------|------|
| 开发 | `vite dev` 本地运行 |
| 演示 | `vite build` + 静态文件托管 |
| 上线 | Vercel / Netlify / GitHub Pages（纯静态，免费） |
