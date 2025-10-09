<script setup>
import abstract from '../mds/abstract.mdx';
import VuePdfEmbed from 'vue-pdf-embed';

const pdfSource = '/XYZCYlinder-projectpage/method/tasksettings.pdf';
const pdfSource2 = '/XYZCYlinder-projectpage/method/psnr.pdf';
</script>

<template>
  <div style="margin-top: 0%;">
    <el-divider />

    <el-row justify="center">
        <el-col :span="18">
            <abstract />
        </el-col>
    </el-row>

    <el-row justify="center">
        <el-col :span="18">
            <el-row justify="space-between">
                <!-- 调整第一个 PDF 的宽度比例，这里设置为占据 18 列 -->
                <el-col :span="10">
                    <vue-pdf-embed :source="pdfSource" :height="220" />
                </el-col>
                <!-- 调整第二个 PDF 的宽度比例，这里设置为占据 6 列 -->
                <el-col :span="8">
                    <vue-pdf-embed :source="pdfSource2" :height="220" />
                </el-col>
            </el-row>
        </el-col>
    </el-row>
    

  </div>
</template>

<style scoped>

</style>