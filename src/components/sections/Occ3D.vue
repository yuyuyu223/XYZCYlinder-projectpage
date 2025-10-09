<script setup lang="ts">
import { ref, computed } from 'vue';

// 定义所有模型 - 添加图像路径
const allModels = [
  // 第一组模型
  {
    id: 'model1',
    name: 'Scene 1',
    path: '/occ/output_360_0.html',
    imagePath: '/occ/x_360_0.png',
    group: 'group1'
  },
  {
    id: 'model2',
    name: 'Scene 2',
    path: '/occ/output_140_0.html',
    imagePath: '/occ/x_140_0.png',
    group: 'group1'
  },
  {
    id: 'model3',
    name: 'Scene 3',
    path: '/occ/output_320_0.html',
    imagePath: '/occ/x_320_0.png',
    group: 'group1'
  },
  // 第二组模型
  {
    id: 'model4',
    name: 'Scene 4',
    path: '/occ/output_0_0.html',
    imagePath: '/occ/x_0_0.png',
    group: 'group2'
  },
  {
    id: 'model5',
    name: 'Scene 5',
    path: '/occ/output_0_80.html',
    imagePath: '/occ/x_0_80.png',
    group: 'group2'
  },
  {
    id: 'model6',
    name: 'Scene 6',
    path: '/occ/output_0_40.html',
    imagePath: '/occ/x_0_40.png',
    group: 'group2'
  }
];

// 当前选中的模型组
const currentGroup = ref<'group1' | 'group2'>('group1');

// 计算当前组应该显示的模型
const visibleModels = computed(() => {
  return allModels.filter(model => model.group === currentGroup.value);
});

// 检查模型是否属于当前组
const isModelVisible = (modelId: string) => {
  const model = allModels.find(m => m.id === modelId);
  return model?.group === currentGroup.value;
};

// 切换模型组
const switchModelGroup = (group: 'group1' | 'group2') => {
  currentGroup.value = group;
};

// 布局控制
const viewerHeight = ref('300px');
const imageHeight = ref('200px');
</script>

<template>
  <el-divider />
  
  <el-row justify="center">
    <h1 class="section-title">Predicted Occ Visualization</h1>
  </el-row>

  <el-row justify="center">
    <el-col :xs="24" :sm="22" :md="20" :lg="18" :xl="16">
      <p class="section-description">
        Our model utilizes occupancy prediction to obtain a coarse geometry. This geometry then serves a dual role: it acts as guidance for the initial positions of the voxel Gaussians and as a spatial constraint for their subsequent splitting process.
        <!-- The geometry predicted by our model is considerably smoother and more regular. This is attributed to the model's strong generalization capability, which effectively filters out the significant noise present in the GT labels. This effect is particularly evident in the geometry of "trees". Due to the high uncertainty inherent in the complex structure of dense branches and leaves, the model tends to learn and predict a smooth, "averaged" overall geometry, rather than a fine-grained structure with numerous internal voids. -->
      </p>
      
      <!-- 模型组切换按钮 -->
      <div class="group-switcher">
        <el-row justify="center" class="switcher-row">
          <el-button-group>
            <el-button 
              :type="currentGroup === 'group1' ? 'primary' : 'default'"
              @click="switchModelGroup('group1')"
              size="large"
            >
              Nuscenes
            </el-button>
            <el-button 
              :type="currentGroup === 'group2' ? 'primary' : 'default'"
              @click="switchModelGroup('group2')"
              size="large"
            >
              Carla-Centric
            </el-button>
          </el-button-group>
        </el-row>
      </div>
      
      <!-- 直接显示的卡片布局 -->
      <div class="viewers-container">
        <el-row :gutter="30" justify="center" align="stretch">
          <el-col 
            v-for="model in allModels" 
            :key="model.id"
            :xs="24" 
            :sm="24" 
            :md="12" 
            :lg="8"
            class="viewer-column"
            v-show="isModelVisible(model.id)"
          >
            <div class="viewer-card">
              <!-- 模型标题 -->
              <h3 class="model-title">{{ model.name }}</h3>
              
              <!-- 直接显示的图像预览区域 -->
              <div class="image-preview">
                <img 
                  :src="model.imagePath" 
                  :alt="model.name + ' preview'"
                  class="preview-image"
                />
              </div>
              
              <!-- HTML内容区域 -->
              <div class="html-content">
                <iframe
                  :src="model.path"
                  :style="{ width: '100%', height: viewerHeight, border: 'none' }"
                  frameborder="0"
                  allowfullscreen
                />
              </div>
            </div>
          </el-col>
        </el-row>
      </div>
    </el-col>
  </el-row>
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
  color: var(--dark-text-primary);
}

.group-switcher {
  margin: 2rem 0;
}

.viewers-container {
  width: 100%;
  margin: 0 auto;
}

.viewer-column {
  margin-bottom: 30px;
}

/* 修改卡片样式 - 移除overflow限制 */
.viewer-card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  overflow: visible; /* 改为visible允许内容溢出 */
  height: 100%;
  position: relative; /* 添加相对定位 */
}

.model-title {
  text-align: center;
  font-size: 1.3rem;
  font-weight: 600;
  margin: 0;
  padding: 15px 20px;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  color: #2c3e50;
  border-bottom: 1px solid #e4e7ed;
}

/* 修改图像预览容器样式 */
.image-preview {
  width: 100%;
  height: auto;
  overflow: visible; /* 改为visible允许图像溢出 */
  background: #f8f9fa;
  border-bottom: 1px solid #e9ecef;
  margin: 0;
  padding: 0;
  position: relative;
}

.preview-image {
  width: 100%;
  height: auto;
  max-height: 250px;
  object-fit: contain;
  display: block;
  margin: 0;
  padding: 0;
  transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1), 
              box-shadow 0.4s cubic-bezier(0.4, 0, 0.2, 1);
  cursor: pointer;
  position: relative; /* 添加相对定位 */
  z-index: 10; /* 提高z-index确保在悬停时在最上层 */
}

/* 修改悬停效果 - 更高的放大系数 */
.preview-image:hover {
  transform: scale(1.5); /* 增加到1.5倍 */
  box-shadow: 0 15px 40px rgba(0, 0, 0, 0.3);
  z-index: 100; /* 进一步提高z-index */
}

/* 添加遮罩层效果 */
.image-preview::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0);
  transition: background 0.3s ease;
  z-index: 5;
}

.image-preview:hover::before {
  background: rgba(0, 0, 0, 0.1);
}

/* 修改放大镜指示器 */
.image-preview::after {
  content: '🔍';
  position: absolute;
  top: 10px;
  right: 10px;
  background: rgba(255, 255, 255, 0.95);
  border-radius: 50%;
  width: 35px;
  height: 35px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 16px;
  opacity: 0;
  transition: opacity 0.3s ease, transform 0.3s ease;
  pointer-events: none;
  z-index: 15;
  transform: scale(0.8);
}

.image-preview:hover::after {
  opacity: 1;
  transform: scale(1);
}

/* HTML内容样式 */
.html-content {
  padding: 0;
  background: #fafafa;
  margin: 0;
  position: relative;
  z-index: 1;
}

.html-content iframe {
  min-height: v-bind(viewerHeight);
  background: #f5f5f5;
  width: 100%;
  margin: 0;
  padding: 0;
}

/* 响应式调整 */
@media (max-width: 1200px) {
  .section-title {
    font-size: 2.5rem;
  }
}

@media (max-width: 992px) {
  .section-title {
    font-size: 2.2rem;
  }
}

@media (max-width: 768px) {
  .section-title {
    font-size: 2rem;
  }
  
  .section-description {
    font-size: 1rem;
    padding: 0 15px;
  }
  
  .model-title {
    font-size: 1.2rem;
    padding: 12px 15px;
  }
  
  .preview-image {
    max-height: 200px;
  }
  
  .preview-image:hover {
    transform: scale(1.3); /* 在小屏幕上减小放大比例 */
  }
}

@media (max-width: 576px) {
  .section-title {
    font-size: 1.8rem;
  }
  
  .preview-image {
    max-height: 150px;
  }
  
  .preview-image:hover {
    transform: scale(1.2); /* 在更小屏幕上进一步减小放大比例 */
  }
}
</style>
