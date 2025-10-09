<script setup>
import { defineComponent } from 'vue'
import { VueLatex } from 'vatex'
import VuePdfEmbed from 'vue-pdf-embed';
const pdfSource3 = '/XYZCYlinder-projectpage/method/main_pipe.pdf';
// const pdfSource4 = '/method/Xnet.pdf';
// const pdfSource5 = '/method/Ynet.pdf';
// const pdfSource6 = '/method/Znet.pdf';
// const pdfSource7 = '/method/hybrid.pdf';

// export default defineComponent({
//   components: {
//     VueLatex
//   }
// })


</script>

<template>
  <div>
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Method Overview</h1>
    </el-row>

    <el-row justify="center" style="margin-top: 20px;">
        <el-col :span="18">
          <vue-pdf-embed :source="pdfSource3" />
        </el-col>
        <el-col :span="18">
          <p>
We introduce XYZCylinder, the 3D feedforward reconstruction model for driving scenes 
<span style="background: linear-gradient(45deg, #8B4513, #DAA520); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-weight: bold;">
  achieving SOTA performance on nuScenes (ego-forward) and Carla-Centric (ego-inward) datasets, and enhance the generalization capability with various camera configurations.
</span> 
We propose the Unified Cylinder Camera Modeling (UCCM) strategy, a deterministic, explicit view transformation method with adjustable training-free parameters 
that provides inherent robustness to varying camera parameters, circumvents the limitations of implicit fixed view transformation approaches, and serves as the foundation for subsequent networks.
For the Unified Cylinder Camera Model, we introduce a hybrid representation decoded by newly designed Cylinder Plane Feature Groups (CPFGs) to decomposes scenes into volume-based, pixel-based, and sky-texture components. 
This factorization with several dedicated modules enables efficient, high-fidelity reconstruction.
          </p>
        </el-col>
    </el-row>
    <!-- <el-row justify="center" style="margin-top: 20px;">
      <el-col  :span="18">
        <p>
          The XNet serves as the core of our Volume-Aware Module, featuring a unique "X-shaped" architecture with a dual-branch encoder and a dual-branch decoder. It is designed to process the Unified Cylindrical Plane (UCP) features and decouple them into independent Geometry and Texture streams. These features are then used to generate the 3D Gaussians that form the main body of the scene, accurately constructing its structure and appearance.
        </p>
      </el-col>
      <el-col :span="18">
        <p>
          The YNet is a versatile network architecture employed in two key modules. It features a "Y-shaped" structure with a dual-branch encoder that merges into a single-branch decoder.

In the Occupancy-Aware Module, YNet generates a coarse geometric layout of the scene, predicting occupied space to efficiently prune empty regions.
In the Pixel-Aware Module, YNet focuses on generating high-frequency details, predicting per-pixel depth offsets and textures to enhance the final rendered quality.
        </p>
      </el-col>
    </el-row>

    <el-row justify="center" style="margin-top: 20px;">
      <el-col :span="18">
        <p>
          The ZNet is a specialized network for our Background Generation Module, designed to synthesize a high-quality sky. It operates by treating the background as a 2D texture mapped onto the UCP. ZNet samples texture features from the main pipeline based on rays cast from the target camera. These view-aligned features are then fed into a StyleGAN2-like synthesis network to generate the final, realistic background image, ensuring a seamless blend with the foreground.
        </p>
      </el-col>
    </el-row> -->

  </div>
</template>

<style scoped>

</style>