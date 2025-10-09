<script>

import Header from './components/Header.vue'
import Main from './components/Main.vue'
import Footer from './components/Footer.vue'

export default {
  components: {
    Header,
    Main,
    Footer,
  }
}

</script>

<template>
    <div class="dark">
      
      <el-container>
        <el-header><Header/></el-header>
        <el-main><Main/></el-main>
        <el-footer><Footer/></el-footer>
      </el-container>
      
      <!-- 回到顶部 -->
      <el-backtop :right="40" :bottom="80" />
    </div>
</template>

<style>

/* Dark theme */
:root {
  --dark-bg: #000000;
  --dark-surface: #0a0a0a;
  --dark-text-primary: #e0e0e0;
  --dark-text-secondary: rgba(255, 255, 255, 0.7);
  --dark-accent: #bb86fc;
  --dark-accent-hover: #dcd0ff;
  --dark-border-color: #333333;
  --dark-code-bg: #1a1a1a;
}

/* 设置整个页面的背景颜色 */
html, body {
  background-color: var(--dark-bg);
  margin: 0;
  padding: 0;
  height: 100vh;
  overflow: hidden;
}

body {
  overflow-y: auto;
}

.dark {
  background-color: var(--dark-bg);
  color: var(--dark-text-primary);
  min-height: 100vh;
  overflow-x: hidden;
  overflow-y: hidden;
}

.dark h1, .dark h2, .dark h3, .dark h4, .dark h5, .dark h6 {
  color: var(--dark-text-primary);
}

.dark p, .dark li {
    color: var(--dark-text-primary);
}

.dark code {
  background-color: var(--dark-code-bg);
  color: var(--dark-text-primary);
  border: 1px solid var(--dark-border-color);
}

.dark pre {
  background-color: var(--dark-code-bg);
  border: 1px solid var(--dark-border-color);
  border-radius: 5px;
  padding: 10px;
}

.dark blockquote {
  background-color: var(--dark-code-bg);
  border-left: 5px solid var(--dark-border-color);
  color: var(--dark-text-primary);
}

.dark a {
  color: var(--dark-accent);
}

.dark a:hover {
  color: var(--dark-accent-hover);
}

.dark img {
  box-shadow: 0 0 10px rgba(255, 255, 255, 0.1);
}

.dark table {
  border: 1px solid var(--dark-border-color);
}

.dark th, .dark td {
  border: 1px solid var(--dark-border-color);
}

/* Header styles */
.dark .el-menu {
  background: transparent !important;
  border-bottom: none !important;
}

.dark .el-menu-item a,
.dark .el-sub-menu__title {
  color: var(--dark-text-primary) !important;
  font-size: 16px;
}

.dark .el-menu-item:hover,
.dark .el-sub-menu .el-sub-menu__title:hover {
  background-color: var(--dark-surface) !important;
}

.dark .el-menu-item.is-active {
  color: var(--dark-accent) !important;
  border-bottom: 2px solid var(--dark-accent) !important;
  background-color: transparent !important;
}

.dark .el-affix--fixed {
  background-image: none !important;
  background-color: var(--dark-surface) !important;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.5) !important;
}

/* General Styles (should be compatible with dark theme) */
h1 {
  font-family: "MyFont", Verdana, sans-serif;
  letter-spacing: 2px;
  font-size: 28px;
  margin: 0px;
  text-align: center;
}

h2, h3, h4, h5, h6 {
  font-family: "MyFont", Verdana, sans-serif;
  letter-spacing: 2px;
}

.el-header, .el-footer {
  padding: 0px;
}

p, li {
  font-size: 16px;
  line-height: 1.75rem;
}

code {
  font-family: 'Consolas', monospace;
  border-radius: 5px;
  padding: 2px 5px;
}

pre {
  line-height: 1.4rem;
}

pre:not(pre[id=bibtex]) {
  overflow-x: auto;
}

pre code {
  border: none;
  padding: 0px;
}

blockquote {
  padding: 10px 20px;
  margin: 0;
}

blockquote > p {
  margin-block-start: 0.5em;
  margin-block-end: 0.5em;
}

a {
  text-decoration: none;
}

a:hover {
  border-bottom: dotted;
}

p > img {
  width: 80%;
  display: block;
  margin: 0 auto;
  border-radius: 10px;
}

table {
  border-collapse: collapse;
  width: max-content;
  max-width: 100%;
  margin: 0 auto;
  display:block;
  overflow-x:auto;
}

thead {
  border-bottom-width: 1px;
  border-top-width: 2px;
  border-left-width: 0px;
  border-right-width: 0px;
  border-style: solid;
  border-color: var(--dark-border-color);
}

tbody tr:last-child {
  border-bottom-width: 2px;
  border-top-width: 0px;
  border-left-width: 0px;
  border-right-width: 0px;
  border-style: solid;
  border-color: var(--dark-border-color);
}

th, td {
    padding-left: 1rem;
    padding-right: 1rem;
}

.katex-display {
  overflow-x: auto;
  overflow-y: hidden;
}

</style>