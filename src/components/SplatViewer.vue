<script setup lang="ts">
import { onMounted, onUnmounted, ref, watch } from 'vue';
import * as GaussianSplats3D from '@mkkellogg/gaussian-splats-3d';

// 定义组件的props接口
interface SplatViewerProps {
  splatPath: string;
  viewerId?: string;
  width?: string;
  height?: string;
  aspectRatio?: string;
  cameraUp?: [number, number, number];
  initialCameraPosition?: [number, number, number];
  initialCameraLookAt?: [number, number, number];
  splatPosition?: [number, number, number];
  splatRotation?: [number, number, number, number];
  splatScale?: [number, number, number];
  splatAlphaRemovalThreshold?: number;
  showLoadingUI?: boolean;
  enableControls?: boolean;
  sharedMemoryForWorkers?: boolean;
  timeout?: number; // Timeout in milliseconds
}

// 设置默认值
const props = withDefaults(defineProps<SplatViewerProps>(), {
  viewerId: () => `splat-viewer-${Math.random().toString(36).substr(2, 9)}`,
  width: '100%',
  height: 'auto',
  aspectRatio: '16/9',
  cameraUp: () => [0, -1, 0],
  initialCameraPosition: () => [0, 0, -5],
  initialCameraLookAt: () => [0, 0, 0],
  splatPosition: () => [0, 0, 0],
  splatRotation: () => [1, 0, 0, 0],
  splatScale: () => [0.05, 0.05, 0.05],
  splatAlphaRemovalThreshold: 5,
  showLoadingUI: true,
  enableControls: false,
  sharedMemoryForWorkers: false,
  timeout: 30000 // Default timeout 30s
});

const emit = defineEmits<{
  viewerReady: [viewer: any];
  loadComplete: [];
  error: [error: Error];
}>();

const containerRef = ref<HTMLElement>();
let viewer: any = null;
let isLoaded = ref(false);
let loadError = ref<Error | null>(null);

// Promise timeout wrapper
function withTimeout<T>(promise: Promise<T>, ms: number): Promise<T> {
  return new Promise((resolve, reject) => {
    const timeoutId = setTimeout(() => {
      reject(new Error(`Operation timed out after ${ms} ms`));
    }, ms);

    promise.then(
      (res) => {
        clearTimeout(timeoutId);
        resolve(res);
      },
      (err) => {
        clearTimeout(timeoutId);
        reject(err);
      }
    );
  });
}

onMounted(async () => {
  if (!containerRef.value) return;

  try {
    loadError.value = null;
    // 创建viewer实例
    viewer = new GaussianSplats3D.Viewer({
      rootElement: containerRef.value,
      cameraUp: props.cameraUp,
      initialCameraPosition: props.initialCameraPosition,
      initialCameraLookAt: props.initialCameraLookAt,
      sharedMemoryForWorkers: props.sharedMemoryForWorkers,
    });

    // 添加场景 with timeout
    await withTimeout(viewer.addSplatScene(props.splatPath, {
      splatAlphaRemovalThreshold: props.splatAlphaRemovalThreshold,
      showLoadingUI: props.showLoadingUI,
      position: props.splatPosition,
      rotation: props.splatRotation,
      scale: props.splatScale
    }), props.timeout);

    // 启动viewer
    viewer.start();
    
    // 根据配置决定是否启用控制
    if (!props.enableControls) {
      viewer.perspectiveControls?.stopListenToKeyEvents();
      viewer.orthographicControls?.stopListenToKeyEvents();
    }

    isLoaded.value = true;
    emit('viewerReady', viewer);
    emit('loadComplete');

  } catch (error) {
    console.error('Failed to load splat scene:', error);
    loadError.value = error as Error;
    emit('error', error as Error);
  }
});

onUnmounted(() => {
  if (viewer) {
    viewer.dispose?.();
    viewer = null;
  }
});

// 监听splatPath变化，重新加载场景
watch(() => props.splatPath, async (newPath) => {
  if (viewer && newPath) {
    try {
      isLoaded.value = false;
      loadError.value = null;
      await withTimeout(viewer.addSplatScene(newPath, {
        splatAlphaRemovalThreshold: props.splatAlphaRemovalThreshold,
        showLoadingUI: props.showLoadingUI,
        position: props.splatPosition,
        rotation: props.splatRotation,
        scale: props.splatScale
      }), props.timeout);
      isLoaded.value = true;
    } catch (error) {
      console.error('Failed to reload splat scene:', error);
      loadError.value = error as Error;
      emit('error', error as Error);
    }
  }
});

// 暴露方法给父组件
defineExpose({
  getViewer: () => viewer,
  isLoaded: () => isLoaded.value,
  dispose: () => {
    if (viewer) {
      viewer.dispose?.();
      viewer = null;
    }
  }
});
</script>

<template>
  <div class="splat-viewer-container">
    <div
      ref="containerRef"
      :id="viewerId"
      class="splat-viewer"
      :style="{
        width: width,
        height: height,
        aspectRatio: height === 'auto' ? aspectRatio : undefined
      }"
    />
    <div v-if="!isLoaded && !loadError" class="loading-overlay">
      <div class="loading-spinner">Loading 3DGS...</div>
    </div>
    <div v-if="loadError" class="loading-overlay">
      <div class="error-message">
        <p>Error loading 3D model.</p>
        <p>Details: {{ loadError.message }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.splat-viewer-container {
  position: relative;
  width: 100%;
}

.splat-viewer {
  width: 100%;
  height: 100%;
  min-height: 200px;
}

.loading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 0, 0, 0.1);
  border-radius: 8px;
}

.loading-spinner {
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
}

.error-message {
  padding: 20px;
  background: white;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0,0,0,0.15);
  color: red;
  text-align: center;
}

/* 覆盖GaussianSplats3D的默认样式 */
:deep(.spinnerPrimary0) {
  display: none !important;
}

:deep(.spinnerOuterContainer0) {
  height: 100% !important;
  margin: 0 auto !important;
  top: 0 !important;
  left: 0 !important;
}

:deep(.spinnerContainerPrimary0) {
  padding-top: 0% !important;
  position: relative !important;
  transform: none !important;
  width: fit-content !important;
  margin: 0 auto !important;
  left: 0 !important;
  padding: 10px 20px !important;
}

:deep(.messageContainerPrimary0) {
  padding-top: 0% !important;
}
</style>