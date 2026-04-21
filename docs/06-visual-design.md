# 06 · 视觉设计规范

> 风格参考：**Inspira UI + Aceternity UI + Magic UI**  
> 核心原则：**光效与粒子烘托氛围，照片永远是主角**

---

## 设计语言定位

| 参考来源 | 借鉴方向 |
|----------|----------|
| **Inspira UI** | Vue 3 原生组件基础、极光/粒子/丝绸背景特效、3D Card、Glow Border |
| **Aceternity UI** | 暗色科技展厅基调、光束追踪、视差滚动、Glare Card、发光边框 |
| **Magic UI** | 文字动效精细度、入场动画的克制流畅、Blur Reveal / Text Reveal |

**融合关键词：**
> 暗色展厅 · 光效点缀 · 粒子氛围 · 克制动效 · 照片主角

---

## 色彩系统

```css
/* 背景层级 */
--bg-base:        #080808;   /* 主背景，近全黑，展厅感 */
--bg-surface:     #111111;   /* 卡片/面板背景 */
--bg-elevated:    #1a1a1a;   /* 悬浮层背景 */

/* 文字层级 */
--text-primary:   #f0f0f0;   /* 标题、重要正文 */
--text-secondary: #a0a0a0;   /* 说明文字、caption */
--text-muted:     #555555;   /* 辅助信息、时间等 */

/* 强调色（米金，贯穿全站） */
--accent:         #c8a97e;   /* 主强调色，hover、选中、下划线 */
--accent-dim:     #8a7055;   /* 弱化版，用于边框 */
--accent-glow:    rgba(200, 169, 126, 0.15);  /* 发光用半透明版 */

/* 光效色 */
--glow-white:     rgba(255, 255, 255, 0.06);  /* 卡片高光 */
--glow-blue:      rgba(96, 165, 250, 0.08);   /* 冷调光效点缀 */
```

---

## 字体系统

```css
/* 展览标题 / Hero 大标题 */
font-family: 'Cormorant Garamond', 'Playfair Display', serif;
/* → 高端、文艺、展览感，字重 300～400 */

/* 系列标题 / 导航 */
font-family: 'Cormorant Garamond', serif;
/* → 字重 400，字号 1.25rem～2rem */

/* 中文内容 / caption / 正文 */
font-family: 'Noto Serif SC', 'Source Han Serif SC', serif;
/* → 与英文衬线搭配，气质一致 */

/* UI 标签 / 辅助信息 */
font-family: 'Inter', 'system-ui', sans-serif;
/* → 字重 300～400，字号 0.75rem～0.875rem */
```

---

## 组件选用规范

### 背景层（Background）

| 使用位置 | 选用组件 | 说明 |
|----------|----------|------|
| 全站底层 | **Particles Background** | 低密度、低不透明度粒子，作为全局氛围底色 |
| 首页 Hero | **Aurora Background** 或 **Silk Background** | 封面区极光/丝绸流动效果，视觉冲击 |
| 系列页进入 | **Ripple**（涟漪） | 章节切换时短暂出现的涟漪过渡 |
| 关于页 | **Wavy Background** | 低调波浪，不抢内容 |

> ⚠️ 背景特效统一设置低不透明度（0.3～0.5），绝不能盖过照片本身

---

### 卡片层（Card）

| 使用位置 | 选用组件 | 说明 |
|----------|----------|------|
| 首页系列卡片 | **3D Card Effect** + **Glare Card** | Hover 时 3D 倾斜 + 眩光扫过 |
| 大图浏览弹层 | **Card Spotlight** | 光圈跟随鼠标高亮 |
| 关于页作品精选 | **Direction Aware Hover Card** | 方向感知悬停，增加灵动感 |

---

### 文字动效（Text Animation）

| 使用位置 | 选用组件 | 说明 |
|----------|----------|------|
| 网站主标题入场 | **Blur Reveal** | 从模糊到清晰，优雅入场 |
| 系列标题入场 | **Text Reveal** / **Letter Pullup** | 字母依次上升或揭示 |
| 图片 caption | **Text Generate Effect** | 文字逐渐显现，配合照片加载 |
| 系列副标题 | **Morphing Text** | 在多个短语间平滑变形（可选） |
| 页面 tagline | **Sparkles Text** | 标题文字带星光闪烁点缀 |

> ⚠️ 同一页面内文字动效种类不超过 **2 种**，避免视觉噪声

---

### 特效层（Special Effects）

| 使用位置 | 选用组件 | 说明 |
|----------|----------|------|
| 系列卡片边框 | **Border Beam** / **Glow Border** | 流光扫过边框，增强质感 |
| 封面图容器 | **Glowing Effect** | 封面图周围动态发光光晕 |
| 照片加载完成 | **Progressive Blur** 反向播放 | 从模糊到清晰的加载过渡 |
| 彩蛋/特殊时刻 | **Particle Image** | 照片以粒子形式聚合出现（重点一张） |

---

### 导航 & 交互

| 使用位置 | 效果 | 说明 |
|----------|------|------|
| 顶部导航栏 | 毛玻璃 `backdrop-blur-md` + 半透明背景 | 滚动后出现 |
| 导航链接 hover | 米金色下划线从左向右展开 | 统一 hover 语言 |
| 返回按钮 | 箭头向左淡入 + 位移动效 | |
| 页面切换 | GSAP `clip-path` 或淡入淡出 | 不超过 0.5s |

---

## 动效原则

| 原则 | 具体要求 |
|------|----------|
| **时长克制** | 入场动效 0.4～0.8s，交互反馈 ≤ 0.3s |
| **缓动统一** | 统一使用 `cubic-bezier(0.16, 1, 0.3, 1)`（spring feel） |
| **层级分明** | 背景特效 < 内容动效 < 交互反馈，优先级不能乱 |
| **照片优先** | 任何动效在照片附近时减弱或暂停，不抢焦点 |
| **可关闭** | 尊重 `prefers-reduced-motion`，动效可降级 |

---

## 照片展示规范

| 规范 | 要求 |
|------|------|
| 宽高比 | 保留原始比例，不强制裁切 |
| 加载过渡 | 骨架屏（正确比例）→ Progressive Blur 揭示 |
| 大图弹层背景 | `#000` 纯黑，不加任何特效，照片独立呈现 |
| Caption 位置 | 图片下方或右侧，字号 0.8rem，颜色 `--text-secondary` |
| 网格间距 | `gap-3` 或 `gap-4`，保持呼吸感 |
| 封面图 | 每个系列单独设定，不自动截取，手动选择最有代表性的一张 |

---

## 禁止事项

- ❌ 禁止在照片上叠加任何动效特效
- ❌ 禁止背景粒子/光效不透明度超过 0.5
- ❌ 禁止同一页面使用超过 2 种文字动效
- ❌ 禁止颜色跳变（所有颜色变化必须有 transition）
- ❌ 禁止在大图弹层内加任何背景特效
- ❌ 禁止使用超过 3 种不同字体

---

## 参考截图风格关键词（Moodboard）

用以下关键词在 Dribbble / Pinterest 搜索参考：

- `dark photography portfolio`
- `immersive gallery website`
- `aceternity ui dark`
- `film photography website minimal`
- `editorial photography layout`
