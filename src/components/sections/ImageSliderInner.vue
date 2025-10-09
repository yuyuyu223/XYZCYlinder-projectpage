<script lang="ts" setup>
import { ImgComparisonSlider } from '@img-comparison-slider/vue';
import { ref, computed, onMounted, onUnmounted } from 'vue';

// 定义数据集类型
interface Dataset {
  name: string;
  displayName: string;
  images: Array<{
    before: string;
    after: string;
    name: string;
    before_tag: string;
    after_tag: string;  
  }>;
}

// 定义两个数据集的图片
const datasets: Dataset[] = [
  {
    name: 'real',
    displayName: 'Nuscenes',
    images: [
      { before: '/image_inner/nuscenes_0_1.png', after: '/image_inner/nuscenes_0_0_new.png', name: 'Scene 1', before_tag: 'OmniScene', after_tag: 'Ours' },
      { before: '/image_inner/nuscenes_1_1.png', after: '/image_inner/nuscenes_1_0_new.png', name: 'Scene 2', before_tag: 'OmniScene', after_tag: 'Ours' },
      { before: '/image_inner/nuscenes_2_1.png', after: '/image_inner/nuscenes_2_0_new.png', name: 'Scene 3', before_tag: 'OmniScene', after_tag: 'Ours' },
      { before: '/image_inner/nuscenes_3_1.png', after: '/image_inner/nuscenes_3_0_new.png', name: 'Scene 4', before_tag: 'OmniScene', after_tag: 'Ours' },
      { before: '/image_inner/nuscenes_4_1.png', after: '/image_inner/nuscenes_4_0_new.png', name: 'Scene 5', before_tag: 'DepthSplat', after_tag: 'Ours' },
      { before: '/image_inner/nuscenes_5_1.png', after: '/image_inner/nuscenes_5_0_new.png', name: 'Scene 6', before_tag: 'DepthSplat', after_tag: 'Ours' },
      // { before: '/image_inner/nuscenes_6_1.png', after: '/image_inner/nuscenes_6_0_new.png', name: 'Scene 7', before_tag: 'DepthSplat', after_tag: 'Ours' },
    ]
  },
  {
    name: 'carla',
    displayName: 'Carla Centric',
    images: [
      { before: '/image_inner/carla_0_1.png', after: '/image_inner/carla_0_0.png', name: 'Scene 1', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_1_1.png', after: '/image_inner/carla_1_0.png', name: 'Scene 2', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_2_1.png', after: '/image_inner/carla_2_0.png', name: 'Scene 3', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_3_1.png', after: '/image_inner/carla_3_0.png', name: 'Scene 4', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_4_1.png', after: '/image_inner/carla_4_0.png', name: 'Scene 5', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_5_1.png', after: '/image_inner/carla_5_0.png', name: 'Scene 6', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_6_1.png', after: '/image_inner/carla_6_0.png', name: 'Scene 7', before_tag: '6Img-to-3D', after_tag: 'Ours' },
      { before: '/image_inner/carla_7_1.png', after: '/image_inner/carla_7_0.png', name: 'Scene 8', before_tag: '6Img-to-3D', after_tag: 'Ours' },
    ]
  },
];

// 响应式状态
const currentDatasetIndex = ref(0);
const isAutoPlaying = ref(false);
const autoPlayInterval = ref<number | null>(null);
const autoPlaySpeed = ref(100); // 100毫秒更新一次
const sliderValue = ref(50); // 滑块位置 (0-100)
const sliderDirection = ref(1); // 1为向右，-1为向左

// 计算属性
const currentDataset = computed(() => datasets[currentDatasetIndex.value]);
const currentImages = computed(() => currentDataset.value.images);
const isRealDataset = computed(() => currentDataset.value.name === 'real');

// 方法
const switchDataset = (index: number) => {
  currentDatasetIndex.value = index;
};

// 自动播放相关方法
const startAutoPlay = () => {
  if (autoPlayInterval.value) return;
  
  isAutoPlaying.value = true;
  autoPlayInterval.value = setInterval(() => {
    // 更新滑块位置
    sliderValue.value += sliderDirection.value * 5; // 每次移动5%
    
    // 检查边界并改变方向
    if (sliderValue.value >= 100) {
      sliderValue.value = 100;
      sliderDirection.value = -1;
    } else if (sliderValue.value <= 0) {
      sliderValue.value = 0;
      sliderDirection.value = 1;
    }
  }, autoPlaySpeed.value);
};

const stopAutoPlay = () => {
  if (autoPlayInterval.value) {
    clearInterval(autoPlayInterval.value);
    autoPlayInterval.value = null;
  }
  isAutoPlaying.value = false;
};

const toggleAutoPlay = () => {
  if (isAutoPlaying.value) {
    stopAutoPlay();
  } else {
    startAutoPlay();
  }
};

// 生命周期钩子
onMounted(() => {
  // 启动自动播放
  startAutoPlay();
});

onUnmounted(() => {
  stopAutoPlay();
});
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Qualitative Comparison</h1>
    </el-row>

    <el-row justify="center">
      <el-col :xs="24" :sm="20" :md="16" :lg="18" :xl="16">
        <p>
          The qualitative comparisons on the Carla-Centric and NuScenes datasets highlight the superiority of our model. Our method achieves significantly better visual quality on Carla-Centric, producing finer texture details and more accurate geometry without the holes that plague other methods. On NuScenes, our model further demonstrates its advantages through enhanced geometric completeness, superior texture fidelity with fewer artifacts and sharper text, and higher geometric accuracy, free from ghosting or distortion.
        </p>
      </el-col>
    </el-row>

    <!-- 数据集选择按钮 -->
    <el-row justify="center" style="margin: 30px 0;">
      <el-button-group>
        <el-button
          v-for="(dataset, index) in datasets"
          :key="dataset.name"
          :type="currentDatasetIndex === index ? 'primary' : 'default'"
          @click="switchDataset(index)"
          size="large"
        >
          {{ dataset.displayName }}
        </el-button>
      </el-button-group>
    </el-row>

    <!-- 自动播放控制按钮 -->
    <el-row justify="center" style="margin: 20px 0;">
      <el-button
        @click="toggleAutoPlay"
        :type="isAutoPlaying ? 'danger' : 'success'"
        :icon="isAutoPlaying ? 'VideoPause' : 'VideoPlay'"
        size="default"
      >
        {{ isAutoPlaying ? '停止滑块自动播放' : '开始滑块自动播放' }}
      </el-button>
      <div style="margin-left: 20px; line-height: 32px;">
        滑块位置: {{ sliderValue }}%
      </div>
    </el-row>

    <!-- 数据集标题 -->
    <el-row justify="center" style="margin: 20px 0;">
      <h2 class="dataset-title">{{ currentDataset.displayName }}</h2>
    </el-row>

    <!-- 图片网格展示 -->
    <el-row justify="center">
      <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="16">
        <el-row :gutter="20" justify="start">
          <el-col 
            v-for="(image, index) in currentImages" 
            :key="`${currentDataset.name}-${index}`"
            :xs="24" 
            :sm="12" 
            :md="isRealDataset ? 8 : 8" 
            :lg="isRealDataset ? 12 : 6"
            :xl="isRealDataset ? 8 : 6"
            style="margin-bottom: 30px;"
          >
            <div :class="['image-card', { 'real-image-card': isRealDataset }]">
              <div class="image-title">{{ image.name }}</div>
              <ImgComparisonSlider class="comparison-slider" :value="sliderValue">
                <figure slot="first" class="before">
                  <img
                    slot="first"
                    style="width: 100%; height: auto;"
                    :src="image.before"
                  />
                  <figcaption>{{ image.before_tag }}</figcaption>
                </figure>
                <figure slot="second" class="after">
                  <img
                    slot="second"
                    style="width: 100%; height: auto;"
                    :src="image.after"
                  />
                  <figcaption>{{ image.after_tag }}</figcaption>
                </figure>
              </ImgComparisonSlider>
            </div>
          </el-col>
        </el-row>
      </el-col>
    </el-row>

    <!-- 如果没有图片的提示 -->
    <el-row v-if="currentImages.length === 0" justify="center">
      <el-col>
        <el-empty description="No images available for this dataset" />
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

.dataset-title {
  color: #409eff;
  margin: 0;
  font-size: 24px;
  font-weight: 500;
}

.image-card {
  background: #fff;
  border-radius: 8px;
  padding: 15px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.1);
  transition: transform 0.3s ease;
}

.real-image-card {
  padding: 20px;
  transform: scale(1.05);
}

.image-card:hover {
  transform: translateY(-2px);
}

.real-image-card:hover {
  transform: scale(1.05) translateY(-2px);
}

.image-title {
  text-align: center;
  font-size: 16px;
  font-weight: 500;
  color: #2e3452;
  margin-bottom: 10px;
}

.comparison-slider {
  width: 100%;
}

.before,
.after {
  margin: 0;
}

.before figcaption,
.after figcaption {
  background: #fff;
  border: 1px solid #c0c0c0;
  border-radius: 8px;
  color: #2e3452;
  opacity: 0.8;
  padding: 8px 12px;
  position: absolute;
  top: 10%;
  transform: translateY(-50%);
  line-height: 100%;
  font-size: 12px;
  font-weight: 500;
}

.before figcaption {
  left: 0px;
}

.after figcaption {
  right: 0px;
}

/* 响应式调整 */
@media (max-width: 768px) {
  .image-card {
    padding: 10px;
  }
  
  .real-image-card {
    padding: 12px;
    transform: none;
  }
  
  .image-title {
    font-size: 14px;
  }
  
  .before figcaption,
  .after figcaption {
    font-size: 10px;
    padding: 6px 8px;
  }

  
}
</style>
