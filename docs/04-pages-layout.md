# 04 · 页面信息架构 · 交互设计

> 视觉风格：Inspira UI + Aceternity UI + Magic UI  
> 详细设计规范见 [06-visual-design.md](./06-visual-design.md)

---

## 全局页面流程

```
[HomePage]
    ↓ 点击某个系列
[CollectionPage]
    ↕ 顶部导航切换系列 / 返回首页
[AboutPage]（P1）
```

---

## Page 1 · HomePage（首页）

### 页面目标
- 第一眼建立视觉冲击，传递摄影师的风格调性
- 清晰展示有哪些系列，让观众选择进入

### 信息架构

```
┌─────────────────────────────────────────────────────┐
│  [Logo]                         [系列1][系列2][关于] │  ← 顶部导航（毛玻璃）
├─────────────────────────────────────────────────────┤
│                                                     │
│         ✦ Aurora Background / Silk Background ✦    │  ← 全屏背景特效
│                                                     │
│              [封面大图]                              │  ← 封面照片（主角）
│                                                     │
│         ── Ruby Love ──                             │  ← Blur Reveal 入场
│         Photography                                 │  ← Sparkles Text 点缀
│                                                     │
│              [↓ 进入展览]                           │  ← Border Beam 按钮
│                                                     │
├─────────────────────────────────────────────────────┤
│  ░░░░░░░░░░░░  Particles Background（极低不透明度）  │
│                                                     │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐          │
│  │          │  │          │  │          │          │
│  │  系列封面 │  │  系列封面 │  │  系列封面 │          │  ← 3D Card + Glare
│  │          │  │          │  │          │          │
│  │ 城市之夜  │  │ 山与远方  │  │  人与光   │          │
│  └──────────┘  └──────────┘  └──────────┘          │
│                                                     │
│  ┌──────────┐  ┌──────────┐                        │
│  │  系列封面 │  │  系列封面 │                        │
│  │   雨季    │  │  空与远   │                        │
│  └──────────┘  └──────────┘                        │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 特效分配

| 区域 | 特效 | 来源库 | 参数控制 |
|------|------|--------|----------|
| 全屏背景 | Aurora Background | Inspira UI | opacity: 0.4，色调偏冷紫/蓝 |
| 底层全局 | Particles Background | Inspira UI | count: 30，opacity: 0.2 |
| 网站主标题 | Blur Reveal | Inspira UI | duration: 0.8s |
| Photography 副标题 | Sparkles Text | Inspira UI | 星光密度低 |
| 进入展览按钮 | Border Beam | Inspira UI | 流光色: --accent |
| 系列卡片 | 3D Card Effect + Glare Card | Inspira UI | maxTilt: 8deg |
| 系列卡片边框 | Glow Border | Inspira UI | glowColor: --accent-glow |

### 交互说明

| 操作 | 反馈 |
|------|------|
| 页面加载完成 | Aurora 背景渐入 → 封面图 Progressive Blur 揭示 → 标题 Blur Reveal |
| 滚轮向下 | 平滑滚动到系列区域，粒子密度微增 |
| Hover 系列卡片 | 3D 倾斜 + Glare 光扫过 + Glow Border 亮起 |
| 点击系列卡片 | clip-path 过渡进入 CollectionPage |

---

## Page 2 · CollectionPage（系列展览页）

### 页面目标
- 让观众完全沉浸在这个系列的照片里
- 照片是绝对主角，特效退到最远处
- 浏览方式自然流畅，不分散注意力

### 布局方案（推荐：网格概览 + 点击大图）

```
┌─────────────────────────────────────────────────────┐
│  [← 返回]         城市之夜           [系列1][系列2]  │  ← 顶部导航
│─────────────────────────────────────────────────────│
│                                                     │
│  城市之夜                ← Letter Pullup 入场       │
│  City at Night                                      │
│  夜晚的城市有另一种语言。 ← Text Generate Effect     │
│                                                     │
│  ░ Ripple 短暂出现后消退（系列切换时触发）           │
│                                                     │
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐           │
│  │ img1 │  │ img2 │  │ img3 │  │ img4 │           │  ← 照片网格
│  └──────┘  └──────┘  └──────┘  └──────┘           │  ← Progressive Blur 加载
│  ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐           │
│  │ img5 │  │ img6 │  │ img7 │  │ img8 │           │
│  └──────┘  └──────┘  └──────┘  └──────┘           │
│                                                     │
└─────────────────────────────────────────────────────┘

            ↓ 点击某张图

┌─────────────────────────────────────────────────────┐
│  [✕ 关闭]                        [← 3/12 →]        │
│─────────────────────────────────────────────────────│
│                                                     │
│                                                     │
│            [全屏大图，纯黑背景，无特效]              │  ← 照片绝对主角
│                                                     │
│            Card Spotlight（光圈跟随鼠标）            │
│                                                     │
│─────────────────────────────────────────────────────│
│  霓虹倒映在空无一人的街面上           ← caption     │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 特效分配

| 区域 | 特效 | 来源库 | 参数控制 |
|------|------|--------|----------|
| 系列切换瞬间 | Ripple | Inspira UI | 短暂，0.6s 后消退 |
| 系列标题入场 | Letter Pullup | Inspira UI | stagger: 0.04s/字 |
| 系列简介入场 | Text Generate Effect | Inspira UI | delay: 0.3s |
| 照片加载 | Progressive Blur 揭示 | Inspira UI | 每张独立触发 |
| 照片 hover | 轻微放大 scale(1.02) | CSS transition | duration: 0.3s |
| 大图弹层 | Card Spotlight | Inspira UI | 光圈半径: 200px，opacity: 0.08 |
| 大图背景 | 纯黑 `#000`，**无任何特效** | — | — |

### 交互说明

| 操作 | 反馈 |
|------|------|
| 进入页面 | Ripple 短暂出现 → 系列标题 Letter Pullup → 图片依次 Progressive Blur 揭示 |
| Hover 网格照片 | scale(1.02) 微放大，cursor pointer |
| 点击照片 | 弹层淡入（opacity 0→1，0.25s），照片 Progressive Blur 揭示 |
| 键盘 ← → | 切换照片，前后图片交叉淡入淡出 |
| 键盘 Esc | 弹层淡出关闭 |
| 鼠标在大图内移动 | Card Spotlight 光圈跟随 |
| 点击遮罩层 | 弹层淡出关闭 |

---

## Page 3 · AboutPage（关于页，P1）

### 信息架构

```
┌─────────────────────────────────────────────────────┐
│  [← 返回]                                           │
│─────────────────────────────────────────────────────│
│                                                     │
│  ░ Wavy Background（低调，opacity: 0.15）           │
│                                                     │
│  [摄影师照片]                                       │  ← Direction Aware Hover Card
│                                                     │
│  关于我          ← Blur Reveal                      │
│  ─────────────────                                 │
│  简短介绍，2～3 句。                                │  ← Text Generate Effect
│                                                     │
│  联系方式                                           │
│  📧 email@example.com                              │
│  📷 @instagram_handle                              │
│                                                     │
└─────────────────────────────────────────────────────┘
```

### 特效分配

| 区域 | 特效 | 来源库 |
|------|------|--------|
| 页面背景 | Wavy Background | Inspira UI |
| 摄影师照片 | Direction Aware Hover Card | Inspira UI |
| 关于标题 | Blur Reveal | Inspira UI |
| 介绍文字 | Text Generate Effect | Inspira UI |

---

## 全局导航

```
[Logo / Ruby Love]                 [城市之夜] [山与远方] [人与光] [关于]
```

| 状态 | 样式 |
|------|------|
| 默认 | 透明背景，文字 `--text-secondary` |
| 滚动后 | `backdrop-blur-md` + `bg-black/40`，border-bottom 细线 |
| 链接 hover | 米金色下划线从左向右展开（CSS transition） |
| 当前系列 | 米金色 `--accent`，不加下划线 |

---

## 交互原则

| 原则 | 具体要求 |
|------|----------|
| **照片绝对主角** | 大图弹层内禁止任何背景特效 |
| **特效退到远处** | 背景特效 opacity 统一 ≤ 0.4 |
| **不闪** | 所有切换有过渡，时长 0.25～0.5s |
| **不抢** | 文字动效只在内容第一次出现时播放，不循环 |
| **不卡** | 懒加载 + 预加载相邻 1～2 张图 |
| **可降级** | 尊重 `prefers-reduced-motion`，特效全部关闭 |
