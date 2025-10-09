<script setup>
import { ref } from 'vue'

// 可控的 marginTop 变量，单位为 px
const videoMarginTop = ref(30)
const firstVideoMarginTop = ref(0)

// 视频数据
const videos = [
  { video: '/video/output_223_72_scene.mp4' },
  { video: '/video/output_223_8_scene.mp4' },
]
</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Explainer Video</h1>
    </el-row>

    <!-- 每个网站的视频的iframe可能不一致，最好在这里手动调整 -->
    <el-row justify="center">
      <el-col
        :xs="24"
        :sm="21"
        :md="26"
        :lg="28"
        :xl="22"
        style="padding: 0; margin-bottom: 0;"
      >
        <!-- local -->
        <template v-for="(item, idx) in videos" :key="idx">
          <el-container
            class="video-container"
            :style="{
              height: '48%',
              margin: (idx === 0 ? firstVideoMarginTop + 'px' : videoMarginTop + 'px') + ' 0px 0px 0px'
            }"
          >
            <div style="text-align: center; margin-right:10px; margin-bottom:2%; font-weight: bold; width: 15%; font-size: clamp(6px, 1vw, 14px); display: flex; flex-direction: column; justify-content: space-between; height: 100%;">
              <div>ControlNet Condition</div>
              <div>Generated 2D Scene</div>
              <div>GT 2D Scene</div>
              <div>3D Scene (from Generated)</div>
              <div>Depth (from Generated)</div>
              <div>3D Scene (from GT)</div>
              <div>Depth (from GT)</div>
            </div>
            <video controls muted preload playsinline autoplay style="width: 80%; height: 100%;" loop>
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

.video-container{
  /* 默认的margin-top可以被覆盖 */
  margin: 0px 0px 0px 0px;
}

iframe, video {
  aspect-ratio: 3.6 / 2.4;
  width: 100%;
}
</style>