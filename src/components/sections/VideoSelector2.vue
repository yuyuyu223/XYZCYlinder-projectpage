<script setup>
import { ref, onMounted, nextTick } from 'vue';
import { ElIcon } from 'element-plus';
import { Loading } from '@element-plus/icons-vue';

// 视频切换按钮文本
const videoButtonTexts = [
  "Scene 1",
  "Scene 2", 
  "Scene 3",
  "Scene 4",
  "Scene 5"
];

const videoPrompts = [
  "A driving scene image at boston-seaport. Rain, industrial, turn right, turn right, parked cars.",
  "A driving scene image at singapore-onenorth. Busy street, parked cars, parking lot, ped sitting at bus stop, motorcycle driving.",
  "A driving scene image at singapore-onenorth. Overtake parked car, parked bicycle.",
  "A driving scene image at singapore-hollandvillage. Night, congestion. difficult lighting. very dark.",
  "Daytime. Sunny.a driving scene image at singapore-onenorth. cross intersection, scooter, nature, peds crossing."
]

// 实际视频文件路径
const videoPaths = [
  '/XYZCYlinder-projectpage/video/output_223_6_scene_magicdrivedit.mp4',
  '/XYZCYlinder-projectpage/video/output_223_28_scene_magicdrivedit.mp4',
  '/XYZCYlinder-projectpage/video/output_223_15_scene_magicdrivedit.mp4',
  '/XYZCYlinder-projectpage/video/output_223_2_scene_magicdrivedit.mp4',
  '/XYZCYlinder-projectpage/video/output_223_17_scene_magicdrivedit.mp4'  
];

let selectedVideoPath = ref("");
let prompt = ref("a car driving on the road");
let indexSelected = ref(0);
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
  prompt.value = videoPrompts[value];
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
  prompt.value = videoPrompts[0];
});
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
    <el-col  :span="18">
        <p>
            We begin by using the <span style="background: linear-gradient(45deg, #8B4513, #DAA520); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-weight: bold;">MagicDriveDiT</span> model to generate a set of six-view images conditioned on a <span style="background: linear-gradient(45deg, #8B4513, #DAA520); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-weight: bold;">text prompt</span>. These generated images are then fed as input into our main model. The following video demonstrates our model's ability to generalize to these externally generated inputs.
        </p>
      </el-col>
    </el-row>
    
    <el-row justify="center">
      <el-col  :span="20">
        <el-row justify="space-evenly" style="margin-top: 20px;">
          <!-- 视频切换按钮 -->
          <el-col :xs="6" :sm="4" :md="3" :lg="3" :xl="2" v-for="(buttonText, index) in videoButtonTexts" :key="index" style="text-align: center;">
            <el-button 
              class="video-button" 
              size="large"
              @click="handleChange(index)"
              :class="{ 'selected-button': indexSelected === index, 'unselected-button': indexSelected !== index }"
            >
              {{ buttonText }}
            </el-button>
          </el-col>
          
          <!-- 视频播放器 -->
          <el-row justify="center" style="margin-top: 20px;">
            <el-col :span="20">
              <div style="display: flex; align-items: stretch;">
                <div style="text-align: center; margin-right:10px; font-weight: bold; width: 15%; font-size: clamp(6px, 1vw, 14px); display: flex; flex-direction: column; justify-content: space-between;">
                  <div>Generated 2D Scene</div>
                  <div>3D Scene (from Generated)</div>
                  <div>Depth (from Generated)</div>
                </div>
                <div class="video-container" style="width: 100%; position: relative;">
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
    <el-row justify="center">
      <el-col :span="18">
        <p style="color: gray; font-style: italic;">
            Prompt: {{ prompt }}
        </p>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped>
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
  text-shadow: 0 1px 2px rgba(0,0,0,0.2);
}

.video-button:hover {
  transform: scale(1.03);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}

.selected-button {
  background-color: #409eff;
  color: #ffffff;
  border: 3px solid #2d8cf0;
  box-shadow: 0 4px 16px rgba(64, 158, 255, 0.4);
  text-shadow: 0 1px 2px rgba(0,0,0,0.3);
}

.unselected-button {
  background-color: rgba(245, 247, 250, 0.9);
  color: #606266;
  border: 2px solid #dcdfe6;
  opacity: 0.8;
  font-weight: bold;
}

.section-title {
  font-size: 24px;
  font-weight: bold;
  color: #303133;
  margin-bottom: 20px;
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