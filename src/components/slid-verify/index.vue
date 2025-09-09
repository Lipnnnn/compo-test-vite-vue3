<template>
  <div class="slide-verify">
    <!-- 滑动盒子 -->
    <div class="slide-box">拖动滑块验证</div>
    <!-- 背景条 -->
    <div class="slide-progress" :style="{ width: progressWidth + 'px' }">
      <div class="slide-progress-text">{{ progressText }}</div>
    </div>
    <!-- 滑块 -->
    <div class="slide-block" ref="slideBlock" @mousedown="startDrag" @mousemove="onDrag" @mouseup="endDrag" @mouseleave="endDrag"> > </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
const progressWidth = ref(0)
const isDragging = ref(false)
const slideBlock = ref(null)
const startX = ref(null)
const isPass = ref(false)
const progressText = ref('拖动滑块验证')

function startDrag(event) {
  isDragging.value = true;
  startX.value = event.clientX;
}

function onDrag(event) {
  if (isDragging.value) {
    event.preventDefault();
    // 鼠标移动的位置
    const moveX = event.clientX - startX.value;
    progressWidth.value = moveX;
    if (moveX < 0) {
      return;
    }
    if (moveX >= 264) {
      isPass.value = true;
      // progressText.value = '验证通过';
      return;
    }
    slideBlock.value.style.left = `${moveX}px`;
  }
}

function endDrag() {
  isDragging.value = false;
  if (isPass.value) {
    progressText.value = '验证通过';
  } else {
    progressWidth.value = 0;
    slideBlock.value.style.left = '1px';
  }
}
</script>

<style scoped>
.slide-verify {
  width: 300px;
  height: 40px;
  background-color: #f5f5f5;
  position: relative;
  padding: 1px;
  box-sizing: border-box;
  user-select: none;

  .slide-box {
    width: 100%;
    height: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .slide-progress {
    background-color: #58BE6A;
    height: 38px;
    display: flex;
    align-items: center;
    justify-content: center;
    position: absolute;
    top: 1px;
    left: 1px;
    overflow: hidden;

    .slide-progress-text {
      color: #fff;
      position: absolute;
      left: 100px;
      white-space: nowrap;
    }
  }

  .slide-block {
    position: absolute;
    top: 1px;
    left: 1px;
    width: 38px;
    height: 38px;
    background-color: #fff;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
  }
}
</style>