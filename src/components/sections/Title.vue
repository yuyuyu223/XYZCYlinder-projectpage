<script lang="ts" setup>

import { ElIcon } from 'element-plus'
import { Document, Files, MagicStick, Picture, DataAnalysis, Film } from '@element-plus/icons-vue'
import { ref, computed } from 'vue'

// 匿名切换, true to be anonymous, false to be myself.
const isAnonymous = ref(false);

// logo地址，没有则置为""即可
const logo = '/XYZCYlinder-projectpage/logogif.gif'

// 标题
// const title = 'XYZCylinder: A Unified Feedforward Reconstruction Model for Driving Scene Based on Cylindrical Feature Field Representation'

// 标题补充，没有则置为''即可
const title_supp = ''

// 按钮颜色
const btn_color = '#444444'

// 作者清单（包含作者姓名、头像、主页、地址序号）
const authors = computed(() => {
  if (isAnonymous.value) {
    return [
      {
        name: "Anonymous",
        icon: "/void.webp",
        homepage: "#",
        address_flag: "1"
      }
    ]
  } else {
    return [
      {
        name: "Haochen Yu",
        icon: "https://scholar.googleusercontent.com/citations?view_op=view_photo&user=P8hQuaYAAAAJ&citpid=1",
        homepage: "https://yuyuyu223.github.io/howsenfisher.github.io/",
        address_flag: "1"
      },
      {
        name: "Qiankun Liu",
        icon: "https://scholar.googleusercontent.com/citations?view_op=view_photo&user=TNDbzzMAAAAJ&citpid=6",
        homepage: "https://scholar.google.com/citations?user=TNDbzzMAAAAJ&hl=zh-CN",
        address_flag: "1,*"
      },
      {
        name: "Hongyuan Liu",
        icon: "https://www.3dimagelab.com/wp-content/uploads/2024/01/刘洪源.jpg",
        homepage: "https://scholar.google.com/citations?user=Gt3-rnAAAAAJ&hl=zh-CN",
        address_flag: "1"
      },
      {
        name: "Jianfei Jiang",
        icon: "https://scholar.googleusercontent.com/citations?view_op=view_photo&user=bK_d01YAAAAJ&citpid=3",
        homepage: "https://jianfeij.github.io/",
        address_flag: "1"
      },
      {
        name: "Juntao Lyu",
        icon: "/XYZCYlinder-projectpage/juntao_lyu.jpg",
        homepage: "https://openreview.net/profile?id=~Juntao_Lyu1",
        address_flag: "1"
      },
      {
        name: "Jiansheng Chen",
        icon: "https://scholar.googleusercontent.com/citations?view_op=view_photo&user=A1gA9XIAAAAJ&citpid=5",
        homepage: "https://scholar.google.com/citations?user=A1gA9XIAAAAJ&hl=zh-CN",
        address_flag: "1"
      },
      {
        name: "Huimin Ma",
        icon: "https://www.3dimagelab.com/wp-content/uploads/2024/11/cxk8b2c29FyAcDsWAAI-dhljbIw134-150x150.png",
        homepage: "http://www.3dimagelab.com/index.php/huimin-ma/",
        address_flag: "1,*"
      },
    ]
  }
})

// 地址清单（包含地址名称、头像、主页、地址序号）
const addresses = computed(() => {
  if (isAnonymous.value) {
    return [
      {
        address_flag: "1",
        name: "Anonymous Institution",
        icon: "/void.webp",
        homepage: "#"
      },
    ]
  } else {
    return [
      {
        address_flag: "1",
        name: "University of Science and Technology Beijing",
        icon: "https://www.ustb.edu.cn/images/content/2017-04/20170418142450153379.png",
        homepage: "https://www.ustb.edu.cn/"
      },
    ]
  }
})

// 共一和通讯提示
const con_and_corresponding_author = computed(() => {
    if (isAnonymous.value) {
        return ""
    }
    return "*: Corresponding Authors."
})


// 最新消息
const news = "🔥 [2024-12-15] This template project is still under development."

// 强调内容
const emphases = [
  // "🎉 [ICLR 2026] Poster",
]

// 提供引导资料链接
const buttons = [
  {
    disabled: true,
    name: "Paper",
    component: Document,
  },
  // {
  //   disabled: true,
  //   name: "中译版",
  //   component: Document,
  // },
  {
    disabled: false,
    name: "Code (coming soon)",
    link: "https://github.com/yuyuyu223/XYZCylinder",
    component: Files,
  },
  {
    disabled: false,
    name: "Demo",
    link: "#",
    component: MagicStick,
  },
  {
    disabled: false,
    name: "Poster",
    link: "#",
    component: Picture,
  },
  {
    disabled: false,
    name: "Slide",
    link: "#",
    component: DataAnalysis,
  },
  // {
  //   disabled: false,
  //   name: "Video (减论)",
  //   link: "https://www.bilibili.com/video/BV15XkgYiE73/",
  //   component: Film,
  // },
  // {
  //   disabled: false,
  //   name: "Video (Tutorial)",
  //   link: "https://www.bilibili.com/video/BV1oUrfYzEqZ",
  //   component: Film,
  // },
]

</script>

<template>
  <div class="title-container">

    <!-- 最新消息提示 -->
    <!-- <el-row justify="center">
      <el-col :span="24">
        <el-alert :title="news" type="success" />
      </el-col>
    </el-row> -->

    <!-- 文章logo -->
    <el-row v-if="logo" justify="center">
      <el-image :src="logo" class="logo" fit="contain" />
    </el-row>

    <!-- 文章标题 -->
    <el-row justify="center">
      <el-col :span="20">
        <h1 class="paper-title">
          <span style="background: linear-gradient(45deg, #667eea, #764ba2); -webkit-background-clip: text; -webkit-text-fill-color: transparent; white-space: nowrap; font-weight: bold;">X</span><span style="background: linear-gradient(45deg, #11998e, #38ef7d); -webkit-background-clip: text; -webkit-text-fill-color: transparent; white-space: nowrap; font-weight: bold;">Y</span><span style="background: linear-gradient(45deg, #f093fb, #f5576c); -webkit-background-clip: text; -webkit-text-fill-color: transparent; white-space: nowrap; font-weight: bold;">Z</span><span style="background: linear-gradient(45deg, #4facfe, #00f2fe); -webkit-background-clip: text; -webkit-text-fill-color: transparent; white-space: nowrap; font-weight: bold;">Cylinder</span><span style="background: linear-gradient(45deg, #ff6347, #ff6347, #ff6347); -webkit-background-clip: text; -webkit-text-fill-color: transparent; font-weight: bold;">: Feedforward Reconstruction for Driving Scenes Based on A Unified Cylinder Lifting Method</span>
          <span v-if="title_supp" class="title-supp"> {{ title_supp }}</span>
        </h1>
      </el-col>
    </el-row>

    <!-- 作者名单 -->
    <el-row justify="center">
      <a :href=author.homepage v-for="author in authors">
        <el-button class="title-button" type="primary" text>
          <el-avatar v-if="author.icon" :size="40" :src="author.icon" />
          <span class="author">
            {{ author.name }}<sup v-if="author.address_flag" class="name_sup">{{ author.address_flag }}</sup>
          </span>
        </el-button>
      </a>
    </el-row>

    <!-- 地址名单 -->
    <el-row justify="center">
      <a :href=address.homepage v-for="address in addresses">
        <el-button class="title-button" type="primary" text>
          <el-avatar v-if="address.icon" :size="40" :src="address.icon" />
          <span class="address">
            <sup v-if="address.address_flag" class="address_sup">{{ address.address_flag }}</sup>{{ address.name }}
          </span>
        </el-button>
      </a>
    </el-row>

    <!-- 共一和通讯提示内容 -->
    <el-row justify="center" class="con-cor">
        {{ con_and_corresponding_author }}
    </el-row>

    <!-- 强调内容 -->
    <el-row justify="center" class="emphasis" v-for="emphasis in emphases">
        {{ emphasis }}
    </el-row>

    <!-- 提供引导按钮 -->
    <el-row justify="center" style="margin-bottom: 20px;">
      <el-col :span="20">
        <el-row justify="center">
          <a :href=button.link v-for="button in buttons">
            <el-button class="guidance-button" size="default" :color="btn_color" :disabled="button.disabled" round>
              <el-icon :size="18">
                <component :is="button.component" />
              </el-icon>
              <span class="btn-text">{{ button.name }}</span>
            </el-button>
          </a>
        </el-row>
      </el-col>
    </el-row>

  </div>
</template>

<style scoped>

.title-container {
  background-color: #f0f2f5;
}

.dark .title-container {
  background-color: var(--dark-bg);
}

/* 文章标题字体、字间距、居中排布、字号 */
.paper-title {
  font-family: "MyFont", Verdana, sans-serif;
  letter-spacing: 2px;
  font-size: 42px;
  margin: 32px;
  text-align: center;
}

.gradient-text {
  -webkit-background-clip: text !important;
  background-clip: text !important;
  color: transparent !important;
}

.gradient-x {
  background: linear-gradient(45deg, #0077b6, #00b4d8);
}

.gradient-y {
  background: linear-gradient(45deg, #52b788, #95d5b2);
}

.gradient-z {
  background: linear-gradient(45deg, #ffb703, #fdc500);
}

.gradient-cylinder {
  background: linear-gradient(45deg, #e65c00, #ff8c00);
}

.dark .gradient-x {
  background: linear-gradient(45deg, #00b4d8, #90e0ef);
}

.dark .gradient-y {
  background: linear-gradient(45deg, #95d5b2, #d8f3dc);
}

.dark .gradient-z {
  background: linear-gradient(45deg, #fdc500, #ffe169);
}

.dark .gradient-cylinder {
  background: linear-gradient(to right, #ff8000, #e67300);
}

.gradient-rest {
  background: linear-gradient(45deg, #ff6b6b 0%, #4ecdc4 50%, #45b7d1 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent !important;
  white-space: nowrap;
}

.dark .gradient-rest {
  background: linear-gradient(45deg, #ff9a9e 0%, #fad0c4 50%, #a18cd1 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent !important;
  white-space: nowrap;
}

.title-supp {
  color: #42B883;
}

.dark .title-supp {
  color: var(--dark-accent);
}

/* 姓名和地址按钮 */
.title-button {
  margin: 10px 3px;
}

/* 姓名和地址按钮光标悬浮 */
.title-button:hover {
  margin: 10px 8px;
  background-color: rgba(0, 0, 0, 0.05);
}

.dark .title-button:hover {
  background-color: rgba(255, 255, 255, 0.1);
}

/* 引导材料按钮 */
.guidance-button {
  margin: 8px 5px;
  box-shadow: #d8d8d8 1px 1px 1px 1px;
}

.dark .guidance-button {
  background-color: var(--dark-surface);
  color: var(--dark-text-primary);
  box-shadow: 0 1px 3px rgba(0,0,0,0.12), 0 1px 2px rgba(0,0,0,0.24);
}

.dark .guidance-button .btn-text {
  color: var(--dark-text-primary);
}

/* 姓名属性 */
.author {
  font-size: 18px;
  margin-left: 3px;
}

.dark .author {
  color: var(--dark-text-secondary);
}

/* 姓名上标属性 */
.name_sup {
  color: #606266; 
  margin-left: 3px;
}

.dark .name_sup {
  color: var(--dark-text-secondary);
}

/* 地址属性 */
.address {
  font-size: 18px;
}

.dark .address {
  color: var(--dark-text-secondary);
}

/* 地址上标属性 */
.address_sup {
  color: #606266; 
  margin-right: 1px;
}

.dark .address_sup {
  color: var(--dark-text-secondary);
}

/* 头像属性 */
.el-avatar {
  margin-right: 6px;
  box-shadow: #b7b7b7 0px 0px 3px 1px;
}

.dark .el-avatar {
  box-shadow: 0 0 5px rgba(255, 255, 255, 0.2);
}

/* 共一和通讯文字属性 */
.con-cor {
  font-family: Arial;
  font-size: 14px;
  margin: 18px 0px;
  text-align: center;
}

.dark .con-cor {
  color: var(--dark-text-secondary);
}

/* 强调信息属性 */
.emphasis {
  color: chocolate;
  font-weight: bold;
  margin: 8px;
  font-size: 22px;
  text-align: center;
}

.dark .emphasis {
  color: var(--dark-accent);
}

/* 引导材料按钮文字属性 */
.btn-text {
  font-size: 18px;
  color: #ffffff;
}

.el-alert {
  margin: 10px 0 0;
}

.el-alert:first-child {
  margin: 0;
}

.dark .el-alert {
  background-color: var(--dark-surface);
  color: var(--dark-text-primary);
}

.logo {
  width: 250px; 
  height: 150px;
  /* border-radius: 10%; */
  /* box-shadow: #ced3dc 0px 0px 3px 2px; */
  margin-top: 40px;
}

.dark .logo {
  box-shadow: 0 0 8px rgba(255, 255, 255, 0.15);
}

/* 手机端链接样式处理 */
a:-webkit-any-link {
  text-decoration: none;
}

/* 取消鼠标焦点悬浮在链接上的颜色装饰 */
a:hover {
  color: inherit;
  border-bottom: none;
}

/* 链接装饰，取消下划线和链接颜色 */
a {
	text-decoration: None;
	color: inherit;
}
</style>