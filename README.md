# Beacon | Tasks & Analytics Dashboard

[![React](https://img.shields.io/badge/React-19.0-61DAFB?logo=react&logoColor=black)](https://react.dev/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-3178C6?logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Vite](https://img.shields.io/badge/Vite-8.0-646CFF?logo=vite&logoColor=white)](https://vitejs.dev/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-v4.0-06B6D4?logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![Zustand](https://img.shields.io/badge/State-Zustand-764ABC)](https://zustand-demo.pmnd.rs/)
[![TanStack Query](https://img.shields.io/badge/TanStack_Query-v5-FF4154?logo=reactquery&logoColor=white)](https://tanstack.com/)

Beacon is a responsive tasks and analytics dashboard built with **React 19**, **TypeScript**, **Vite**, **Tailwind CSS v4**, **Zustand**, and **TanStack Query**. Designed with clean frontend architecture patterns, modular state management, route-level code splitting, and resilient error boundaries.

---

## Key Highlights

- **Authentication Guard & Session Persistence**: Client-side route protection using React Router with session state persistence powered by Zustand authentication stores.
- **3D Spatial Telemetry**: Interactive WebGL global activity globe rendered via Cobe with Framer Motion spring physics.
- **Interactive Data Visualizations**: Multi-view analytics powered by Recharts (Area, Bar, Pie, Radar, and Line charts) with dark and light theme integration.
- **Task & Team Workflows**: Feature-rich data tables with real-time filtering, searching, sorting, and modal CRUD operations.
- **Resilient Error Recovery**: Application-level React ErrorBoundary wrappers and Suspense fallback states for graceful loading and exception handling.

---

## Technical Challenges & Solutions

### 1. Code Splitting & Dynamic Bundle Optimization

- **The Problem:** Importing all dashboard pages and heavy visualization libraries (Recharts, Cobe WebGL Globe, Data Tables) into a single monolithic bundle degraded initial page load performance (FCP/LCP), forcing users to download code for unvisited views.
- **The Solution:** Implemented route-level dynamic code splitting using React `lazy()` imports inside `src/pages/index.ts` paired with React `Suspense` boundaries in `App.tsx`:
  ```typescript
  import { lazy } from "react";

  export const LoginPage = lazy(() => import("./LoginPage"));
  export const OverviewPage = lazy(() => import("./OverviewPage"));
  export const AnalyticsPage = lazy(() => import("./AnalyticsPage"));
  export const TasksPage = lazy(() => import("./TasksPage"));
  export const TeamPage = lazy(() => import("./TeamPage"));
  export const AccountPage = lazy(() => import("./AccountPage"));
  export const SettingsPage = lazy(() => import("./SettingsPage"));
  ```
  Vite automatically splits these pages into isolated JavaScript chunks loaded on demand when routes are navigated, significantly reducing initial load times.

### 2. Zustand State Management & Prop-Drilling Elimination

- **The Problem:** Managing complex interactive states across task search filters, team member lists, modal dialog toggles, sidebar navigation states, and authentication across deep component trees created excessive prop-drilling and triggered unnecessary full-page re-renders.
- **The Solution:** Engineered a modular **Zustand** store architecture separating transient UI states from persistent user sessions:
  - `useAuthStore`: Manages user credentials, login/logout actions, and persistent storage (`user-storage`).
  - `useSidebarStore`: Controls responsive sidebar collapse and expansion.
  - `useModalStore`: Handles task creation and edit modal open/close states.
  - `useTaskFilterStore`: Manages task search queries, category filters, and priority sorting.

  By utilizing atomic selector subscriptions (e.g., `const user = useAuthStore((state) => state.user)`), components re-render strictly when their specific slice of state mutates.

---

## Project Structure

```
reactDashboard/
├── src/
│   ├── api/             # API client configuration and query handlers
│   ├── assets/          # Static SVG graphics, avatars, and visual assets
│   ├── components/      # Reusable UI components
│   │   ├── charts/      # Recharts visualizations (Area, Bar, Pie, Radar, Line)
│   │   ├── layout/      # Application layout, sidebar, and navbar components
│   │   ├── ui/          # Base UI primitives (Tables, Dropdowns, Modals, Globe)
│   │   ├── ErrorBoundary.tsx
│   │   ├── Globe.tsx    # 3D WebGL Globe container
│   │   ├── ProtectedRoute.tsx
│   │   └── ThemeProvider.tsx
│   ├── consts/          # Application constants
│   ├── data/            # Mock datasets and schema definitions
│   ├── hooks/           # Custom React hooks
│   ├── lib/             # Utility helpers and QueryClient configuration
│   ├── pages/           # Lazy-loaded page components (Overview, Analytics, Tasks, Team, etc.)
│   ├── stores/          # Zustand stores (useAuthStore, useSidebarStore, etc.)
│   ├── App.tsx          # Root routing and Suspense configuration
│   └── main.tsx         # Application entry point and providers
├── public/              # Static public assets and favicon
├── index.html           # HTML entry point
├── package.json
└── vite.config.ts       # Vite configuration and React Compiler plugin
```

---

## Getting Started

### Prerequisites

Ensure you have the following installed on your machine:

- **Node.js**: `v18.0.0` or higher
- **npm**: `v9.0.0` or higher (or `pnpm` / `yarn`)

### Installation & Setup

1. **Clone the repository**

   ```bash
   git clone https://github.com/theeyad/reactDashboard.git
   cd reactDashboard
   ```

2. **Install dependencies**

   ```bash
   npm install
   ```

3. **Launch the development server**

   ```bash
   npm run dev
   ```

   Open `http://localhost:5173` in your browser to view the application.

---

## Available Scripts

In the project directory, you can run:

| Command           | Description                                                                       |
| :---------------- | :-------------------------------------------------------------------------------- |
| `npm run dev`     | Starts the Vite development server with Hot Module Replacement (HMR).             |
| `npm run build`   | Runs TypeScript compilation (`tsc -b`) and builds the production bundle via Vite. |
| `npm run preview` | Serves the production build locally for verification.                             |
| `npm run lint`    | Runs `Oxlint` for fast static code analysis.                                      |
