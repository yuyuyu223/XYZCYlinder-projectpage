<script setup>
import VuePdfEmbed from 'vue-pdf-embed';
import { ref } from 'vue'

// 可控的 marginTop 变量，单位为 px
const videoMarginTop = ref(-50)
const firstVideoMarginTop = ref(0)
const pdfSource = '/video2/waymo_zero_shot.pdf';
// 视频数据
const videos = [
  { img: '/XYZCYlinder-projectpage/video2/x_166_new.png', video: '/XYZCYlinder-projectpage/video2/output_166_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video2/x_171_new.png', video: '/XYZCYlinder-projectpage/video2/output_171_scene_new.mp4' },
  { img: '/XYZCYlinder-projectpage/video2/x_148_new.png', video: '/XYZCYlinder-projectpage/video2/output_148_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video2/x_142_new.png', video: '/XYZCYlinder-projectpage/video2/output_142_scene.mp4' },
  { img: '/XYZCYlinder-projectpage/video2/x_163_new.png', video: '/XYZCYlinder-projectpage/video2/output_163_scene_new.mp4' },
  { img: '/XYZCYlinder-projectpage/video2/x_74_new.png', video: '/XYZCYlinder-projectpage/video2/output_74_scene_new.mp4' },
]
</script>

<template>
  <div style="margin-bottom: -160px;">
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Zero Shot on Waymo</h1>
    </el-row>

    <el-row justify="center">
    <el-col :span="18">
        <p>
          <!-- Waymo数据集只具有5个相机，补货了略大于180度的环视场景，为了对齐waymo数据集与我们的UCP的设计，除了更改UCP的参数之外，我们按照下图的方式处理了waymo相机，将处于前方的三个摄像机镜像对称到后方，两侧的相机画面采用中心对称。 我们使用Nuscenes训练出的模型来检验其在Waymo数据集的零样本泛化能力-->
     The Waymo dataset is equipped with only five cameras, capturing a forward-facing surround view that slightly exceeds 180 degrees. This configuration is incompatible with the full panoramic input expected by our UCCM framework. To adapt this dataset for our model, we devised a view augmentation strategy. As illustrated in the figure below, we generate the missing rear views by symmetrically mirroring the three front-facing camera images. Concurrently, we apply a central symmetry transformation to the side-camera views. This process allows us to construct the complete 360-degree surround-view input required by the UCCM model. We assess the zero-shot generalization of our model by deploying the nuScenes-trained model directly on the Waymo dataset.
        </p>
      </el-col>
    </el-row>

    

    <el-row justify="center">
      <el-col :span="18">
          <vue-pdf-embed :source="pdfSource" />
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
            <img :src="item.img" class="hover-image" style="width: 39%; object-fit: contain; margin-right: 2%;">
            <video muted preload playsinline autoplay class="hover-video" style="width: 70%; height: 100%;" loop>
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
  aspect-ratio: 8 / 2.4;
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