<script setup>
import VuePdfEmbed from 'vue-pdf-embed';
import { ref } from 'vue'

// 可控的 marginTop 变量，单位为 px
const videoMarginTop = ref(-20)
const firstVideoMarginTop = ref(0)
const pdfSource = '/video2/waymo_zero_shot.pdf';
// 视频数据
const videos = [
  { img: '/XYZCYlinder-projectpage/video5/x_10_371_new.png', video: '/XYZCYlinder-projectpage/video5/output_10_371_scene_new.mp4' },
    { img: '/XYZCYlinder-projectpage/video5/x_1_241_new.png', video: '/XYZCYlinder-projectpage/video5/output_1_241_scene_new.mp4' },
      { img: '/XYZCYlinder-projectpage/video5/x_0_2_new.png', video: '/XYZCYlinder-projectpage/video5/output_0_2_scene_new.mp4' },
        { img: '/XYZCYlinder-projectpage/video5/x_7_0.png', video: '/XYZCYlinder-projectpage/video5/output_7_0_scene.mp4' },
          { img: '/XYZCYlinder-projectpage/video5/x_14_130_new.png', video: '/XYZCYlinder-projectpage/video5/output_14_130_scene_new.mp4' },
          { img: '/XYZCYlinder-projectpage/video5/x_10_212_new.png', video: '/XYZCYlinder-projectpage/video5/output_10_212_scene_new.mp4' },
]
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Zero Shot on Argoverse</h1>
    </el-row>

    <el-row justify="center">
    <el-col :span="18">
        <p>
          <!-- Argoverse数据集具有7个相机，捕获了360度环视场景，且相机本身参数与nuscenes并不一致，我们为该数据集提供了一套特有的UCP构建参数，以兼容我们的模型。 -->
           For the Argoverse dataset, which features a 7-camera configuration, we also developed a customized UCP parameter set to address the inconsistencies in its camera parameters relative to nuScenes, thereby achieving model compatibility. We assess the zero-shot generalization of our model by deploying the nuScenes-trained model directly on the Argoverse dataset.
        </p>
      </el-col>
    </el-row>
    

    <!-- 每个网站的视频的iframe可能不一致，最好在这里手动调整 -->
    <el-row justify="center">
      <el-col
        :span="18"
        style="padding: 0; margin-bottom: 0;"
      >
        <!-- local -->
        <template v-for="(item, idx) in videos" :key="idx">
          <el-container
            class="video-container"
            :style="{
              height: '18%',
              margin: (idx === 0 ? firstVideoMarginTop + 'px' : videoMarginTop + 'px') + ' 0px 0px 0px'
            }"
          >
            <img :src="item.img" class="hover-image" style="width: 15%; object-fit: contain; margin-right: 2%;">
            <video muted preload playsinline autoplay class="hover-video" style="width: 85%; height: 100%;" loop>
              <source :src="item.video" type="video/mp4">
            </video>
          </el-container>
        </template>
        
        <!-- bilibili -->
        <!-- <el-container class="video-container">
          <iframe src="//www.bilibili.com/blackboard/html5mobileplayer.html?bvid=BV1zw68YsEP9" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
        </el-container> -->

        <!-- youtube -->
        <!-- <el-container class="video-container">
          <iframe src="https://www.youtube.com/embed/wjZofJX0v4M?si=BFvRyc3n3fFV_f1G" frameborder="0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
        </el-container> -->
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

.video-container{
  /* 默认的margin-top可以被覆盖 */
  margin: 0px 0px 0px 0px;
}

iframe, video {
  aspect-ratio: 16.8 / 2.4;
  width: 100%;
}

.hover-image {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  position: relative;
  z-index: 1;
}

.hover-image:hover {
  transform: scale(3);
  z-index: 10;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}

.hover-video {
  transition: transform 0.3s ease, box-shadow 0.3s ease;
  cursor: pointer;
  position: relative;
  z-index: 1;
}

.hover-video:hover {
  transform: scale(1.5);
  z-index: 10;
  box-shadow: 0 8px 16px rgba(0, 0, 0, 0.3);
}
</style>