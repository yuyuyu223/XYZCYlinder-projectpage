<script setup>
import { ref, onMounted, nextTick } from 'vue';

// 视频切换按钮文本
const videoButtonTexts = [
  "Cyberpunk",
  "Autumn", 
  "Sakura",
  "Desert",
  "Snow"
];

// 实际视频文件路径
const videoPaths = [
  '/XYZCYlinder-projectpage/video/cyber.mp4',
  '/XYZCYlinder-projectpage/video/autumn.mp4',
  '/XYZCYlinder-projectpage/video/sakura.mp4',
  '/XYZCYlinder-projectpage/video/desert.mp4',
  '/XYZCYlinder-projectpage/video/snow.mp4',
];

let selectedVideoPath = ref("");
let indexSelected = ref(0);
let isLoading = ref(false);
let isTransitioning = ref(false);
let videoKey = ref(0);
let videoOpacity = ref(1);

const handleChange = async (value) => {
  if (value === indexSelected.value || isTransitioning.value) return;
  
  isTransitioning.value = true;
  
  // 淡出效果
  videoOpacity.value = 0;
  
  // 等待淡出动画完成
  await new Promise(resolve => setTimeout(resolve, 300));
  
  // 切换视频
  indexSelected.value = value;
  selectedVideoPath.value = videoPaths[value];
  videoKey.value += 1;
  
  // 等待DOM更新
  await nextTick();
  
  // 预加载新视频
  const videoElement = document.querySelector('.video-player');
  if (videoElement) {
    // 监听视频加载完成事件
    const onLoaded = () => {
      videoElement.removeEventListener('loadeddata', onLoaded);
      
      // 淡入效果
      setTimeout(() => {
        videoOpacity.value = 1;
        isTransitioning.value = false;
      }, 100);
    };
    
    videoElement.addEventListener('loadeddata', onLoaded);
    videoElement.load();
    
    // 设置超时处理
    setTimeout(() => {
      if (isTransitioning.value) {
        videoElement.removeEventListener('loadeddata', onLoaded);
        videoOpacity.value = 1;
        isTransitioning.value = false;
      }
    }, 5000);
  } else {
    // 降级处理：直接显示
    setTimeout(() => {
      videoOpacity.value = 1;
      isTransitioning.value = false;
    }, 300);
  }
};

onMounted(() => {
  selectedVideoPath.value = videoPaths[0];
});
</script>

<template>
  <div>
    <el-divider />

    <!-- <el-row justify="center">
      <h1 class="section-title">More generalization results</h1>
    </el-row> -->

    
    <el-row justify="center">
      <el-col  :span="20">
        <el-row justify="space-evenly" style="margin-top: 20px;">
          <!-- 视频切换按钮 -->
          <el-col :xs="6" :sm="4" :md="3" :lg="3" :xl="2" v-for="(buttonText, index) in videoButtonTexts" :key="index" style="text-align: center;">
            <el-button 
              class="video-button" 
              @click="handleChange(index)"
              :class="{ 'selected-button': indexSelected === index, 'unselected-button': indexSelected !== index }"
              size="large"
            >
              {{ buttonText }}
            </el-button>
          </el-col>
          
          <!-- 视频播放器 -->
          <el-row justify="center" style="margin-top: 20px;">
            <el-col :span="20">
              <div style="display: flex; align-items: stretch;">
                <div style="text-align: center; margin-right:10px; font-weight: bold; width: 15%; font-size: clamp(6px, 1vw, 14px); display: flex; flex-direction: column; justify-content: space-between;">
                  <div>ControlNet Condition</div>
                  <div>Generated 2D Scene</div>
                  <div>3D Scene (from Generated)</div>
                  <div>Depth (from Generated)</div>
                </div>
                <div class="video-container" style="width: 85%; position: relative;">
                  <div class="loading-overlay" v-show="isTransitioning">
                    <el-icon class="loading-icon"><Loading /></el-icon>
                  </div>
                  <video 
                    :key="videoKey"
                    ref="videoPlayer"
                    class="video-player"
                    controls 
                    muted 
                    preload="metadata" 
                    playsinline 
                    autoplay 
                    style="width: 100%; border-radius: 8px;" 
                    :style="{ opacity: videoOpacity }"
                    loop>
                    <source :src="selectedVideoPath" type="video/mp4">
                    您的浏览器不支持视频播放。
                  </video>
                </div>
              </div>
            </el-col>
          </el-row>
        </el-row>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
.section-title {
    font-size: 2.8rem;
    font-weight: bold;
    margin-bottom: 1.5rem;
    text-align: center;
    background: linear-gradient(45deg, #409EFF, #67C23A);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  .section-description {
    text-align: center;
    font-size: 1.1rem;
    margin-bottom: 2rem;
    color: #606266;
  }

.video-button {
  cursor: pointer;
  border-radius: 8px;
  transition: all 0.3s ease;
  width: 100%;
  margin: 6px 0;
  font-weight: bold;
  font-size: 16px;
  padding: 14px 20px;
  min-height: 55px;
  text-shadow: 0 1px 2px rgba(0,0,0,0.1);
}

.video-button:hover {
  transform: scale(1.03);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.selected-button {
  transition: 0.5s ease;
  box-shadow: 0px 0px 12px 0px #409eff;
  border: 3px solid #409eff;
  background-color: #409eff;
  color: #ffffff;
  font-weight: bold;
  text-shadow: 0 1px 2px rgba(0,0,0,0.3);
}

.unselected-button {
  transition: 0.5s ease;
  opacity: 0.8;
  background-color: #f5f7fa;
  color: #303133;
  border: 2px solid #dcdfe6;
  font-weight: bold;
}



.video-player {
  background-color: #000;
  transition: opacity 0.3s ease;
}

.video-container {
  position: relative;
}

.loading-overlay {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
}

.loading-icon {
  font-size: 48px;
  color: #409eff;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
</style>