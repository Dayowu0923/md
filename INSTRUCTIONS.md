# 前端開發 Instructions

> **文件索引**  
> 此文件已重組，請參考以下文件：

---

## 📁 文件結構

### 1. [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md) - 設計系統

**通用設計規範（可重用於任何專案）**

包含內容：

- 暗黑液態玻璃設計風格
- 顏色系統與 CSS 變數
- 液態玻璃效果規範
- 炫光動畫效果庫
- 響應式設計規範
- 通用組件規格（GlassCard、GlowButton、LoadingSpinner 等）
- 主題系統
- 可訪問性規範
- 效能優化指南

**用途**: 將此設計系統應用於任何需要類似視覺風格的專案

---

### 2. [FEATURES.md](./FEATURES.md) - 功能需求

**QM 專案特定功能需求**

包含內容：

- 專案概述與核心功能
- 技術棧與專案結構
- 頁面與功能需求（登入頁、戰情看板）
- 數據流與狀態管理
- API 端點定義
- 專案特定組件
- 路由配置
- Mock 數據
- 開發階段規劃

**用途**: 替換此文件即可將設計系統應用於其他專案

---

## 🚀 快速開始

### 新專案使用此設計系統

1. **複製設計系統文件**

   ```bash
   cp DESIGN_SYSTEM.md ../new-project/docs/
   ```

2. **創建新的功能需求文件**

   - 複製 `FEATURES.md` 作為模板
   - 修改為新專案的功能需求
   - 保持專案結構一致

3. **導入設計系統樣式**

   ```bash
   # 複製通用樣式檔案
   cp -r styles/ ../new-project/src/styles/
   ```

4. **重用通用組件**
   ```bash
   # 複製通用組件
   cp components/common/* ../new-project/src/components/common/
   ```

---

## 📋 開發最佳實踐

### TypeScript 類型安全

**✅ 正確的做法**:

```typescript
// 使用正確的 Event 類型
const handleClick = (event: MouseEvent) => {
  const target = event.target as HTMLElement;
  // ...
};

// 使用 defineModel (Vue 3.4+)
const model = defineModel<string>();
```

**❌ 避免使用 `any`**:

```typescript
// 不推薦
const handleEvent = (event: any) => {
  /* ... */
};
```

---

### Vue 3 組件規範

**✅ 使用 `defineModel`**:

```vue
<script setup lang="ts">
const username = defineModel<string>("username");
const password = defineModel<string>("password");
</script>

<template>
  <input v-model="username" />
  <input v-model="password" type="password" />
</template>
```

---

### HTML 標籤規範

**✅ 正確的標籤寫法**:

```vue
<template>
  <!-- 一般標籤必須有開頭和結尾 -->
  <div class="container"></div>
  <span></span>

  <!-- 只有 Vue 組件和特定元素可自閉合 -->
  <GlassCard />
  <input type="text" />
  <img src="..." />
</template>
```

---

## 🔧 技術要求（通用）

### 核心技術

- **Vue 3.5+** (Composition API + `<script setup>`)
- **TypeScript 5.9+**
- **Vite 7.2+**
- **Vue Router 4**
- **Pinia** (狀態管理)

### 推薦工具

- **VueUse** - 組合式工具函數
- **Axios** - API 請求
- **GSAP** (可選) - 進階動畫
- **Chart.js / ECharts** (可選) - 圖表

---

## 📦 專案結構（建議）

```
src/
├── main.ts
├── App.vue
├── router/           # 路由配置
├── stores/           # Pinia 狀態管理
├── views/            # 頁面組件
├── components/
│   ├── common/       # 通用組件（可重用）
│   └── [feature]/    # 功能特定組件
├── composables/      # 組合式函數
├── api/              # API 請求
├── types/            # TypeScript 類型
├── directives/       # 自定義指令
└── styles/           # 全局樣式
    ├── variables.css
    ├── glassmorphism.css
    └── animations.css
```

---

## ✅ 開發檢查清單

開發前請確認：

- [ ] 所有 Event 參數都有正確的類型（不使用 `any`）
- [ ] HTML 標籤都有正確的開頭和結尾
- [ ] 優先使用 `defineModel` 而非 `defineProps` + `emit`
- [ ] Props 都有 TypeScript interface 定義
- [ ] 自定義指令有完整的類型標註
- [ ] CSS 動畫使用 `transform` 和 `opacity`
- [ ] 組件文件名使用 PascalCase
- [ ] 沒有未使用的變數和 import
- [ ] 遵循 Conventional Commits 規範

---

## 📚 相關文件

### API 文件

- [API.md](./API.md) - API 端點詳細說明

### 組件文件

- [COMPONENTS.md](./COMPONENTS.md) - 組件使用說明

---

## 🎯 設計參考

**風格靈感**:

- Cyberpunk 2077 UI (霓虹炫光)
- Apple macOS Big Sur (玻璃質感)
- Stripe Dashboard (暗黑模式)
- Glassmorphism.com (液態玻璃範例)

**動畫參考**:

- Awwwards.com (創意動畫)
- CodePen (搜尋 "glassmorphism", "neon glow")
- Dribbble (UI 動畫設計)

**準備開始實作！** 🚀

---

## 💡 使用指南

### 對於 QM 專案開發者

1. 閱讀 [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md) 了解設計風格
2. 閱讀 [FEATURES.md](./FEATURES.md) 了解功能需求
3. 參考上方檢查清單進行開發

### 對於其他專案

1. 保留 [DESIGN_SYSTEM.md](./DESIGN_SYSTEM.md)（設計系統）
2. 替換 [FEATURES.md](./FEATURES.md)（功能需求）
3. 調整專案結構以符合新需求

---

**文件版本**: 2.0 (已模組化)  
**最後更新**: 2026-01-16

---

## 📖 舊版內容已遷移

以下內容已從此文件移除並重新組織：

### 已移至 DESIGN_SYSTEM.md

- 第 3 節：UI/UX 規範
- 第 3.1-3.5 節：設計風格、動畫效果、佈局結構等
- 第 6 節：核心組件規格
- 第 11 節：開發注意事項與最佳實踐（部分）

### 已移至 FEATURES.md

- 第 2 節：功能需求
- 第 4 節：技術要求
- 第 5 節：數據流和狀態
- 第 7 節：特殊需求
- 第 8 節：開發階段
- 第 10 節：Mock 數據範例

如需查看完整的舊版內容，請使用 Git 歷史記錄：

```bash
git log --follow INSTRUCTIONS.md
```
