<template>
  <div class="video-section">
    <el-divider />

    <el-row justify="center">
      <h1 class="section-title">Explainer Video</h1>
    </el-row>

    <el-row justify="center" class="video-gallery">
      <el-col :span="20" :xs="24">
        <div class="video-grid">
          <div 
            v-for="(video, index) in videos" 
            :key="video.id"
            class="video-card"
            :style="{ animationDelay: `${index * 0.1}s` }"
          >
            <!-- 视频封面模式 -->
            <div 
              v-if="!videoStates[video.id]?.isPlaying"
              class="video-cover-container"
              @click="playVideo(video)"
            >
              <div class="video-cover-wrapper">
                <img 
                  :src="video.cover" 
                  :alt="video.title"
                  class="video-cover-image"
                  loading="lazy"
                >
                <div class="video-overlay">
                  <div class="play-button">
                    <svg class="play-icon" viewBox="0 0 24 24" fill="currentColor">
                      <path d="M8 5v14l11-7z"/>
                    </svg>
                  </div>
                  <div class="video-info">
                    <h3 class="video-title">{{ video.title }}</h3>
                    <p class="video-duration">{{ video.duration || '0:30' }}</p>
                  </div>
                </div>
              </div>
            </div>

            <!-- 视频播放器模式 -->
            <div v-else class="video-player-container">
              <video
                :ref="el => setVideoRef(video.id, el)"
                class="video-js-player"
                controls
                autoplay
                :poster="video.cover"
                @ended="onVideoEnded(video.id)"
              >
                <source :src="video.video" type="video/mp4">
                您的浏览器不支持视频播放。
              </video>
              <button 
                class="close-video-btn"
                @click="closeVideo(video.id)"
                title="关闭视频"
              >
                <svg viewBox="0 0 24 24" fill="currentColor">
                  <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
                </svg>
              </button>
            </div>
          </div>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script setup>
import { ref, reactive, onMounted, nextTick } from 'vue'

// 视频播放器实例
const videoRefs = reactive({})
const videoStates = reactive({})

// 视频数据
const videos = [
  { 
    id: 'video-120',
    title: 'Explainer Video',
    cover: '/video/cover.png',
    video: '/video/explainer.mp4',
    duration: '4:29'
  }
]

// 设置视频引用
const setVideoRef = (videoId, el) => {
  if (el) {
    videoRefs[videoId] = el
  }
}

// 播放视频
const playVideo = async (video) => {
  if (!videoStates[video.id]) {
    videoStates[video.id] = {}
  }
  videoStates[video.id].isPlaying = true
  
  // 等待DOM更新
  await nextTick()
  
  // 播放视频
  const videoElement = videoRefs[video.id]
  if (videoElement) {
    try {
      // 取消静音
      videoElement.muted = false
      await videoElement.play()
    } catch (error) {
      console.error('视频播放失败:', error)
      // 如果播放失败，保持静音并尝试播放
      videoElement.muted = true
      videoElement.play()
    }
  }
}

// 关闭视频
const closeVideo = (videoId) => {
  const videoElement = videoRefs[videoId]
  if (videoElement) {
    videoElement.pause()
    videoElement.currentTime = 0
  }
  if (videoStates[videoId]) {
    videoStates[videoId].isPlaying = false
  }
}

// 视频播放结束
const onVideoEnded = (videoId) => {
  console.log(`Video ${videoId} ended, returning to cover`)
  closeVideo(videoId)
}

// 初始化
onMounted(() => {
  // 初始化视频状态
  videos.forEach(video => {
    videoStates[video.id] = { isPlaying: false }
  })
})
</script>

<style scoped>
.video-section {
  padding: 20px 0;
}

.section-title {
  font-size: 2.5rem;
  font-weight: 700;
  margin: 2rem 0;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.video-gallery {
  margin: 2rem 0;
}

.video-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 2rem;
  padding: 1rem;
}

.video-card {
  background: white;
  border-radius: 16px;
  overflow: hidden;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  transition: all 0.3s ease;
  animation: fadeInUp 0.6s ease forwards;
  opacity: 0;
  transform: translateY(20px);
}

.video-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.15);
}

@keyframes fadeInUp {
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.video-cover-container {
  position: relative;
  width: 100%;
  cursor: pointer;
  aspect-ratio: 16/9;
  overflow: hidden;
}

.video-cover-wrapper {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 12px;
  overflow: hidden;
}

.video-cover-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.4s ease, filter 0.4s ease;
}

.video-cover-container:hover .video-cover-image {
  transform: scale(1.1);
  filter: brightness(0.7);
}

.video-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(
    to bottom,
    transparent 0%,
    rgba(0, 0, 0, 0.1) 60%,
    rgba(0, 0, 0, 0.7) 100%
  );
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  transition: all 0.3s ease;
}

.play-button {
  width: 70px;
  height: 70px;
  background: rgba(255, 255, 255, 0.9);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}

.play-button:hover {
  transform: scale(1.1);
  background: rgba(255, 255, 255, 1);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.3);
}

.play-icon {
  width: 30px;
  height: 30px;
  color: #667eea;
  margin-left: 3px;
}

.video-info {
  position: absolute;
  bottom: 20px;
  left: 20px;
  right: 20px;
  color: white;
  text-shadow: 0 2px 4px rgba(0, 0, 0, 0.5);
}

.video-title {
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0 0 5px 0;
  line-height: 1.3;
}

.video-duration {
  font-size: 0.9rem;
  opacity: 0.8;
  margin: 0;
}

.video-player-container {
  position: relative;
  width: 100%;
  aspect-ratio: 16/9;
  border-radius: 12px;
  overflow: hidden;
}

.video-js-player {
  width: 100%;
  height: 100%;
  border-radius: 12px;
}

.close-video-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  width: 40px;
  height: 40px;
  background: rgba(0, 0, 0, 0.7);
  border: none;
  border-radius: 50%;
  color: white;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.3s ease;
  z-index: 10;
  backdrop-filter: blur(10px);
}

.close-video-btn:hover {
  background: rgba(0, 0, 0, 0.9);
  transform: scale(1.1);
}

.close-video-btn svg {
  width: 20px;
  height: 20px;
}

/* 响应式设计 */
@media (max-width: 1200px) {
  .video-grid {
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
  }
}

@media (max-width: 768px) {
  .section-title {
    font-size: 2rem;
  }
  
  .video-grid {
    grid-template-columns: 1fr;
    gap: 1rem;
    padding: 0.5rem;
  }
  
  .video-card {
    margin: 0;
  }
  
  .play-button {
    width: 60px;
    height: 60px;
  }
  
  .play-icon {
    width: 25px;
    height: 25px;
  }
  
  .video-title {
    font-size: 1.1rem;
  }
}

@media (max-width: 480px) {
  .section-title {
    font-size: 1.8rem;
    margin: 1.5rem 0;
  }
  
  .video-grid {
    gap: 1rem;
    padding: 0.5rem;
  }
  
  .video-cover-container {
    aspect-ratio: 16/9;
  }
  
  .play-button {
    width: 50px;
    height: 50px;
  }
  
  .play-icon {
    width: 20px;
    height: 20px;
  }
  
  .video-info {
    bottom: 15px;
    left: 15px;
    right: 15px;
  }
  
  .video-title {
    font-size: 1rem;
  }
  
  .video-duration {
    font-size: 0.8rem;
  }
}

/* 暗色主题支持 */
@media (prefers-color-scheme: dark) {
  .video-card {
    background: #1a1a1a;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
  }
  
  .video-card:hover {
    box-shadow: 0 8px 30px rgba(0, 0, 0, 0.4);
  }
  
  .play-button {
    background: rgba(255, 255, 255, 0.1);
    backdrop-filter: blur(15px);
  }
  
  .play-button:hover {
    background: rgba(255, 255, 255, 0.2);
  }
  
  .play-icon {
    color: #ffffff;
  }
}
</style>