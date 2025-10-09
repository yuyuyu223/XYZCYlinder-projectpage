<script>
import { Swiper, SwiperSlide} from 'swiper/vue';
import { Navigation, Pagination, Autoplay} from 'swiper/modules';
import VuePdfEmbed from 'vue-pdf-embed'
import 'swiper/css/bundle';

export default {
  components: {
      Swiper,
      SwiperSlide,
      VuePdfEmbed,
      Navigation,
      Pagination,
      Autoplay,
    },
  data() {
    return {
      modules: [
        Navigation,
        Pagination,
        Autoplay,
      ],
      // PDF源文件路径
      pdfSources: [
        "/method/Xnet.pdf",
        "/method/Ynet.pdf",
        "/method/Znet.pdf",
        // "./carousel/4.pdf",
        // "./carousel/5.pdf",
        // "./carousel/6.pdf",
      ],
    }
  }
}
</script>

<template>
  <el-row justify="center">
    <el-col :span="24">
      <!-- 设置轮播图：循环播放、首张图序号、响应式、导航和分页、自动播放 -->
      <swiper
        :loop="true"
        :slidesPerView="1"
        :breakpoints="{
          600: {
            slidesPerView: 2,
          },
          800: {
            slidesPerView: 3,
          },
        }"
        :modules="modules"
        :navigation="{ 
          hideOnClick:true,
        }"
        :pagination="{ 
          hideOnClick:true,
          clickable:true, 
          type:'bullets' 
        }"
        :autoplay="{ 
          delay:5000,
          disableOnInteraction:false,
          pauseOnMouseEnter:true,
        }"
        >
        <swiper-slide v-for="pdfSource in pdfSources">
          <div class="pdf-container">
            <vue-pdf-embed :source="pdfSource" />
          </div>
        </swiper-slide>
      </swiper>
    </el-col>
  </el-row>
</template>
  
<style>
/* 设置Swiper风格 */
.swiper {
  --swiper-theme-color: white;
}

/* PDF容器 - 统一高度，宽度自适应 */
.pdf-container {
  width: 100%;
  height: 500px; /* 统一目标高度 */
  overflow: hidden;
  position: relative;
  background: #f5f5f5;
  display: flex;
  align-items: center;
  justify-content: center;
}

/* PDF嵌入组件样式 */
.pdf-container .vue-pdf-embed {
  height: 100%;
  width: auto; /* 宽度根据原始比例自适应 */
  max-width: 100%; /* 确保不超出容器宽度 */
}

.pdf-container canvas {
  height: 100% !important;
  width: auto !important; /* 让宽度根据比例自动调整 */
  max-width: 100% !important;
  display: block;
}

/* 响应式缩放 */
@media (max-width: 768px) {
  .pdf-container {
    height: 400px;
  }
}

@media (max-width: 480px) {
  .pdf-container {
    height: 300px;
  }
}

/* 确保Swiper滑块高度一致 */
.swiper-slide {
  height: auto;
}

.swiper-wrapper {
  align-items: stretch;
}
</style>
  