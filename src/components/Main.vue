<script>
import Title from './sections/Title.vue'
import Abstract from './sections/Abstract.vue'
import Markdown from './sections/Markdown.vue'
import BibTeX from './sections/BibTeX.vue'
import Carousel from './sections/Carousel.vue'
import Video from './sections/Video.vue'
import ImageSlider from './sections/ImageSlider.vue'
import ImageSliderInner from './sections/ImageSliderInner.vue'
import ImageSelector from './sections/ImageSelector.vue'
import Echart from './sections/Echart.vue'
import Table from './sections/Table.vue'
import Collapse from './sections/Collapse.vue'
import SeletionForComparison from './sections/SeletionForComparison.vue'
import Comment from './sections/Comment.vue'
import Latex from './sections/Latex.vue'
import Model3D from './sections/Model3D.vue'
import GaussianSplats3D from './sections/GaussianSplats3D.vue'
import VideoSelector from './sections/VideoSelector.vue'
import VideoSelector2 from './sections/VideoSelector2.vue'
import VideoSelector3 from './sections/VideoSelector3.vue'
import GenVideo from './sections/GenVideo.vue'
import Occ3D from './sections/Occ3D.vue'
import Video2 from './sections/Video2.vue'
import Video3 from './sections/Video3.vue'
import Video4 from './sections/Video4.vue'
import Video5 from './sections/Video5.vue'
import MoreVideo from './sections/MoreVideo.vue'

export default {
  components: {
    Title,
    Abstract,
    Markdown,
    BibTeX,
    Carousel,
    Video,
    ImageSlider,
    ImageSliderInner,
    ImageSelector,
    Echart,
    Collapse,
    Table,
    SeletionForComparison,
    Comment,
    Latex,
    Model3D,
    GaussianSplats3D,
    VideoSelector,
    VideoSelector2,
    VideoSelector3,
    GenVideo,
    Occ3D,
    Video2,
    Video3,
    Video4,
    Video5,
    MoreVideo
  }
}
</script>

<template>
  <Title/>
  <!-- <Carousel/> -->
  <!-- <Video/> -->
  <Abstract/>
  <!-- <Markdown/> -->
  <Latex/>
  <Table/>
  <!-- <Collapse/> -->
  <!-- <Echart/> -->
  <ImageSliderInner/>
  <!-- <Model3D/> -->
  <Occ3D/>
  <GaussianSplats3D/>
  <MoreVideo/>
  <!-- <SeletionForComparison/> -->
  
  <!-- <ImageSlider/> -->
  <!-- <ImageSelector/> -->
  <!-- <Comment/> -->
  <VideoSelector/>
  <VideoSelector2/>
  <VideoSelector3/>
  <!-- <GenVideo/> -->
  <Video2/>
  <Video3/>
  <Video4/>
  <Video5/>
   <!-- <BibTeX/> -->
</template>