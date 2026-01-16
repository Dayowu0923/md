# 設計系統 - 暗黑液態玻璃風格

> **通用設計規範**  
> 此文件包含可重用的視覺設計系統，適用於各種專案

---

## 1. 設計風格：暗黑液態玻璃 (Dark Glassmorphism)

### 1.1 顏色系統

```css
/* 背景 */
--bg-primary: #0a0a0f; /* 深邃黑 */
--bg-secondary: #13131a; /* 次級背景 */

/* 玻璃效果 */
--glass-bg: rgba(255, 255, 255, 0.05);
--glass-border: rgba(255, 255, 255, 0.1);
--glass-shadow: rgba(0, 0, 0, 0.5);

/* 炫光色 */
--glow-primary: #00f0ff; /* 青色炫光 */
--glow-secondary: #ff006e; /* 粉紅炫光 */
--glow-accent: #8b5cf6; /* 紫色炫光 */
--glow-success: #00ff87; /* 綠色炫光 */

/* 文字 */
--text-primary: #ffffff;
--text-secondary: rgba(255, 255, 255, 0.7);
--text-muted: rgba(255, 255, 255, 0.4);
```

---

## 2. 液態玻璃效果規範

### 2.1 玻璃卡片樣式

```css
background: rgba(255, 255, 255, 0.05);
backdrop-filter: blur(20px) saturate(180%);
border: 1px solid rgba(255, 255, 255, 0.1);
border-radius: 16px;
box-shadow: 0 8px 32px rgba(0, 0, 0, 0.4), inset 0 1px 0 rgba(255, 255, 255, 0.1);
```

### 2.2 炫光效果

```css
/* 按鈕炫光 */
box-shadow: 0 0 20px rgba(0, 240, 255, 0.5), 0 0 40px rgba(0, 240, 255, 0.3),
  inset 0 0 10px rgba(0, 240, 255, 0.2);

/* hover 時加強 */
box-shadow: 0 0 30px rgba(0, 240, 255, 0.8), 0 0 60px rgba(0, 240, 255, 0.5),
  0 0 100px rgba(0, 240, 255, 0.3);
```

### 2.3 漸變背景

```css
/* 動態漸變 */
background: linear-gradient(135deg, #0a0a0f 0%, #1a1a2e 50%, #0a0a0f 100%);
background-size: 200% 200%;
animation: gradientShift 15s ease infinite;

@keyframes gradientShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}
```

---

## 3. 動畫效果庫

### 3.1 背景動畫

#### 液態光斑漂浮

```css
.liquid-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.3;
  animation: float 20s ease-in-out infinite;
}

@keyframes float {
  0%,
  100% {
    transform: translate(0, 0) scale(1);
  }
  33% {
    transform: translate(30px, -30px) scale(1.1);
  }
  66% {
    transform: translate(-20px, 20px) scale(0.9);
  }
}
```

#### 玻璃碎片反射

```css
.glass-shard {
  position: absolute;
  background: linear-gradient(45deg, transparent, rgba(255, 255, 255, 0.1), transparent);
  animation: shimmer 3s ease-in-out infinite;
}

@keyframes shimmer {
  0%,
  100% {
    opacity: 0;
  }
  50% {
    opacity: 1;
  }
}
```

### 3.2 按鈕互動動畫

```css
.glow-button {
  transition: all 0.3s ease;
}

/* Hover: 炫光增強 + 輕微放大 */
.glow-button:hover {
  transform: scale(1.05);
  box-shadow: 0 0 30px var(--glow-color), 0 0 60px var(--glow-color), 0 0 100px var(--glow-color);
}

/* Active: 內縮效果 */
.glow-button:active {
  transform: scale(0.95);
}

/* Loading: 旋轉光環 */
@keyframes pulse {
  0%,
  100% {
    box-shadow: 0 0 20px var(--glow-color);
  }
  50% {
    box-shadow: 0 0 40px var(--glow-color);
  }
}

.glow-button.loading {
  animation: pulse 1.5s ease-in-out infinite;
}
```

### 3.3 頁面過場動畫

```css
/* 淡入淡出 + 模糊 */
.page-enter-active,
.page-leave-active {
  transition: all 0.3s ease;
}

.page-enter-from {
  opacity: 0;
  filter: blur(10px);
  transform: translateY(20px);
}

.page-leave-to {
  opacity: 0;
  filter: blur(10px);
  transform: translateY(-20px);
}
```

### 3.4 卡片依序彈入 (Stagger)

```css
.card-enter-active {
  animation: slideIn 0.5s ease backwards;
}

.card-enter-active:nth-child(1) {
  animation-delay: 0.1s;
}
.card-enter-active:nth-child(2) {
  animation-delay: 0.2s;
}
.card-enter-active:nth-child(3) {
  animation-delay: 0.3s;
}

@keyframes slideIn {
  from {
    opacity: 0;
    transform: translateY(30px) scale(0.9);
  }
  to {
    opacity: 1;
    transform: translateY(0) scale(1);
  }
}
```

### 3.5 數據更新動畫

```css
/* 數字滾動 */
.number-counter {
  transition: all 0.5s ease;
}

/* 光暈脈衝提示 */
@keyframes dataPulse {
  0% {
    box-shadow: 0 0 10px var(--glow-success);
  }
  50% {
    box-shadow: 0 0 30px var(--glow-success);
  }
  100% {
    box-shadow: 0 0 10px var(--glow-success);
  }
}

.data-updated {
  animation: dataPulse 0.8s ease;
}
```

---

## 4. 響應式設計規範

### 4.1 斷點系統

```css
/* 移動裝置 */
@media (max-width: 767px) {
  /* 單列堆疊 */
  .grid {
    grid-template-columns: 1fr;
  }
}

/* 平板 */
@media (min-width: 768px) and (max-width: 1023px) {
  /* 2 列佈局 */
  .grid {
    grid-template-columns: repeat(2, 1fr);
  }
}

/* 桌面 */
@media (min-width: 1024px) {
  /* 3-4 列網格佈局 */
  .grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  }
}
```

### 4.2 流式網格系統

```css
.responsive-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 24px;
  padding: 24px;
}
```

---

## 5. 通用組件規格

### 5.1 GlassCard (玻璃卡片)

**Props**:

```typescript
interface GlassCardProps {
  blur?: number; // 模糊度 (預設 20)
  glow?: boolean; // 是否顯示炫光邊框
  glowColor?: string; // 炫光顏色
  padding?: string; // 內距
}
```

**樣式結構**:

```vue
<template>
  <div class="glass-card" :class="{ glow: glow }" :style="cardStyle">
    <slot />
  </div>
</template>

<style scoped>
.glass-card {
  background: var(--glass-bg);
  backdrop-filter: blur(var(--blur, 20px)) saturate(180%);
  border: 1px solid var(--glass-border);
  border-radius: 16px;
  box-shadow: 0 8px 32px var(--glass-shadow);
  padding: var(--padding, 24px);
  transition: all 0.3s ease;
}

.glass-card.glow {
  border-color: var(--glow-color);
  box-shadow: 0 0 20px var(--glow-color), 0 8px 32px var(--glass-shadow);
}

.glass-card:hover {
  transform: translateY(-2px);
  box-shadow: 0 12px 48px var(--glass-shadow);
}
</style>
```

---

### 5.2 GlowButton (炫光按鈕)

**Props**:

```typescript
interface GlowButtonProps {
  type?: "primary" | "secondary";
  loading?: boolean;
  glowIntensity?: "low" | "medium" | "high";
  disabled?: boolean;
}
```

**樣式結構**:

```vue
<template>
  <button
    class="glow-button"
    :class="[type, glowIntensity, { loading, disabled }]"
    :disabled="disabled || loading"
  >
    <span v-if="loading" class="spinner"></span>
    <span class="button-content">
      <slot />
    </span>
  </button>
</template>

<style scoped>
.glow-button {
  position: relative;
  padding: 12px 32px;
  font-size: 16px;
  font-weight: 600;
  color: var(--text-primary);
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid var(--glow-primary);
  border-radius: 8px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 0 20px rgba(0, 240, 255, 0.3);
}

.glow-button.primary {
  background: linear-gradient(135deg, var(--glow-primary), var(--glow-accent));
  border: none;
}

.glow-button:hover:not(.disabled) {
  transform: scale(1.05);
  box-shadow: 0 0 30px var(--glow-primary), 0 0 60px var(--glow-primary);
}

.glow-button.loading .spinner {
  /* 旋轉動畫 */
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>
```

---

### 5.3 LoadingSpinner (載入動畫)

```vue
<template>
  <div class="loading-spinner" :class="size">
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
    <div class="spinner-ring"></div>
  </div>
</template>

<style scoped>
.loading-spinner {
  position: relative;
  width: 48px;
  height: 48px;
}

.spinner-ring {
  position: absolute;
  width: 100%;
  height: 100%;
  border: 3px solid transparent;
  border-top-color: var(--glow-primary);
  border-radius: 50%;
  animation: spin 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
}

.spinner-ring:nth-child(1) {
  animation-delay: -0.45s;
}
.spinner-ring:nth-child(2) {
  animation-delay: -0.3s;
}
.spinner-ring:nth-child(3) {
  animation-delay: -0.15s;
}
</style>
```

---

### 5.4 AnimatedBackground (動態背景)

```vue
<template>
  <div class="animated-background">
    <div class="gradient-bg"></div>
    <div class="liquid-orb orb-1"></div>
    <div class="liquid-orb orb-2"></div>
    <div class="liquid-orb orb-3"></div>
  </div>
</template>

<style scoped>
.animated-background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  z-index: -1;
}

.gradient-bg {
  position: absolute;
  width: 100%;
  height: 100%;
  background: linear-gradient(135deg, #0a0a0f 0%, #1a1a2e 50%, #0a0a0f 100%);
  background-size: 200% 200%;
  animation: gradientShift 15s ease infinite;
}

.liquid-orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.3;
}

.orb-1 {
  width: 400px;
  height: 400px;
  background: var(--glow-primary);
  top: -100px;
  left: -100px;
  animation: float 20s ease-in-out infinite;
}

.orb-2 {
  width: 350px;
  height: 350px;
  background: var(--glow-secondary);
  bottom: -100px;
  right: -100px;
  animation: float 25s ease-in-out infinite reverse;
}

.orb-3 {
  width: 300px;
  height: 300px;
  background: var(--glow-accent);
  top: 50%;
  left: 50%;
  animation: float 30s ease-in-out infinite;
}
</style>
```

---

## 6. 主題系統 (可選)

### 6.1 暗黑主題 (預設)

```css
:root[data-theme="dark"] {
  --bg-primary: #0a0a0f;
  --bg-secondary: #13131a;
  --text-primary: #ffffff;
  /* ... 其他暗黑變數 */
}
```

### 6.2 亮色主題 (預留)

```css
:root[data-theme="light"] {
  --bg-primary: #f5f5f7;
  --bg-secondary: #ffffff;
  --text-primary: #1d1d1f;
  --glass-bg: rgba(255, 255, 255, 0.7);
  /* ... 調整為亮色 */
}
```

---

## 7. 可訪問性規範

### 7.1 對比度要求

- 確保炫光效果下文字對比度 ≥ 4.5:1
- 重要按鈕對比度 ≥ 7:1

### 7.2 動畫偏好設定

```css
@media (prefers-reduced-motion: reduce) {
  * {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

### 7.3 鍵盤導航

```css
/* 確保 focus 狀態可見 */
:focus-visible {
  outline: 2px solid var(--glow-primary);
  outline-offset: 2px;
}

button:focus-visible {
  box-shadow: 0 0 0 3px var(--glow-primary);
}
```

---

## 8. 效能優化指南

### 8.1 動畫效能

**✅ 推薦使用 GPU 加速屬性**:

- `transform`
- `opacity`
- `filter` (謹慎使用)

**❌ 避免**:

- `width`, `height`
- `top`, `left`, `right`, `bottom`
- 過多的 `box-shadow` 動畫

### 8.2 `will-change` 使用

```css
/* 只在需要時使用 */
.animated-element {
  will-change: transform, opacity;
}

/* 動畫結束後移除 */
.animated-element.animation-done {
  will-change: auto;
}
```

### 8.3 模糊效果優化

```css
/* 使用固定像素值而非百分比 */
backdrop-filter: blur(20px); /* ✅ 好 */
backdrop-filter: blur(2rem); /* ❌ 較慢 */
```

---

## 9. 設計參考資源

### 9.1 風格靈感

- **Cyberpunk 2077 UI** - 霓虹炫光效果
- **Apple macOS Big Sur** - 玻璃質感
- **Stripe Dashboard** - 暗黑模式設計
- **Glassmorphism.com** - 液態玻璃範例

### 9.2 動畫參考

- **Awwwards.com** - 創意動畫範例
- **CodePen** - 搜尋 "glassmorphism", "neon glow"
- **Dribbble** - UI 動畫設計

### 9.3 工具推薦

- **Glassmorphism Generator** - css.glass
- **Gradient Generator** - cssgradient.io
- **Color Palette** - coolors.co
- **Animation Timeline** - animista.net

---

## 10. 開發最佳實踐

### 10.1 CSS 組織

```
styles/
├── variables.css       # CSS 變數定義
├── glassmorphism.css   # 玻璃效果樣式
├── animations.css      # 動畫效果
├── responsive.css      # 響應式斷點
└── utilities.css       # 工具類別
```

### 10.2 CSS 變數使用

```vue
<script setup lang="ts">
const glowColor = ref("#00f0ff");

const cardStyle = computed(() => ({
  "--glow-color": glowColor.value,
}));
</script>

<template>
  <div class="glass-card" :style="cardStyle">
    <!-- 內容 -->
  </div>
</template>
```

### 10.3 組件封裝原則

- **單一職責**: 每個組件只做一件事
- **可組合性**: 通過 props 和 slots 靈活組合
- **樣式隔離**: 使用 scoped CSS
- **類型安全**: 使用 TypeScript 定義 props

---

**此設計系統可應用於任何需要暗黑液態玻璃風格的專案！** ✨
