<script setup>
import VuePdfEmbed from 'vue-pdf-embed';
import { ref } from 'vue'

// 可控的 marginTop 变量，单位为 px
const videoMarginTop = ref(-10)
const firstVideoMarginTop = ref(0)
const pdfSource = '/video2/waymo_zero_shot.pdf';
// 视频数据
const videos = [
  { img: '/XYZCYlinder-projectpage/video4/x_2_20_new.png', video: '/XYZCYlinder-projectpage/video4/output_2_20_scene_new.mp4' },
    { img: '/XYZCYlinder-projectpage/video4/x_2_151_new.png', video: '/XYZCYlinder-projectpage/video4/output_2_151_scene_new.mp4' },
      { img: '/XYZCYlinder-projectpage/video4/x_2_580_new.png', video: '/XYZCYlinder-projectpage/video4/output_2_580_scene_new.mp4' },
        { img: '/XYZCYlinder-projectpage/video4/x3_180.png', video: '/XYZCYlinder-projectpage/video4/output_3_180_scene.mp4' },
          { img: '/XYZCYlinder-projectpage/video4/x_0_280.png', video: '/XYZCYlinder-projectpage/video4/output_0_280_scene.mp4' },
          // { img: '/video4/x_20_40.png', video: '/video4/output_20_40_scene.mp4' },
]
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Zero Shot on Once</h1>
    </el-row>

    <el-row justify="center">
    <el-col :span="18">
        <p>
          <!-- Pandaset数据集和Nuscenes相同，具有6个相机，捕获了360度环视场景，但是相机本身参数与nuscenes并不一致，我们为该数据集提供了一套特有的UCCM构建参数，以兼容我们的模型。 -->
     While Once shares the 6-camera, 360° surround-view configuration of nuScenes, its intrinsic and extrinsic camera parameters are distinct. Consequently, we tailored a unique set of UCCM parameters for Once to ensure its compatibility with our model. We assess the zero-shot generalization of our model by deploying the nuScenes-trained model directly on the Once dataset.
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
            <img :src="item.img" class="hover-image" style="width: 20%; object-fit: contain; margin-right: 2%;">
            <video muted preload playsinline autoplay class="hover-video" style="width: 78%; height: 100%;" loop>
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
  aspect-ratio: 16 / 2.4;
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