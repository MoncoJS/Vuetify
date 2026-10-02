# Project Setup Guide (Vue 3 + Vuetify + Tailwind CSS v4)

คู่มือการติดตั้งและตั้งค่าโปรเจกต์ Vue 3 ที่ใช้งานร่วมกับ **Vuetify 3** และ **Tailwind CSS v4**

---

## 📌 สารบัญ

1. [Step 1: ติดตั้งโปรเจกต์ Vue 3 (Vite + TypeScript)](#step-1-ติดตั้งโปรเจกต์-vue-3)
2. [Step 2: ติดตั้งและตั้งค่า Vuetify 3](#step-2-ติดตั้งและตั้งค่า-vuetify-3)
3. [Step 3: ติดตั้งและตั้งค่า Tailwind CSS v4](#step-3-ติดตั้งและตั้งค่า-tailwind-css-v4)
4. [Step 4: แนวทางการใช้งาน Tailwind ร่วมกับ Vuetify (แก้ปัญหาสไตล์ชนกัน)](#step-4-แนวทางการใช้งาน-tailwind-ร่วมกับ-vuetify)
5. [คำสั่งที่ใช้ในการรันโปรเจกต์](#คำสั่งที่ใช้ในการรันโปรเจกต์)

---

## Step 1: ติดตั้งโปรเจกต์ Vue 3

สร้างโปรเจกต์ด้วยคำสั่ง:

```bash
npm create vue@latest
```

ตั้งค่าตัวเลือกดังนี้:

```text
✔ Project name: … shop
✔ Add TypeScript? … Yes
✔ Add JSX Support? … No
✔ Add Vue Router for Single Page Application development? … Yes
✔ Add Pinia for state management? … Yes
✔ Add Vitest for Unit Testing? … No
✔ Add an End-to-End Testing Solution? … No
✔ Add ESLint for code quality? … Yes
✔ Add Prettier for code formatting? … Yes
✔ Add Oxlint for faster linting? … Yes
```

เข้าไปยังโฟลเดอร์โปรเจกต์และติดตั้ง Dependencies:

```bash
cd shop
npm install
```

---

## Step 2: ติดตั้งและตั้งค่า Vuetify 3

### 1. ติดตั้ง Packages

```bash
npm install vuetify @mdi/font
```

### 2. สร้างไฟล์ Plugin สำหรับ Vuetify

สร้างไฟล์ `src/plugin/vuetify.ts`:

```ts
// src/plugin/vuetify.ts
import 'vuetify/styles'
import '@mdi/font/css/materialdesignicons.css'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'

const vuetify = createVuetify({
  components,
  directives,
  theme: {
    defaultTheme: 'light',
  },
})

export default vuetify
```

### 3. ลงทะเบียน Vuetify ใน `src/main.ts`

```ts
// src/main.ts
import './assets/main.css'

import { createApp } from 'vue'
import { createPinia } from 'pinia'

import App from './App.vue'
import router from './router'
import vuetify from '@/plugin/vuetify'

const app = createApp(App)

app.use(createPinia())
app.use(router)
app.use(vuetify)

app.mount('#app')
```

---

## Step 3: ติดตั้งและตั้งค่า Tailwind CSS v4

### 1. ติดตั้ง Tailwind CSS และ Vite Plugin

```bash
npm install tailwindcss @tailwindcss/vite
```

### 2. ตั้งค่า `vite.config.ts`

เพิ่มปลั๊กอิน `@tailwindcss/vite` ในคอนฟิก:

```ts
// vite.config.ts
import { fileURLToPath, URL } from 'node:url'
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import vueDevTools from 'vite-plugin-vue-devtools'
import tailwindcss from '@tailwindcss/vite'

// https://vite.dev/config/
export default defineConfig({
  plugins: [vue(), vueDevTools(), tailwindcss()],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url)),
    },
  },
})
```

### 3. Import Tailwind ใน `src/assets/main.css`

```css
/* src/assets/main.css */
@import 'tailwindcss';
```

---

## Step 4: แนวทางการใช้งาน Tailwind ร่วมกับ Vuetify

เนื่องจาก Vuetify Component มี CSS เฉพาะตัว (เช่น `.v-btn`) ซึ่งอาจมีความสำคัญ (Specificity) สูงกว่า Tailwind Utility ทั่วไป คุณสามารถเลือกปรับแต่งได้ตามกรณีการใช้งาน:

### 1. การเปลี่ยนสี Vuetify Component

- **ใช้ `color` prop ของ Vuetify (แนะนำ):**

  ```vue
  <v-btn color="primary">ปุ่มหลัก</v-btn>
  <v-btn color="amber-darken-2">ปุ่มสีส้ม</v-btn>
  <v-btn color="#d97706">ปุ่มรหัสสี Hex</v-btn>
  ```

- **ใช้ Tailwind Important (`!`) เมื่อต้องการบังคับด้วย Utility Class:**
  ```vue
  <v-btn class="!bg-amber-600 !text-white">ปุ่มใช้ Tailwind</v-btn>
  ```

### 2. การจัด Layout และ Spacing

สามารถใช้ Tailwind คุมโครงสร้างและระยะห่างได้ตามปกติ:

```vue
<template>
  <main class="min-h-screen bg-slate-100 p-8 flex flex-col items-center justify-center gap-4">
    <h1 class="text-2xl font-bold text-slate-800">หน้าหลัก</h1>
    <v-btn color="primary" elevation="2">Vuetify Button</v-btn>
  </main>
</template>
```

---

## คำสั่งที่ใช้ในการรันโปรเจกต์

| คำสั่ง            | คำอธิบาย                                                |
| :---------------- | :------------------------------------------------------ |
| `npm run dev`     | รันโปรเจกต์ในโหมด Development (`http://localhost:5173`) |
| `npm run build`   | ทำการ Type-check และ Build สำหรับ Production            |
| `npm run preview` | พรีวิวไฟล์ Build ก่อน Deploy                            |
| `npm run lint`    | ตรวจสอบและแก้ไข Code Style ด้วย ESLint / Oxlint         |
| `npm run format`  | จัดรูปแบบโค้ดด้วย Prettier                              |
