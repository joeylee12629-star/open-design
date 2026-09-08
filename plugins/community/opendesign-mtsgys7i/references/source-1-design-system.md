# OpenDesign 官网设计系统（实测提取）

- 来源：https://open-design.ai/zh/ 首页
- 方法：2026-09-08 用 Playwright 读取 `:root` 变量与各元素 computed styles（1440×900 视口），截图留底 `research/od-home-hero.jpeg`
- 用途：活动页 `index.html` 的 `:root` token 与组件规格全部取自本文；未在官网实测到的值标「待补」

## 1. 色板

| Token | 值 | 官网变量名 | 用途 |
|---|---|---|---|
| paper | `#fafafa` | `--paper` | 页面底色（html/body/footer） |
| paper-warm | `#f5f5f5` | `--paper-warm` | 次级底、极淡分隔 |
| paper-dark | `#f0f0f0` | `--paper-dark` / `--line-soft` | 网格线、极淡描边 |
| bone | `#ffffff` | `--bone` | 卡片底、深色面上的文字 |
| ink | `#262626` | `--ink` | 正文、标题、主按钮底 |
| ink-soft | `#434343` | `--ink-soft` | 段落正文（17px 说明文） |
| ink-mute | `#595959` | `--ink-mute` | 辅助说明、横条 detail 文字 |
| ink-faint | `#8c8c8c` | `--ink-faint` | 眉题、导航分组标题 |
| line | `#d9d9d9` | `--line` | 1px 边线、页脚顶线、卡片描边 |
| line-hover | `#bfbfbf` | `--line-hover` | hover 时描边 |
| accent | `#63fe13` | `--coral` / `--mustard` | 选择框、荧光笔色带、状态点、大数字 |
| accent-soft | `#83ff3b` | `--coral-soft` | accent 的亮变体（hover） |
| accent-badge | `#68f22e` | 横条 `NEW` 徽章实测 | 小徽章底 |
| accent-tint | `#d8ffb5` | 横条底实测 `rgb(216,255,181)` | 顶部活动横条底 |
| accent-tag | `#d6ffc2` | Hero 标签实测 `color(srgb .84 .999 .76)` | 标签胶囊底 |
| accent-ink | `#0d2601` | 绿底上文字实测 `rgb(13,38,1)` | 绿底徽章 / 绿圆图标上的文字 |
| olive | `#218c00` | `--olive` | 深绿文字、成功态 |
| card-dark | `#141414` | 数据卡实测 `rgb(20,20,20)` | 深色统计卡、深色 CTA 面板 |

用法纪律（实测归纳）：
- `#63fe13` 在首页只出现 55 次，全部是**小面积**：H1 选择框 1px 边线 + 4 个 9px 方柄、Discord 图标旁 7px 状态点、`领 Credits` 11px 徽章、下载按钮里 30px 绿圆、Dock 选中态图标、57.6px 大数字。**不做大面积色块、不做背景。**
- 大面积「绿」只用淡绿 `#d8ffb5`（横条）和 `#d6ffc2`（标签），文字仍是 `#262626`。
- 阴影只有两档：主按钮 `0 14px 26px -16px rgba(38,38,38,.42)`；大卡 `0 30px 80px -40px rgba(38,38,38,.28)`；官网变量 `--shadow: 0 30px 60px -30px rgba(38,38,38,.16)`。

## 2. 字体与字阶

- 字族：`"Albert Sans", "PingFang SC", "Microsoft YaHei", sans-serif`（官网 `--serif/--sans/--body/--mono` 四个变量全部指向同一栈，即全站单一字族）
- 字体文件：`https://open-design.ai/skill-assets/AlbertSans-VariableFont_wght.woff2`（变量字重 100–900，已本地化到 `assets/`）
- 图标字体：Remix Icon（线性图标族）

| 层级 | 字号 / 字重 / 行高 / 字距 | 颜色 | 实测位置 |
|---|---|---|---|
| H1 hero | 72px / 700 / 1.0 / 0 | ink | `.hero-title` |
| Display L | 56px / 600 / 1.06 / −0.022em | ink | 「OpenDesign 订阅」 |
| Display M | 48px / 800 / 1.0 / −0.028em | ink | 「用 OpenDesign 能产出什么」 |
| Display S | 38–42px / 800 / 1.0–1.1 / −0.028em | ink | 各区块 h2 `.display` |
| 大数字 | 57.6px / 800 / 0.98 / −0.028em | accent / ink | 统计卡 h3 |
| 段落大字 | 26px / 800 / 1.35 | ink | About 区 h2 |
| Lede | 22.5px / 500 / 1.5 | ink | About 首段 |
| 正文 L | 17px / 400 / 1.5 | ink-soft | 说明段（max-width 720px） |
| 正文 | 16px / 400 / 1.55 | ink | body 默认 |
| 正文 S | 14px / 400–500 / 1.55 | ink-mute | FAQ 答案、导航链接 |
| 横条 | 13px / 400（重点 700） | ink / ink-mute | 顶部活动横条 |
| 标签 | 13px / 600 | ink | `.hero-tag` |
| 徽章 | 11px / 800 | accent-ink | `NEW` / `领 Credits` |
| 眉题 | 11px / 700 / uppercase / +0.06em | ink-faint | `.nav-mega-col-head` |
| 页脚字标 | 149.76px / 800 / −0.045em | ink | 「OpenDesign.」 |

## 3. 间距与栅格

- 基数 4px：官网 `--spacing-2 … --spacing-68` 均为 4 的倍数（含 6/10/18/22 等半档）
- 容器：`max-width: 1360px`，左右 padding 64px（`.container`）；窄内容 1080 / 980 / 900 / 720 / 520
- 区块垂直：主要区块 130px、紧凑区 90px、testimonial 120px、newsletter 96px、capabilities 40px
- Hero 文案区：上 120.8px 下 36px，居中
- 导航：高 85px（padding 22px 0）；本次活动页压到 64px（Joey 的官网构图一致性规则：导航 ≤ 72px）
- 页脚：`padding: 60px 0 32px`，顶部 `1px solid #d9d9d9`

## 4. 圆角

| 值 | 出现次数 | 用途 |
|---|---|---|
| 6px | 78 | 导航项、语言切换、小控件 |
| 999px | 37 | 所有按钮、标签、徽章（胶囊） |
| 50% | 26 | 状态点、圆形图标 |
| 8px | 14 | 输入框、语言下拉边框 |
| 12–13px | 9 | 内容面板、Dock 项 |
| 15–18px | 30 | 卡片（视频框 16px、统计卡 18px） |
| 24px | 3 | 底部 CTA 大面板 |

## 5. 按钮

| 类型 | 规格 |
|---|---|
| 主按钮 | 底 `#262626`、字 `#fff`、999px、14px/500、padding 14px 22px、阴影 `0 14px 26px -16px rgba(38,38,38,.42)`、高 ~52px |
| Hero 主按钮 | 同上但 15px/750、padding 11px 24px 11px 11px、左侧 30px 绿圆（`#63fe13` 底 + `#0d2601` 箭头，900 字重） |
| 次按钮（幽灵） | 透明底、`1px solid rgba(21,20,15,.2)`、999px、14px/500、padding 14px 22px |
| Hero 次按钮 | 底 `rgba(255,255,255,.86)`、`1px solid rgba(26,26,26,.18)`、15px/650、高 48px、可内嵌绿徽章 |
| 导航 CTA | 底 `#262626`、字 `#fafafa`、13px/500、padding 7px 15px |
| 导航 Star 胶囊 | 透明底、`1px solid rgba(21,20,15,.14)`、12.5px/650、字 `#595959` |
| 横条箭头 | 底 `#262626`、字 `#f7f7f3`、13px/750、padding 5px 10px |

## 6. 品牌 motif

1. **选择框（Figma selection）**：H1 外 `1px solid #63fe13`，padding 28px 48px，四角各一个 9px × 9px `#63fe13` 实心方柄（absolute 定位到四角）。全站只用在一处主标题上。
2. **荧光笔色带**：副标题关键短语底部一条 `#63fe13` 半透明色带（约文字高 40%），像 marker 划过。
3. **顶部活动横条**：`position: fixed; top: 0; z-index: 70`，高 45px，底 `#d8ffb5`，`1px solid` 底线（`#b6dfaa` 附近），内容 = `NEW` 徽章 + `|` 分隔（`rgba(38,38,38,.26)`）+ 粗体标题 + 灰色 detail + 黑色箭头胶囊 + 右侧关闭 ×。激活时 `html` 加 class，用 `--home-campaign-banner-height` 给页面让位。
4. **标签胶囊**：`#d6ffc2` 底、13px/600、padding 6px 13px，横向一排（品牌一致性设计 / 开源 · Apache-2.0 / BYOK …）。
5. **背景**：极淡网格纸 + 黄金螺旋线稿 + 设计工具 3D 物件（铅笔、T 工具、光标、渐变条）。活动页只取网格 + 螺旋线稿，不做 3D。
6. **深色统计卡**：`#141414` 底、18px 圆角、阴影 `0 28px 60px -42px rgba(0,0,0,.72)`，大数字用 `#63fe13`。
7. **页脚巨型字标**：「OpenDesign.」149.76px/800/−0.045em，压在页脚底部。

## 7. 动效

- 官网 hero 标题逐词 blur-in（`.blur-word`）；本次沿用 200–300ms ease-out 入场、40ms stagger
- 尊重 `prefers-reduced-motion`：全部关闭

## 8. 未实测 / 待补

- 输入框规格（首页 newsletter 输入框实测被隐藏，未取到有效值）
- 价格卡片规格（首页无独立卡片描边元素）
- 暗色模式：官网现行无暗色主题
