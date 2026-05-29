<template>
  <div class="slider-back">
    <!-- 进度条：跟随滑块移动 -->
    <div
        class="slider-progress"
        :style="{ width: `${45+sliderPosition}px` }"
    ></div>

    <!-- 可拖拽的滑块按钮 -->
    <div
        class="slider-btn"
        :class="{'is-dragging':isDragging}"
        ref="sliderBtnRef"
        @mousedown="onTouchStart"

        :style="{ transform: `translateX(${sliderPosition}px)` }"
    >
      <el-icon :size="10" color="#fff"><DArrowRight /></el-icon>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// 核心尺寸配置 (需与CSS中的宽高保持一致)
const sliderWidth = 60      // 滑块宽度
const containerWidth = 250   // 背景轨道总宽度

// 响应式状态
const sliderPosition = ref(0)  // 移动距离
const sliderBtnRef = ref(null)

let isDragging = false
let startX = 0 // 起始坐标

// 获取当前鼠标或触摸的 X 坐标
const getClientX = (e) => e.type.includes('touch') ? e.clientX : e.clientX

// 按下/触摸开始
const onTouchStart = (e) => {
  console.log('按下',e)
  startX = getClientX(e)
  isDragging = true
  window.addEventListener('mousemove', onTouchMove)
  window.addEventListener('mouseup', onTouchEnd)
}

// 移动过程
const onTouchMove = (e) => {
  if (!isDragging) return
  console.log('move',e)
  const moveX = getClientX(e) - startX
  console.log('移动',moveX)
  let newPosition = sliderPosition.value + moveX
  // 边界限制：不能小于0，不能超出轨道最大范围
  newPosition = Math.max(0, Math.min(newPosition, containerWidth - sliderWidth))
  sliderPosition.value = newPosition
  startX = getClientX(e) // 更新起始点以实现平滑跟手
}

// 释放/触摸结束
const onTouchEnd = () => {
  isDragging = false
  const maxPosition = containerWidth - sliderWidth
  if (sliderPosition.value >= maxPosition) {
    // 滑到最右侧，触发解锁成功
    alert('解锁成功！')
    resetSlider()
  } else {
    // 未滑到底，自动回弹重置
    resetSlider()
  }
}

// 重置滑块到初始位置
const resetSlider = () => {
  sliderPosition.value = 0
}
</script>

<style scoped>
.slider-back {
  position: relative; /* 关键：作为子元素绝对定位的参考 */
  width: 250px;
  height: 40px;
  background: #afb2b3;
  border-radius: 25px;
  overflow: hidden;   /* 隐藏超出的进度条 */
  user-select: none;  /* 防止拖动时选中页面文字 */
}

/* 新增的进度条样式 */
.slider-progress {
  position: absolute;
  top: 0; left: 0;
  height: 100%;
  background: #8bc34a; /* 你可以换成你想要的进度条颜色 */
  border-radius: 25px;
  transition: width 0.1s ease;
  z-index: 1;
}

.slider-btn {
  position: absolute;
  top: 0; left: 0;
  height: 40px;
  width: 60px;
  background: #3e96ec;
  border-radius: 25px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: transform 0.1s ease;
  z-index: 2; /* 确保滑块在进度条之上 */
}

.slider-btn .is-dragging {
  transition: none !important;
}
</style>