<script setup>
import { ref } from 'vue'

// 可控的 marginTop 变量，单位为 px
const videoMarginTop = ref(-20)
const firstVideoMarginTop = ref(0)

// 视频数据
const videos = [
  { img: '/XYZCYlinder-projectpage/video/x_120.png', video: '/XYZCYlinder-projectpage/video/output_120_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_150.png', video: '/XYZCYlinder-projectpage/video/output_150_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_190.png', video: '/XYZCYlinder-projectpage/video/output_190_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_280.png', video: '/XYZCYlinder-projectpage/video/output_280_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_330.png', video: '/XYZCYlinder-projectpage/video/output_330_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_360.png', video: '/XYZCYlinder-projectpage/video/output_360_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video/x_10.png', video: '/XYZCYlinder-projectpage/video/output_10_scene.mp4' },
  // { img: '/video/input_7.png', video: '/video/output_7_scene.mp4' },
  // { img: '/video/input_8.png', video: '/video/output_8_scene.mp4' },
  // { img: '/video/input_9.png', video: '/video/output_9_scene.mp4' },
  // { img: '/video/input_10.png', video: '/video/output_10_scene.mp4' },
  // { img: '/video/input_11.png', video: '/video/output_11_scene.mp4' },
  // { img: '/video/input_12.png', video: '/video/output_12_scene.mp4' },
  // { img: '/video/input_13.png', video: '/video/output_13_scene.mp4' },
  // { img: '/video/input_14.png', video: '/video/output_14_scene.mp4' },
  // { img: '/video/input_15.png', video: '/video/output_15_scene.mp4' },
  // { img: '/video/input_16.png', video: '/video/output_16_scene.mp4' },
  // { img: '/video/input_17.png', video: '/video/output_17_scene.mp4' },
  // { img: '/video/input_18.png', video: '/video/output_18_scene.mp4' },
  // { img: '/video/input_19.png', video: '/video/output_19_scene.mp4' },
]
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">More Visualizations</h1>
    </el-row>

    <!-- 每个网站的视频的iframe可能不一致，最好在这里手动调整 -->
    <el-row justify="center">
      <el-col
        :xs="16"
        :sm="16"
        :md="16"
        :lg="16"
        :xl="16"
        style="padding: 0; margin-bottom: 0;"
      >
        <!-- local -->
        <template v-for="(item, idx) in videos" :key="idx">
          <el-container
            class="video-container"
            :style="{
              margin: (idx === 0 ? firstVideoMarginTop + 'px' : videoMarginTop + 'px') + ' 0px 0px 0px'
            }"
          >
            <img :src="item.img" class="hover-image" style="width: 20%; object-fit: contain; margin-right: 2%;">
            <video muted preload playsinline autoplay class="hover-video" style="width: 78%;" loop>
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