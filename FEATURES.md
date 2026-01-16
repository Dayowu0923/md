# 功能需求 - QM 戰情看板系統

> **專案特定需求**  
> 此文件包含 QM 專案的功能需求，可替換為其他專案需求

---

## 1. 專案概述

**專案名稱**: QM (戰情看板管理系統)

**目標**: 提供暗黑風格的戰情看板介面，具備炫光和液態玻璃特效

**核心功能**:

1. 使用者登入系統
2. 戰情看板儀表板 (Dashboard)

---

## 2. 技術棧

### 2.1 框架與工具

**已有**:

- Vue 3.5+ (Composition API)
- TypeScript 5.9+
- Vite 7.2+

**需要新增**:

- **Vue Router 4** - 路由管理
- **Pinia** - 狀態管理（用戶認證）
- **Axios** - API 請求
- **VueUse** - 組合式工具函數

**可選增強**:

- **GSAP** - 進階動畫（液態效果）
- **Chart.js / ECharts** - 數據圖表
- **Particles.js** - 粒子背景效果

### 2.2 專案結構

```
src/
├── main.ts
├── App.vue
├── router/
│   └── index.ts              # 路由配置
├── stores/
│   └── auth.ts               # 認證狀態
├── views/
│   ├── LoginView.vue         # 登入頁
│   └── DashboardView.vue     # 戰情看板
├── components/
│   ├── common/
│   │   ├── GlassCard.vue     # 玻璃卡片組件
│   │   ├── GlowButton.vue    # 炫光按鈕
│   │   └── LoadingSpinner.vue
│   ├── login/
│   │   ├── LoginForm.vue
│   │   └── AnimatedBackground.vue
│   └── dashboard/
│       ├── DashboardHeader.vue
│       ├── StatCard.vue      # 數據卡片
│       └── ChartPanel.vue
├── composables/
│   └── useAuth.ts            # 認證邏輯
├── api/
│   └── auth.ts               # API 請求
├── types/
│   └── index.ts              # TypeScript 類型
├── directives/
│   └── clickOutside.ts       # 自定義指令
└── styles/
    ├── variables.css         # CSS 變數
    ├── glassmorphism.css     # 玻璃效果
    └── animations.css        # 動畫效果
```

---

## 3. 頁面與功能需求

### 3.1 登入頁 (LoginView)

#### 頁面元素

- 登入表單（帳號、密碼）
- 「記住我」選項
- 忘記密碼連結（未來功能）
- 登入按鈕（炫光特效）
- 背景：動態液態玻璃效果 + 漸變光效

#### 佈局結構

```
┌────────────────────────────────────┐
│                                    │
│    [動態背景 + 液態光效]            │
│                                    │
│         ┌────────────┐             │
│         │  登入表單   │ (玻璃卡片)  │
│         │  + 炫光邊框 │             │
│         └────────────┘             │
│                                    │
└────────────────────────────────────┘
```

#### 互動流程

1. 用戶輸入帳號密碼
2. 點擊登入按鈕（按鈕發光動畫）
3. 驗證中顯示加載特效
4. 成功後路由跳轉到 Dashboard（過場動畫）
5. 失敗顯示錯誤提示（震動 + 紅色光暈）

---

### 3.2 戰情看板頁 (DashboardView)

#### 頁面元素

- 頂部導航列（Logo、搜尋、通知、用戶頭像）
- 數據卡片區域（玻璃質感）
  - 即時數據統計
  - 動態圖表
  - 狀態指示器
- 響應式網格佈局
- 霓虹光暈效果

#### 佈局結構

```
┌──────────────────────────────────────┐
│  Header (Logo | 搜尋 | 通知 | 頭像)  │
├──────────────────────────────────────┤
│                                      │
│  ┌─────────┐ ┌─────────┐ ┌────────┐ │
│  │ 卡片 1  │ │ 卡片 2  │ │ 卡片 3 │ │
│  │ (玻璃)  │ │ (玻璃)  │ │(玻璃) │ │
│  └─────────┘ └─────────┘ └────────┘ │
│                                      │
│  ┌──────────────────────────────┐   │
│  │   圖表區域 (液態玻璃背景)      │   │
│  └──────────────────────────────┘   │
│                                      │
└──────────────────────────────────────┘
```

#### 互動流程

**登出流程**:

1. 點擊用戶頭像 → 下拉選單
2. 選擇「登出」
3. 清除 token
4. 返回登入頁（淡出動畫）

**數據更新**:

- 數字滾動動畫
- 光暈脈衝提示
- 自動刷新（每 30 秒）

---

## 4. 數據流與狀態管理

### 4.1 認證 Store (Pinia)

```typescript
// stores/auth.ts
interface AuthState {
  user: User | null;
  token: string | null;
  isAuthenticated: boolean;
}

interface User {
  id: string;
  username: string;
  email: string;
  avatar?: string;
}

// Actions
login(username: string, password: string): Promise<void>
logout(): void
checkAuth(): boolean
```

### 4.2 本地存儲

```typescript
// 存儲 token
localStorage.setItem("auth_token", token);
localStorage.setItem("remember_me", "true");

// 讀取 token
const token = localStorage.getItem("auth_token");
```

---

## 5. API 端點

### 5.1 認證 API

#### 登入

```
POST /api/auth/login
Content-Type: application/json

Body:
{
  "username": "admin",
  "password": "admin123"
}

Response:
{
  "token": "eyJhbGc...",
  "user": {
    "id": "1",
    "username": "admin",
    "email": "admin@qm.com",
    "avatar": "https://..."
  }
}
```

#### 登出

```
POST /api/auth/logout
Authorization: Bearer <token>

Response:
{
  "success": true
}
```

---

### 5.2 Dashboard API

#### 獲取戰情數據

```
GET /api/dashboard/stats
Authorization: Bearer <token>

Response:
{
  "stats": [
    {
      "title": "線上用戶",
      "value": 1234,
      "trend": "+12%",
      "color": "#00f0ff"
    },
    {
      "title": "系統狀態",
      "value": "99.9%",
      "trend": "stable",
      "color": "#00ff87"
    },
    {
      "title": "告警數量",
      "value": 3,
      "trend": "-5",
      "color": "#ff006e"
    },
    {
      "title": "響應時間",
      "value": "45ms",
      "trend": "-10%",
      "color": "#8b5cf6"
    }
  ]
}
```

---

## 6. 專案特定組件

### 6.1 LoginForm.vue

**功能**:

- 表單驗證（帳號、密碼必填）
- 記住我 checkbox
- 登入按鈕（loading 狀態）
- 錯誤提示

**Props**:

```typescript
// 無需 props，使用 composable
```

**使用 Composable**:

```typescript
const { login, isLoading, error } = useAuth();
```

---

### 6.2 DashboardHeader.vue

**功能**:

- 顯示 Logo
- 搜尋框（未來功能）
- 通知圖標 + 數量徽章
- 用戶頭像下拉選單

**Props**:

```typescript
interface HeaderProps {
  user: User;
}
```

---

### 6.3 StatCard.vue (數據卡片)

**功能**:

- 顯示數據標題
- 大數字（炫光效果）
- 變化趨勢箭頭
- 迷你圖表（可選）

**Props**:

```typescript
interface StatCardProps {
  title: string;
  value: string | number;
  trend?: string;
  color?: string;
  icon?: string;
}
```

**動畫**:

- 數字滾動動畫
- 數據更新時光暈脈衝

---

## 7. 路由配置

```typescript
// router/index.ts
import { createRouter, createWebHistory } from "vue-router";
import { useAuthStore } from "@/stores/auth";

const routes = [
  {
    path: "/login",
    name: "Login",
    component: () => import("@/views/LoginView.vue"),
    meta: { requiresAuth: false },
  },
  {
    path: "/",
    name: "Dashboard",
    component: () => import("@/views/DashboardView.vue"),
    meta: { requiresAuth: true },
  },
  {
    path: "/:pathMatch(.*)*",
    redirect: "/login",
  },
];

const router = createRouter({
  history: createWebHistory(),
  routes,
});

// 路由守衛
router.beforeEach((to, from, next) => {
  const authStore = useAuthStore();

  if (to.meta.requiresAuth && !authStore.isAuthenticated) {
    next("/login");
  } else if (to.name === "Login" && authStore.isAuthenticated) {
    next("/");
  } else {
    next();
  }
});

export default router;
```

---

## 8. Mock 數據

### 8.1 登入 Mock

```typescript
// api/auth.ts (開發階段)
const mockLogin = {
  username: "admin",
  password: "admin123",
  response: {
    token: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    user: {
      id: "1",
      username: "admin",
      email: "admin@qm.com",
      avatar: "https://api.dicebear.com/7.x/avataaars/svg?seed=admin",
    },
  },
};

export async function login(username: string, password: string): Promise<LoginResponse> {
  // 模擬 API 延遲
  await new Promise((resolve) => setTimeout(resolve, 1000));

  if (username === mockLogin.username && password === mockLogin.password) {
    return mockLogin.response;
  } else {
    throw new Error("帳號或密碼錯誤");
  }
}
```

### 8.2 Dashboard Mock 數據

```typescript
// api/dashboard.ts (開發階段)
export const mockDashboardStats = [
  { title: "線上用戶", value: 1234, trend: "+12%", color: "#00f0ff" },
  { title: "系統狀態", value: "99.9%", trend: "stable", color: "#00ff87" },
  { title: "告警數量", value: 3, trend: "-5", color: "#ff006e" },
  { title: "響應時間", value: "45ms", trend: "-10%", color: "#8b5cf6" },
];

export async function getDashboardStats() {
  await new Promise((resolve) => setTimeout(resolve, 800));
  return mockDashboardStats;
}
```

---

## 9. 開發階段規劃

### Phase 1: 基礎設置 (Day 1-2)

- ✅ 安裝依賴 (Router, Pinia, Axios)
- ✅ 設置路由和狀態管理
- ✅ 建立 CSS 變數和基礎樣式
- ✅ 實現玻璃效果和炫光樣式

### Phase 2: 登入頁 (Day 3-4)

- ✅ AnimatedBackground 組件
- ✅ LoginForm 表單和驗證
- ✅ GlowButton 組件
- ✅ 登入邏輯和 API 整合
- ✅ 錯誤處理和動畫

### Phase 3: 戰情看板 (Day 5-7)

- ✅ DashboardView 佈局
- ✅ GlassCard 組件
- ✅ StatCard 數據卡片
- ✅ 圖表整合
- ✅ 響應式調整

### Phase 4: 優化和完善 (Day 8)

- ✅ 動畫流暢度優化
- ✅ 響應式測試
- ✅ 可訪問性改進
- ✅ 性能檢測

---

## 10. 特殊需求

### 10.1 認證邏輯

- JWT Token 存儲在 localStorage
- 路由守衛：未登入自動跳轉登入頁
- Token 過期處理（自動登出）
- 記住我功能（7 天有效期）

### 10.2 錯誤處理

```typescript
// 登入錯誤
interface LoginError {
  message: string;
  code: "INVALID_CREDENTIALS" | "NETWORK_ERROR" | "SERVER_ERROR";
}

// 顯示錯誤
const showError = (error: LoginError) => {
  // 震動 + 紅色光暈動畫
  errorMessage.value = error.message;
  playShakeAnimation();
};
```

### 10.3 自動登出

```typescript
// 監聽 token 過期
watch(
  () => authStore.tokenExpired,
  (expired) => {
    if (expired) {
      authStore.logout();
      router.push("/login");
      showNotification("登入已過期，請重新登入");
    }
  }
);
```

---

## 11. TypeScript 類型定義

```typescript
// types/index.ts

export interface User {
  id: string;
  username: string;
  email: string;
  avatar?: string;
}

export interface LoginRequest {
  username: string;
  password: string;
}

export interface LoginResponse {
  token: string;
  user: User;
}

export interface DashboardStat {
  title: string;
  value: string | number;
  trend?: string;
  color?: string;
  icon?: string;
}

export interface ApiError {
  message: string;
  code: string;
  status: number;
}
```

---

## 12. 開發注意事項

### 12.1 TypeScript 類型安全

**✅ 推薦做法**:

```typescript
// 正確的 Event 類型
const handleSubmit = (e: Event) => {
  e.preventDefault();
  const form = e.target as HTMLFormElement;
  // ...
};

// 使用 defineModel (Vue 3.4+)
const username = defineModel<string>("username");
const password = defineModel<string>("password");
```

**❌ 避免使用 `any`**:

```typescript
// 不好
const handleClick = (event: any) => {
  // ...
};

// 好
const handleClick = (event: MouseEvent) => {
  // ...
};
```

---

### 12.2 Composable 定義

```typescript
// composables/useAuth.ts
import { useAuthStore } from "@/stores/auth";
import type { LoginRequest } from "@/types";

export function useAuth() {
  const authStore = useAuthStore();
  const router = useRouter();

  const login = async (credentials: LoginRequest) => {
    try {
      await authStore.login(credentials.username, credentials.password);
      router.push("/");
    } catch (error) {
      throw error;
    }
  };

  const logout = () => {
    authStore.logout();
    router.push("/login");
  };

  return {
    user: computed(() => authStore.user),
    isAuthenticated: computed(() => authStore.isAuthenticated),
    isLoading: computed(() => authStore.isLoading),
    login,
    logout,
  };
}
```

---

## 13. 測試帳號

### 開發階段測試帳號

```
帳號: admin
密碼: admin123

帳號: user
密碼: user123
```

---

**準備開始實作 QM 戰情看板系統！** 🚀
