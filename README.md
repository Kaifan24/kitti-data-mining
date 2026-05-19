\# KITTI 驾驶场景数据分析与长尾 Corner Case 挖掘



基于KITTI目标检测数据集(7481帧, 39597个标注目标),构建自动化数据分析与难例挖掘pipeline。



\## 核心发现



\- 类别严重不均衡: Car占72.6%, Person\_sitting仅0.6%, 相差120倍

\- 小目标占23.3%, 是模型最易漏检的类型

\- 设计6维难度评分体系, 自动挖掘出1062帧(14.2%)极难场景

\- 最大难度来源: 小目标(影响64.1%的帧) > 严重遮挡(50.2%) > 远距离(42.0%)



\## 难度评分维度



| 维度 | 权重 | 影响帧数 | 占比 |

|------|------|---------|------|

| 小目标(<32px) | 2 | 4798 | 64.1% |

| 严重遮挡(>=2) | 2 | 3758 | 50.2% |

| 远距离(>50m) | 2 | 3144 | 42.0% |

| 严重截断(>0.5) | 1 | 2423 | 32.4% |

| 稀有类别 | 3 | 1434 | 19.2% |

| 密集场景(>=15) | - | 1002 | 13.4% |



\## 项目结构



\- src/01\_data\_overview.py - 数据集总览分析(类别/大小/遮挡/距离分布)

\- src/02\_hard\_case\_mining.py - 长尾难例自动挖掘与评分



\## 环境



\- 镜像: nvcr.io/nvidia/pytorch:24.10-py3

\- 数据: KITTI 2D Object Detection (label\_2)



\## 复现



docker run --gpus all -it --rm -v <项目路径>:/workspace --shm-size=8g nvcr.io/nvidia/pytorch:24.10-py3

cd /workspace \&\& python src/01\_data\_overview.py \&\& python src/02\_hard\_case\_mining.py

