<script lang="ts" setup>

import { onMounted, ref } from 'vue';
import * as echarts from 'echarts';

// Carla-Centric 数据集结果
const carlaData = [
  {
    model: 'SplatterImage',
    psnr: 13.04,
    lpips: 0.708,
    ssim: 0.448,
    pcc: 0.180,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'PixelSplat',
    psnr: 14.67,
    lpips: 0.565,
    ssim: 0.412,
    pcc: 0.554,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'MVSplat',
    psnr: 15.32,
    lpips: 0.507,
    ssim: 0.457,
    pcc: 0.566,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'DepthSplat',
    psnr: 16.27,
    lpips: 0.503,
    ssim: 0.508,
    pcc: 0.581,
    psnrHighlight: 'third-best',
    lpipsHighlight: 'third-best',
    ssimHighlight: 'third-best',
    pccHighlight: ''
  },
  {
    model: 'DrivingForward',
    psnr: 15.38,
    lpips: 0.541,
    ssim: 0.445,
    pcc: 0.696,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: 'second-best'
  },
  {
    model: 'Omni-Scene',
    psnr: 15.54,
    lpips: 0.558,
    ssim: 0.462,
    pcc: 0.551,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: '6img-to-3d',
    psnr: 17.33,
    lpips: 0.485,
    ssim: 0.620,
    pcc: 0.765,
    psnrHighlight: 'second-best',
    lpipsHighlight: 'second-best',
    ssimHighlight: 'second-best',
    pccHighlight: 'third-best'
  },
  {
    model: 'Ours',
    psnr: 18.40,
    lpips: 0.359,
    ssim: 0.622,
    pcc: 0.817,
    psnrHighlight: 'best',
    lpipsHighlight: 'best',
    ssimHighlight: 'best',
    pccHighlight: 'best'
  }
]

// Nuscenes 数据集结果
const nuscenesData = [
  {
    model: 'SplatterImage',
    psnr: 17.31,
    lpips: 0.661,
    ssim: 0.442,
    pcc: 0.027,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'PixelSplat',
    psnr: 21.33,
    lpips: 0.376,
    ssim: 0.607,
    pcc: 0.077,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'MVSplat',
    psnr: 21.87,
    lpips: 0.342,
    ssim: 0.621,
    pcc: 0.201,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'DepthSplat',
    psnr: 23.19,
    lpips: 0.339,
    ssim: 0.675,
    pcc: 0.431,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: ''
  },
  {
    model: 'DrivingForward',
    psnr: 23.84,
    lpips: 0.280,
    ssim: 0.739,
    pcc: 0.437,
    psnrHighlight: 'third-best',
    lpipsHighlight: '',
    ssimHighlight: 'second-best',
    pccHighlight: ''
  },
  {
    model: 'Omni-Scene',
    psnr: 24.11,
    lpips: 0.242,
    ssim: 0.734,
    pcc: 0.806,
    psnrHighlight: 'second-best',
    lpipsHighlight: 'second-best',
    ssimHighlight: 'third-best',
    pccHighlight: 'second-best'
  },
  {
    model: '6img-to-3d',
    psnr: 20.74,
    lpips: 0.650,
    ssim: 0.560,
    pcc: 0.570,
    psnrHighlight: '',
    lpipsHighlight: '',
    ssimHighlight: '',
    pccHighlight: 'third-best'
  },
  {
    model: 'Ours',
    psnr: 24.97,
    lpips: 0.231,
    ssim: 0.750,
    pcc: 0.887,
    psnrHighlight: 'best',
    lpipsHighlight: 'best',
    ssimHighlight: 'best',
    pccHighlight: 'best'
  }
]

// 排序函数

const sortPSNR = (a: any, b: any) => {
  return a.psnr - b.psnr; // 升序排序，PSNR越大越好
}

const sortLPIPS = (a: any, b: any) => {
  return b.lpips - a.lpips; // 升序排序，LPIPS越小越好
}

const sortSSIM = (a: any, b: any) => {
  return a.ssim - b.ssim; // 升序排序，SSIM越大越好
}

const sortPCC = (a: any, b: any) => {
  return a.pcc - b.pcc; // 升序排序，PCC越大越好
}

// 数据归一化函数
const normalizeData = (data: any[], metric: string, isHigherBetter: boolean = true) => {
  const values = data.map(item => item[metric]);
  const max = Math.max(...values);
  const min = Math.min(...values);
  
  return data.map(item => {
    const normalized = isHigherBetter ? 
      (item[metric] - min) / (max - min) : 
      (max - item[metric]) / (max - min);
    return Math.max(0, Math.min(1, normalized)); // 确保在0-1范围内
  });
}

// 计算归一化数据用于雷达图
const getRadarData = () => {
  // Carla数据归一化
  const carlaPsnrNorm = normalizeData(carlaData, 'psnr', true);
  const carlaLpipssNorm = normalizeData(carlaData, 'lpips', false);
  const carlaSsimNorm = normalizeData(carlaData, 'ssim', true);
  const carlaPccNorm = normalizeData(carlaData, 'pcc', true);
  
  // Nuscenes数据归一化
  const nuscenesPsnrNorm = normalizeData(nuscenesData, 'psnr', true);
  const nuscenesLpipssNorm = normalizeData(nuscenesData, 'lpips', false);
  const nuscenesSsimNorm = normalizeData(nuscenesData, 'ssim', true);
  const nuscenesPccNorm = normalizeData(nuscenesData, 'pcc', true);
  
  return {
    carlaPsnrNorm,
    carlaLpipssNorm,
    carlaSsimNorm,
    carlaPccNorm,
    nuscenesPsnrNorm,
    nuscenesLpipssNorm,
    nuscenesSsimNorm,
    nuscenesPccNorm
  };
}

const chartContainer = ref(null);

// 生成动态的雷达图配置
const generateRadarOption = () => {
  const radarNormData = getRadarData();
  
  return {
    "backgroundColor": "#1f1f1f",
    "animation": true,
    "animationThreshold": 2000,
    "animationDuration": 1000,
    "animationEasing": "cubicOut" as const,
    "animationDelay": 0,
    "animationDurationUpdate": 300,
    "animationEasingUpdate": "cubicOut" as const,
    "animationDelayUpdate": 0,
    "aria": {
        "enabled": false
    },
    "series": [
        {
            "type": "radar",
            "name": "SplatterImage",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[0],
                  radarNormData.carlaLpipssNorm[0], 
                  radarNormData.carlaSsimNorm[0],
                  radarNormData.carlaPccNorm[0],
                  radarNormData.nuscenesPsnrNorm[0],
                  radarNormData.nuscenesLpipssNorm[0],
                  radarNormData.nuscenesSsimNorm[0],
                  radarNormData.nuscenesPccNorm[0]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 0,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#1f77b4"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#1f77b4"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "PixelSplat",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[1],
                  radarNormData.carlaLpipssNorm[1], 
                  radarNormData.carlaSsimNorm[1],
                  radarNormData.carlaPccNorm[1],
                  radarNormData.nuscenesPsnrNorm[1],
                  radarNormData.nuscenesLpipssNorm[1],
                  radarNormData.nuscenesSsimNorm[1],
                  radarNormData.nuscenesPccNorm[1]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 1,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#b2df8a"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#b2df8a"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "MVSplat",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[2],
                  radarNormData.carlaLpipssNorm[2], 
                  radarNormData.carlaSsimNorm[2],
                  radarNormData.carlaPccNorm[2],
                  radarNormData.nuscenesPsnrNorm[2],
                  radarNormData.nuscenesLpipssNorm[2],
                  radarNormData.nuscenesSsimNorm[2],
                  radarNormData.nuscenesPccNorm[2]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 2,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#ffbb33"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#ffbb33"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "DepthSplat",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[3],
                  radarNormData.carlaLpipssNorm[3], 
                  radarNormData.carlaSsimNorm[3],
                  radarNormData.carlaPccNorm[3],
                  radarNormData.nuscenesPsnrNorm[3],
                  radarNormData.nuscenesLpipssNorm[3],
                  radarNormData.nuscenesSsimNorm[3],
                  radarNormData.nuscenesPccNorm[3]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 3,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#d62728"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#d62728"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "DrivingForward",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[4],
                  radarNormData.carlaLpipssNorm[4], 
                  radarNormData.carlaSsimNorm[4],
                  radarNormData.carlaPccNorm[4],
                  radarNormData.nuscenesPsnrNorm[4],
                  radarNormData.nuscenesLpipssNorm[4],
                  radarNormData.nuscenesSsimNorm[4],
                  radarNormData.nuscenesPccNorm[4]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 4,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#17becf"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#17becf"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "Omni-Scene",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[5],
                  radarNormData.carlaLpipssNorm[5], 
                  radarNormData.carlaSsimNorm[5],
                  radarNormData.carlaPccNorm[5],
                  radarNormData.nuscenesPsnrNorm[5],
                  radarNormData.nuscenesLpipssNorm[5],
                  radarNormData.nuscenesSsimNorm[5],
                  radarNormData.nuscenesPccNorm[5]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 5,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#33a02c"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#33a02c"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "6img-to-3d",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[6],
                  radarNormData.carlaLpipssNorm[6], 
                  radarNormData.carlaSsimNorm[6],
                  radarNormData.carlaPccNorm[6],
                  radarNormData.nuscenesPsnrNorm[6],
                  radarNormData.nuscenesLpipssNorm[6],
                  radarNormData.nuscenesSsimNorm[6],
                  radarNormData.nuscenesPccNorm[6]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 6,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 2,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#ff7f0e"
            },
            "areaStyle": {
                "opacity": 0.15,
                "color": "#ff7f0e"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        },
        {
            "type": "radar",
            "name": "Ours",
            "data": [
                [
                  radarNormData.carlaPsnrNorm[7],
                  radarNormData.carlaLpipssNorm[7], 
                  radarNormData.carlaSsimNorm[7],
                  radarNormData.carlaPccNorm[7],
                  radarNormData.nuscenesPsnrNorm[7],
                  radarNormData.nuscenesLpipssNorm[7],
                  radarNormData.nuscenesSsimNorm[7],
                  radarNormData.nuscenesPccNorm[7]
                ]
            ],
            "label": {
                "show": false,
                "margin": 8,
                "valueAnimation": false
            },
            "selectedMode": false,
            "zlevel": 0,
            "z": 8,
            "itemStyle": {
                "normal": {}
            },
            "lineStyle": {
                "show": true,
                "width": 4,
                "opacity": 1,
                "curveness": 0,
                "type": "solid",
                "color": "#9467bd"
            },
            "areaStyle": {
                "opacity": 0.25,
                "color": "#9467bd"
            },
            "rippleEffect": {
                "show": true,
                "brushType": "stroke",
                "scale": 2.5,
                "period": 4
            }
        }
    ],
    "legend": [
        {
            "data": [
                "SplatterImage",
                "PixelSplat",
                "MVSplat",
                "DepthSplat",
                "DrivingForward",
                "Omni-Scene",
                "6img-to-3d",
                "Ours"
            ],
            "selected": {},
            "show": true,
            "left": "right",
            "top": "middle",
            "orient": "vertical",
            "padding": 10,
            "itemGap": 15,
            "itemWidth": 25,
            "itemHeight": 14,
            "textStyle": {
                "color": "#eee",
                "fontSize": 20,
                "fontWeight": "normal"
            },
            "backgroundColor": "rgba(30, 30, 30, 0.7)",
            "borderColor": "#666",
            "borderWidth": 1,
            "borderRadius": 0,
            "pageButtonItemGap": 5,
            "pageButtonPosition": "end",
            "pageFormatter": "{current}/{total}",
            "pageIconColor": "#2f4554",
            "pageIconInactiveColor": "#aaa",
            "pageIconSize": 15,
            "animationDurationUpdate": 800,
            "selector": false,
            "selectorPosition": "auto",
            "selectorItemGap": 7,
            "selectorButtonGap": 10
        }
    ],
    "tooltip": {
        "show": true,
        "trigger": "item",
        "triggerOn": "mousemove|click",
        "axisPointer": {
            "type": "line"
        },
        "showContent": true,
        "alwaysShowContent": false,
        "showDelay": 0,
        "hideDelay": 100,
        "enterable": false,
        "confine": false,
        "appendToBody": false,
        "transitionDuration": 0.4,
        "formatter": function(params) {                var variables = [                    'Carla-PSNR', 'Carla-LPIPS', 'Carla-SSIM', 'Carla-PCC',                    'Nuscenes-PSNR', 'Nuscenes-LPIPS', 'Nuscenes-SSIM', 'Nuscenes-PCC'                ];                var rawData = [                    [13.04, 0.708, 0.448, 0.180, 17.31, 0.661, 0.442, 0.027],                    [14.67, 0.565, 0.412, 0.554, 21.33, 0.376, 0.607, 0.077],                    [15.32, 0.507, 0.457, 0.566, 21.87, 0.342, 0.621, 0.201],                    [16.27, 0.503, 0.508, 0.581, 23.19, 0.339, 0.675, 0.431],                    [15.38, 0.541, 0.445, 0.696, 23.84, 0.280, 0.739, 0.437],                    [15.54, 0.558, 0.462, 0.551, 24.11, 0.242, 0.734, 0.806],                    [17.33, 0.485, 0.620, 0.765, 20.74, 0.650, 0.560, 0.570],                    [18.40, 0.359, 0.622, 0.817, 24.97, 0.231, 0.750, 0.887]                ];                var modelIndex = ['SplatterImage', 'PixelSplat', 'MVSplat', 'DepthSplat', 'DrivingForward', 'Omni-Scene', '6img-to-3d', 'Ours'].indexOf(params.seriesName);                var html = '<div style="padding: 15px; background: rgba(30,30,30,0.95); border-radius: 6px; box-shadow: 0 4px 12px rgba(0,0,0,0.2); border: 1px solid #555;">';                html += '<div style="font-weight: normal; margin-bottom: 8px; font-size: 18px; color: ' + params.color + '">' + params.seriesName + '</div>';                html += '<div style="font-size: 16px; line-height: 1.8; color: #ddd;">';                for (var i = 0; i < variables.length; i++) {                    html += '<div><strong>' + variables[i] + ':</strong> ' + rawData[modelIndex][i].toFixed(3) + '</div>';                }                html += '</div></div>';                return html;            },
        "textStyle": {
            "fontSize": 20,
            "fontWeight": "normal"
        },
        "borderWidth": 0,
        "padding": 5,
        "order": "seriesAsc"
    },
    "radar": [
        {
            "indicator": [
                {
                    "name": "Carla-PSNR",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Carla-LPIPS",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Carla-SSIM",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Carla-PCC",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Nuscenes-PSNR",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Nuscenes-LPIPS",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Nuscenes-SSIM",
                    "max": 1,
                    "min": 0
                },
                {
                    "name": "Nuscenes-PCC",
                    "max": 1,
                    "min": 0
                }
            ],
            "shape": "circle",
            "center": [
                "36%",
                "50%"
            ],
            "radius": "54%",
            "startAngle": 90,
            "name": {
                "textStyle": {
                    "color": "#fff",
                    "fontSize": 20,
                    "fontWeight": "normal"
                }
            },
            "splitLine": {
                "show": false,
                "lineStyle": {
                    "show": true,
                    "width": 1,
                    "opacity": 1,
                    "curveness": 0,
                    "type": "solid",
                    "color": "#666"
                }
            },
            "splitArea": {
                "show": true,
                "areaStyle": {
                    "opacity": 0.1,
                    "color": "#fff"
                }
            },
            "axisLine": {
                "show": true,
                "onZero": true,
                "onZeroAxisIndex": 0,
                "lineStyle": {
                    "show": true,
                    "width": 1,
                    "opacity": 1,
                    "curveness": 0,
                    "type": "solid",
                    "color": "#666"
                }
            }
        }
    ],
    "title": [
        {
            "show": true,
            "text": "Model Performance Comparison",
            "target": "blank",
            "subtext": "Normalized Metrics Across Datasets",
            "subtarget": "blank",
            "left": "center",
            "padding": 5,
            "itemGap": 10,
            "textAlign": "auto",
            "textVerticalAlign": "auto",
            "triggerEvent": false,
            "textStyle": {
                "color": "#eee",
                "fontWeight": "bold",
                "fontSize": 32
            },
            "subtextStyle": {
                "color": "#aaa",
                "fontSize": 24
            }
        }
    ]
  };
  
  return option;
}

onMounted(() => {
  if (chartContainer.value) {
    const chart = echarts.init(chartContainer.value, 'dark', { renderer: 'svg' });
    const radarOption = generateRadarOption();
    chart.setOption(radarOption);
  }
});


</script>

<template>
    <div>
        <el-divider />

        <el-row justify="center">
            <h1 class="section-title">Quantitative Results</h1>
        </el-row>
        
        <!-- 数据表格 -->
        <el-row justify="center">
            <el-col :span="18">
                <p>
                    The table presents the quantitative comparison between our model and several baselines. Notably, our model demonstrates superior performance on two datasets with different focuses, whereas most existing methods excel at only one type of task.
                </p>

                <!-- 卡片 -->
                <el-card class="card">
                    <p class="table-caption">
                        Quantitative comparison of our model against the baselines. 
                        The <span class="highlight best">best</span>, 
                        <span class="highlight second-best">second-best</span>, and 
                        <span class="highlight third-best">third-best</span> results are marked with colors.
                    </p>

                    <el-tabs class="demo-tabs" model-value="carla">

                        <!-- Nuscenes Tab -->
                    <el-tab-pane label="Nuscenes" name="nuscenes">
                        <el-table 
                            :data="nuscenesData"
                            :default-sort="{ prop: 'psnr', order: 'descending' }"
                            scrollbar-always-on
                            stripe
                        >
                            <el-table-column prop="model" label="Model" width="120" fixed sortable/>
                            <el-table-column prop="psnr" label="PSNR ↑" min-width="100" sortable :sort-method="sortPSNR">
                                <template #default="scope">
                                    <span :class="scope.row.psnrHighlight">{{ scope.row.psnr.toFixed(2) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="lpips" label="LPIPS ↓" min-width="100" sortable :sort-method="sortLPIPS">
                                <template #default="scope">
                                    <span :class="scope.row.lpipsHighlight">{{ scope.row.lpips.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="ssim" label="SSIM ↑" min-width="100" sortable :sort-method="sortSSIM">
                                <template #default="scope">
                                    <span :class="scope.row.ssimHighlight">{{ scope.row.ssim.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="pcc" label="PCC ↑" min-width="100" sortable :sort-method="sortPCC">
                                <template #default="scope">
                                    <span :class="scope.row.pccHighlight">{{ scope.row.pcc.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>

                    <!-- Carla-Centric Tab -->
                    <el-tab-pane label="Carla-Centric" name="carla">
                        <el-table 
                            :data="carlaData"
                            :default-sort="{ prop: 'psnr', order: 'descending' }"
                            scrollbar-always-on
                            stripe
                        >
                            <el-table-column prop="model" label="Model" width="120" fixed sortable/>
                            <el-table-column prop="psnr" label="PSNR ↑" min-width="100" sortable :sort-method="sortPSNR">
                                <template #default="scope">
                                    <span :class="scope.row.psnrHighlight">{{ scope.row.psnr.toFixed(2) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="lpips" label="LPIPS ↓" min-width="100" sortable :sort-method="sortLPIPS">
                                <template #default="scope">
                                    <span :class="scope.row.lpipsHighlight">{{ scope.row.lpips.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="ssim" label="SSIM ↑" min-width="100" sortable :sort-method="sortSSIM">
                                <template #default="scope">
                                    <span :class="scope.row.ssimHighlight">{{ scope.row.ssim.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                            <el-table-column prop="pcc" label="PCC ↑" min-width="100" sortable :sort-method="sortPCC">
                                <template #default="scope">
                                    <span :class="scope.row.pccHighlight">{{ scope.row.pcc.toFixed(3) }}</span>
                                </template>
                            </el-table-column>
                        </el-table>
                    </el-tab-pane>

                    

                    </el-tabs>

                </el-card>

                <p>
                    The radar chart illustrates the strong unification capability of our model, where a larger area signifies superior comprehensive performance. It demonstrates that our model achieves excellent overall results on two reconstruction tasks under different settings.
                </p>

                <el-card class="card">
                    <div ref="chartContainer" style="width: 100%; height: 600px;"></div>
                </el-card>
            </el-col>
        </el-row>

    </div>
</template>

<style scoped>
/* * {
    font-weight: bold;
} */
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
  
.card {
    margin-top: 20px;
}

.table-caption {
    text-align: center;
    margin-bottom: 20px;
    font-size: 14px;
    color: #666;
}

.highlight {
    font-weight: bold;
}

.best {
    font-weight: bold;
    color: #ff4444;
    background-color: #ffeeee;
    padding: 2px 6px;
    border-radius: 3px;
}

.second-best {
    font-weight: bold;
    color: #ff8800;
    background-color: #fff4e6;
    padding: 2px 6px;
    border-radius: 3px;
}

.third-best {
    font-weight: bold;
    color: #ffaa00;
    background-color: #fffbe6;
    padding: 2px 6px;
    border-radius: 3px;
}

:deep(.el-table) {
    font-size: 13px;
}

:deep(.el-table__header) {
    font-weight: bold;
}
</style>
  