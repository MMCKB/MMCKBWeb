# 顶部 Tab 栏 + 毛玻璃效果参数文档

## 文件清单

| 文件 | 作用 |
|------|------|
| `src/styles/navbar.css` | 导航栏悬浮、椭圆圆角、下拉面板毛玻璃 |
| `src/styles/main.css` | 毛玻璃变量定义 + 全局玻璃化样式 |
| `src/layouts/MainGridLayout.astro` | 导航栏容器结构（L316-L335） |
| `src/config/backgroundWallpaper.ts` | 壁纸模式 `overlay` + 透明度参数 |
| `src/config/siteConfig.ts` | 导航栏配置（sticky、widthFull 等） |

---

## 一、毛玻璃变量定义（main.css L597-L606）

```css
:root {
  --glass-border-light: rgba(255, 255, 255, 0.38);
  --glass-border-dark: rgba(255, 255, 255, 0.12);
  --glass-shadow: 0 8px 32px rgba(0, 0, 0, 0.12);
  --glass-shadow-dark: 0 8px 32px rgba(0, 0, 0, 0.35);
  --glass-blur: blur(18px) saturate(160%);
  --glass-blur-strong: blur(22px) saturate(170%);
  --glass-blur-soft: blur(14px) saturate(150%);
  --glass-blur-subtle: blur(16px) saturate(160%);
}
```

### 变量说明

| 变量 | 用途 | 值 |
|------|------|----|
| `--glass-border-light` | 亮色模式玻璃边框 | `rgba(255,255,255,0.38)` |
| `--glass-border-dark` | 暗色模式玻璃边框 | `rgba(255,255,255,0.12)` |
| `--glass-shadow` | 亮色模式玻璃阴影 | `0 8px 32px rgba(0,0,0,0.12)` |
| `--glass-shadow-dark` | 暗色模式玻璃阴影 | `0 8px 32px rgba(0,0,0,0.35)` |
| `--glass-blur` | 标准模糊（卡片/按钮/面板） | `blur(18px) saturate(160%)` |
| `--glass-blur-strong` | 强模糊（导航栏） | `blur(22px) saturate(170%)` |
| `--glass-blur-soft` | 柔和模糊（正文区域） | `blur(14px) saturate(150%)` |
| `--glass-blur-subtle` | 轻微模糊（侧边栏/评论） | `blur(16px) saturate(160%)` |

---

## 二、导航栏悬浮参数（navbar.css L35-L42）

### 桌面端

```css
#navbar>div {
    background-color: var(--card-bg);
    border: 1px solid transparent;
    border-radius: 9999px;       /* 椭圆胶囊形 */
    backdrop-filter: blur(0);
    margin: 0.6rem 1rem;          /* 桌面端悬浮间距，上0.6rem 左右1rem */
    @apply navbar-surface-transition;
}
```

### 移动端（navbar.css L226-L230）

```css
@media (max-width: 767px) {
    #navbar>div {
        margin: 0.5rem 0.75rem;           /* 移动端悬浮间距 */
        border-radius: 9999px !important;
    }
}
```

### 过渡动画（navbar.css L22-L29）

```css
@utility navbar-surface-transition {
  transition:
    background-color 0.36s cubic-bezier(0.22, 1, 0.36, 1),
    border-color 0.36s cubic-bezier(0.22, 1, 0.36, 1),
    box-shadow 0.36s cubic-bezier(0.22, 1, 0.36, 1),
    backdrop-filter 0.36s cubic-bezier(0.22, 1, 0.36, 1),
    border-radius 0.36s cubic-bezier(0.22, 1, 0.36, 1);
}
```

### 滚动阴影（navbar.css L46-L52）

```css
#navbar.navbar-sticky-shadow>div {
    box-shadow: var(--shadow-navbar) !important;
}

:root.dark #navbar.navbar-sticky-shadow>div {
    box-shadow: var(--shadow-navbar-dark) !important;
}
```

### 全宽导航栏覆盖（navbar.css L320-L328）

```css
#navbar-wrapper #navbar[data-full-width="true"] > div {
    border-radius: 0 !important;
    width: 100% !important;
    max-width: none !important;
    margin: 0 !important;
}
```

---

## 三、下拉面板 / 浮动面板毛玻璃（navbar.css L278-L312）

### 亮色主题

```css
#wallpaper-wrapper~* .dropdown-content,
#wallpaper-wrapper~* .float-panel,
body:has(#wallpaper-wrapper) .dropdown-content,
body:has(#wallpaper-wrapper) .float-panel,
body.wallpaper-transparent .dropdown-content,
body.wallpaper-transparent .float-panel {
    backdrop-filter: var(--glass-blur);
    background: rgba(255, 255, 255, 0.45) !important;
    box-shadow: var(--glass-shadow) !important;
    border: 1px solid var(--glass-border-light) !important;
}

@supports (-webkit-backdrop-filter: blur(1px)) {
    /* WebKit 兼容 */
    -webkit-backdrop-filter: var(--glass-blur);
}
```

### 暗色主题

```css
:root.dark .dropdown-content,
:root.dark .float-panel {
    background: rgba(30, 30, 30, 0.45) !important;
    box-shadow: var(--glass-shadow-dark) !important;
    border: 1px solid var(--glass-border-dark) !important;
}
```

---

## 四、全局毛玻璃样式（main.css L590-L776）

### 卡片玻璃化（main.css L608-L629）

```css
body.wallpaper-transparent .card-base,
body.wallpaper-transparent .card-base-transparent {
  backdrop-filter: var(--glass-blur);
  background: rgba(255, 255, 255, 0.52) !important;
  border: 1px solid var(--glass-border-light);
  box-shadow: var(--glass-shadow);
}

:root.dark body.wallpaper-transparent .card-base,
:root.dark body.wallpaper-transparent .card-base-transparent {
  background: rgba(30, 30, 30, 0.52) !important;
  border: 1px solid var(--glass-border-dark);
  box-shadow: var(--glass-shadow-dark);
}
```

### 按钮 / 下拉菜单 / 浮动面板玻璃化（main.css L632-L659）

```css
body.wallpaper-transparent .btn-card,
body.wallpaper-transparent .dropdown-content,
body.wallpaper-transparent .float-panel,
body.wallpaper-transparent .bg-(--card-bg),
body.wallpaper-transparent .post-list-layout-switch {
  backdrop-filter: var(--glass-blur);
  background: rgba(255, 255, 255, 0.42) !important;
  border: 1px solid var(--glass-border-light);
}

:root.dark ... {
  background: rgba(30, 30, 30, 0.42) !important;
  border: 1px solid var(--glass-border-dark);
}
```

### 导航栏玻璃化（main.css L661-L689）

```css
body.wallpaper-transparent #navbar[data-transparent-mode] > div {
  backdrop-filter: var(--glass-blur-strong);
  background: rgba(255, 255, 255, 0.42) !important;
  border: 1px solid var(--glass-border-light);
  border-radius: 1rem;
  box-shadow: var(--glass-shadow);
}

:root.dark ... {
  background: rgba(30, 30, 30, 0.42) !important;
  border: 1px solid var(--glass-border-dark);
  box-shadow: var(--glass-shadow-dark);
}

/* 滚动后阴影增强 */
body.wallpaper-transparent #navbar[data-transparent-mode].navbar-sticky-shadow > div {
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.18) !important;
}
```

### 文章正文玻璃化（main.css L692-L716）

```css
body.wallpaper-transparent #post-content,
body.wallpaper-transparent #post-container,
body.wallpaper-transparent .post-page-content {
  backdrop-filter: var(--glass-blur-soft);
  background: rgba(255, 255, 255, 0.48) !important;
  border: 1px solid var(--glass-border-light);
  border-radius: var(--radius-large);
  box-shadow: var(--glass-shadow);
}
```

### 侧边栏玻璃化（main.css L718-L731）

```css
body.wallpaper-transparent #left-sidebar .card-base,
body.wallpaper-transparent #right-sidebar .card-base,
body.wallpaper-transparent .widget-layout .card-base {
  backdrop-filter: var(--glass-blur-subtle);
}
```

### 评论区玻璃化（main.css L733-L749）

```css
body.wallpaper-transparent #post-comments {
  backdrop-filter: var(--glass-blur-subtle);
  background: rgba(255, 255, 255, 0.5) !important;
  border: 1px solid var(--glass-border-light);
}
```

### 悬停高亮（main.css L751-L760）

```css
body.wallpaper-transparent .card-base:hover,
body.wallpaper-transparent .btn-card:hover {
  background: rgba(255, 255, 255, 0.6) !important;
}

:root.dark ... {
  background: rgba(45, 45, 45, 0.6) !important;
}
```

### 过渡平滑（main.css L762-L776）

```css
body.wallpaper-transparent .card-base,
body.wallpaper-transparent .btn-card,
body.wallpaper-transparent .dropdown-content,
body.wallpaper-transparent .float-panel,
body.wallpaper-transparent #navbar[data-transparent-mode] > div,
body.wallpaper-transparent #post-content,
body.wallpaper-transparent #post-comments {
  transition:
    background-color 0.3s ease,
    border-color 0.3s ease,
    box-shadow 0.3s ease,
    backdrop-filter 0.3s ease;
}
```

---

## 五、壁纸配置（backgroundWallpaper.ts）

```ts
export const backgroundWallpaper = {
  mode: "overlay",          // 全屏透明壁纸模式
  overlay: {
    zIndex: -1,             // 壁纸层级
    opacity: 0.75,          // 壁纸透明度
    blur: 8,                 // 背景模糊度 (px)
    cardOpacity: 0.45,      // 卡片透明度 (0-1)
  },
  common: {
    navbar: {
      transparentMode: "semi",   // 导航栏透明模式: semi/full/semifull
      enableBlur: true,            // 是否开启毛玻璃模糊
      blur: 5,                    // 导航栏模糊度
    },
  },
};
```

---

## 六、导航栏配置（siteConfig.ts L67-L89）

```ts
navbar: {
  logo: {
    type: "image",
    value: "assets/images/firefly.png",
    alt: "🍀",
  },
  title: "Firefly",
  widthFull: false,         // 非全宽 → 保持悬浮胶囊形
  menuAlign: "center",     // 菜单居中
  followTheme: false,       // 不跟随主题色
  stickyNavbar: true,       // 固定顶部
}
```

---

## 七、导航栏容器结构（MainGridLayout.astro L316-L335）

```astro
<div
    id="top-row"
    class="z-50 pointer-events-none relative transition-all duration-700 mx-auto"
    class:list={[
        stickyNavbar ? "h-18 fixed top-0 left-0 right-0 z-80" : "",
        navbarWidthFull ? "" : "w-full xl:w-[92vw] max-w-(--page-width) px-0 md:px-4",
    ]}
>
    <div
        id="navbar-wrapper"
        class:list={[
            "pointer-events-auto transition-all",
            stickyNavbar ? "" : "sticky top-0",
        ]}
    >
        <Navbar></Navbar>
    </div>
</div>
```

---

## 八、参数调整指南

| 想要的效果 | 修改位置 | 参数 |
|-----------|---------|------|
| 导航栏上下间距 | `navbar.css` L40 | `margin: 0.6rem 1rem` → 改 `0.6rem` |
| 导航栏圆角大小 | `navbar.css` L38 | `border-radius: 9999px` → 改为如 `1rem` |
| 模糊强度 | `main.css` L602 | `--glass-blur: blur(18px)` → 改 `18px` |
| 卡片透明度 | `main.css` L612 | `rgba(255,255,255,0.52)` → 改 `0.52` |
| 导航栏透明度 | `main.css` L664 | `rgba(255,255,255,0.42)` → 改 `0.42` |
| 壁纸透明度 | `backgroundWallpaper.ts` L160 | `opacity: 0.75` |
| 壁纸模糊度 | `backgroundWallpaper.ts` L162 | `blur: 8` |
| 卡片透明度(配置) | `backgroundWallpaper.ts` L164 | `cardOpacity: 0.45` |
| 暗色背景色 | `main.css` L626 | `rgba(30,30,30,0.52)` → 改 RGB 值 |
| 阴影强度 | `main.css` L600 | `0 8px 32px rgba(0,0,0,0.12)` |
| 过渡动画速度 | `navbar.css` L24 | `0.36s` → 改秒数 |
```
