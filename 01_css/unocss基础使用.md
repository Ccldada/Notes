# 下载安装

```typescript
# 安装unocss和三个预设，第一个是工具类预设，第二个是属性化模式支持，第三个是icon支持
npm i -D unocss @unocss/preset-uno @unocss/preset-attributify @unocss/preset-icons
# or
yarn add -D unocss @unocss/preset-uno @unocss/preset-attributify @unocss/preset-icons
# or
pnpm i -D unocss @unocss/preset-uno @unocss/preset-attributify @unocss/preset-icons

```
# 配置

* 修改vite.config.js文件：
```typescript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
// 引入Unocss
import Unocss from 'unocss/vite';
import { presetUno, presetAttributify, presetIcons } from 'unocss'
// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    Unocss({ // 使用Unocss
      presets: [
        presetUno(),
        presetAttributify(),
        presetIcons()],
    })
  ]
})
```
或者
```typescript
import { fileURLToPath, URL } from "node:url";

import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import UnoCSS from "unocss/vite";
import { presetUno, presetAttributify, presetIcons } from "unocss";
import { myPreset } from "./my-preset";

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    vue(),
    UnoCSS({
      presets: [presetUno(), presetAttributify(), presetIcons(), myPreset],
    }),
  ],
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
});

```
* 自定义预设 
```typescript
// my-preset.ts
export const myPreset= {
  name: 'my-preset',
  rules: [
    [/^m-(\d+)$/, ([_, num]) => ({ margin: `${num}px` })],
    [/^p-(\d+)$/, ([_, num]) => ({ padding: `${num}px` })],
    ["red", { color: "red" }], // 这里可以自定义
  ],
  variants: [
    /* ... */
  ],
  shortcuts: [
    /* ... */
  ]
  // ...
}
```
* 预设uno.config.js
```typescript
// uno.config.ts
import { defineConfig } from 'unocss'
export default defineConfig({
  rules: [['yyyy', { color: 'white' }]],
})
```
* 修改main.js文件
```typescript
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'
import 'uno.css' // focus code

createApp(App).mount('#app')

```
* 在app.vue中编写代码
```typescript
<script setup>
</script>

<template>
  <button class="bg-blue w-80 h-10 ml-10 m-1">
    button
  </button>
</template>

<style scoped>
</style>

```
>config.js里的 rules优先级 大于 js预设 里的
# Uno preset

>该预设继承自 @unocss/preset-wind 和 @unocss/preset-mini
```typescript
pnpm add -D @unocss/preset-uno

import { presetUno } from 'unocss'
```
## 用法

该预设尝试提供流行的功能优先框架（包括 Tailwind CSS、Windi CSS、Bootstrap、Tachyons 等）的共同超集。

```typescript
.ma4 {
  margin: 1rem;
}
.ml-3 {
  margin-left: 0.75rem;
}
.ms-2 {
  margin-inline-start: 0.5rem;
}
.mt-10px {
  margin-top: 10px;
}
```
# 属性化

```typescript
pnpm add -D @unocss/preset-attributify

import { presetAttributify } from 'unocss'
```
假设你有一个使用 Tailwind 工具类的按钮。当列表变得越来越长时，它变得很难阅读和维护。使用属性化模式，你可以将工具类分成属性：
```typescript
<button
  bg="blue-400 hover:blue-500 dark:blue-500 dark:hover:blue-600"
  text="sm white"
  font="mono light"
  p="y-2 x-4"
  border="2 rounded blue-200"
>
  Button
</button>
```
## 前缀自引用

对于具有与前缀相同的工具类（如 `flex`、`grid`、`border`）的工具类，提供了一个特殊的 ~ 值。

例如:

```plain
<button class="border border-red">Button</button>
```
可以写为:
```plain
<button border="~ red">Button</button>
```
## 无值属性

除了 Windi CSS 的属性化模式外，这个预设还支持无值属性。

例如：

```plain
<div class="m-2 rounded text-teal-400" />
```
可以写为：
```plain
<div m-2 rounded text-teal-400 />
```
# 文本排版

```typescript
pnpm add -D @unocss/preset-typography

import { presetTypography } from 'unocss'
```
## rem转换px

```typescript
# pnpm i @unocss/preset-rem-to-px -D
```







.

