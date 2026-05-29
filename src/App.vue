<script setup lang="ts">
import { ref } from 'vue';

import Iphone from './components/iphone.vue';

const defaultPreviewUrl = import.meta.env.VITE_UNIAPP_PREVIEW_URL ?? '';
const previewUrl = ref(defaultPreviewUrl);

</script>

<template>
  <main class="page-shell">
<!--    <Huadong/>-->
<!--    <section class="hero">-->
<!--      <p class="eyebrow">uniapp preview host</p>-->
<!--      <h1>把 uniapp 的 H5 预览放进手机模型里</h1>-->
<!--      <p class="lede">-->
<!--        先把你的 uniapp 项目运行成 H5 地址，然后把这个地址填到下面。这个手机壳组件会用 iframe 把页面嵌进去。-->
<!--        如果你的页面被浏览器拦截，通常是 uniapp 的站点设置了 X-Frame-Options 或 CSP。-->
<!--      </p>-->
<!--    </section>-->

<!--    <section class="controls">-->
<!--      <label class="field">-->
<!--        <span>uniapp H5 预览地址</span>-->
<!--        <input-->
<!--          v-model="previewUrl"-->
<!--          type="url"-->
<!--          placeholder="例如：http://localhost:8080"-->
<!--          autocomplete="off"-->
<!--        />-->
<!--      </label>-->
<!--      <p class="hint">-->
<!--        也可以把地址写进环境变量 `VITE_UNIAPP_PREVIEW_URL`，这样每次启动都会自动加载。-->
<!--      </p>-->
<!--    </section>-->

    <section class="preview-stage">
      <Iphone
        class="device"
        :width="433"
        :height="882"
        mode="auto"
        :preview-url="'http://localhost:4001' || undefined"
      />
    </section>
  </main>
</template>


<style>
/* 全局样式 - 不使用 scoped 确保全局生效 */
html, body {
  margin: 0;
  padding: 0;
  overflow: hidden !important;
  height: 100%;
  width: 100%;
  position: fixed;
}
</style>
<style scoped>



.page-shell {
  height: 100vh;
  overflow-y: hidden;
  padding: clamp(20px, 2vw, 56px);
  display: grid;
  gap: 24px;
  align-items: start;
  justify-items: center;
  background:
    radial-gradient(circle at top left, rgba(59, 130, 246, 0.3), transparent 30%),
    radial-gradient(circle at bottom right, rgba(14, 165, 233, 0.18), transparent 28%),
    linear-gradient(180deg, #07111f 0%, #0f172a 55%, #111827 100%);
  color: #f8fafc;
}

.hero {
  width: min(760px, 100%);
  text-align: center;
}

.eyebrow {
  margin: 0 0 12px;
  font-size: 0.78rem;
  letter-spacing: 0.28em;
  text-transform: uppercase;
  color: rgba(148, 163, 184, 0.92);
}

h1 {
  margin: 0;
  font-size: clamp(2.4rem, 6vw, 5rem);
  line-height: 0.95;
  letter-spacing: -0.06em;
}

.lede {
  width: min(66ch, 100%);
  margin: 18px auto 0;
  font-size: 1rem;
  line-height: 1.75;
  color: rgba(226, 232, 240, 0.82);
}

.controls {
  width: min(760px, 100%);
  padding: 18px;
  border-radius: 24px;
  background: rgba(15, 23, 42, 0.5);
  border: 1px solid rgba(148, 163, 184, 0.18);
  box-shadow: 0 24px 60px rgba(15, 23, 42, 0.18);
}

.field {
  display: grid;
  gap: 10px;
}

.field span {
  font-size: 0.92rem;
  font-weight: 700;
  color: rgba(226, 232, 240, 0.86);
}

.field input {
  width: 100%;
  border: 1px solid rgba(148, 163, 184, 0.2);
  border-radius: 18px;
  padding: 14px 16px;
  background: rgba(255, 255, 255, 0.08);
  color: #f8fafc;
  font: inherit;
  outline: none;
}

.field input::placeholder {
  color: rgba(226, 232, 240, 0.48);
}

.field input:focus {
  border-color: rgba(96, 165, 250, 0.7);
  box-shadow: 0 0 0 4px rgba(59, 130, 246, 0.18);
}

.hint {
  margin: 10px 0 0;
  font-size: 0.92rem;
  line-height: 1.65;
  color: rgba(226, 232, 240, 0.72);
}

.preview-stage {
  width: 100%;
  display: flex;
  justify-content: center;
}

.device {
  width: min(100vw, 453px);
  height: auto;
  transform: scale(0.8);
  transform-origin: top center;
  filter: drop-shadow(0 36px 70px rgba(15, 23, 42, 0.42));
}
</style>
