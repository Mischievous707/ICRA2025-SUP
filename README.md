# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation[🔗](http://)

<img src="/img/intruduction.png" alt="github-pic"  />

> - [ ] **[L]** CTSG is a hierarchical 3D scene graph mapping method designed for visual object navigation. It features a dual-layer structure: an object layer and a novel *Conway* layer (short for *context* and *way* topology), composed of topological waypoints enriched with multi-modal context information.
>
> - [ ] **[3]** cases video--现实世界 最好的
>
<!--
## Abstract
For visual target navigation tasks, a suitable environment representation plays a vital part in robot system to complete the navigation tasks. 3D Scene Graphs serve as sparse representations that excel at representing environments efficiently compare to dense semantic maps. Typical Scene Graphs are generally constructed based on multi-level semantic labels with hierarchical structures, which may lead to the loss of valuable spatial information, such as spatial topological relations, common sense understanding and visual context information. In this work, we propose **CTSG**, a Hierarchical 3D scene graph mapping approach for visual object navigation. Our graph adopts a dual-layer structure. Besides object layer, a novel conway (short for context information and way topology) layer is introduced, which consists of topological waypoints with rich multi-modal context information. We demonstrate the effectiveness of our method in novel visual target navigation tasks through simulation and real-world experiments across varied environments and instructions.

## Video

https://github.com/user-attachments/assets/637966af-dfbb-458e-bb6a-15ddcbc1a3d8


## Approach

<img src="/img/pipeline.png" />

> 描述-优势

### Scene Graph Construction

<img src="/img/conwaygraph.png" />

> 描述---换成gif

<img src="/img/semantic.png" />

> 描述

### Visual Target Navigation with Scene Graph

> **[M]**整体架构图+retrieval details描述
-->

## Experiment

[parameter_list](#section-heading)

### 1. Dataset

1. scene data

   HM3D

   （Matterport 3D）

2. task data

   HM3D

### 2. Scene Graph 

obj location acc: 对比ConceptGraph，hovsg?在gt环境下评测 

1. gt从habitat-sim如何获取
2. 如何对比（和competetive的xx方法对比）
3. 结果：

| dataset | scene | ConceptGraph | ours |
| ----------- | ----- | ------------ | ---- |
|      _       |       |              |      |
|       _      |       |              |      |

分析：

描述：为什么做，怎么做，结果如何，代表xx

### 3. Visual Target Navigation

1. Navigation Evaluation

   conway整体方法（包括构图、检索）有效性

   ​	1. with/without：在原本的基础上去掉conway检索=只对所有object列表做clip+vlm

   > 2. room cluster：在原本的基础上把conway换成room（聚类得到，有图文），其他不变

2. Retrieval Evaluation

   去掉VLM不确定因素

   1. Top K Accuracy

      为了找到最好的K

      去掉VLM不确定因素，探讨clip结果的precision

      选出最合适的K

      👇

      | layer/Top K acc | K=1  | K=3  | K=5  |
      | --------------- | ---- | ---- | ---- |
      | conway          |      |      |      |
      | object          |      |      |      |

      > 或者
      >
      > 加上vlm，评测两个内容：1. clip选择的结果，2. VLM对不同K的表现

   2. conway  & room？多模态or单模态？在确定topk超参之后进行
      
      单模态多模态

      1. 去掉vlm的我们的方法，纯评估graph：

         clip conway top 3 --> expand to 9 --> clip obj top 1

      2.去掉文本模态：

         clip conway top 3 --> expand to 9 --> clip obj（only image） top 1

      3. 去掉图像模态：

         clip conway top 3 --> expand to 9 --> clip obj（only text） top 1
      
      
      5. conway 换成gt room

         clip room label top 3 --> clip obj top 1

      6. 去掉图像模态（论文baseline）

         clip room label top 3 --> clip obj（only text） top 1
3. 多任务task

      


## 参数表<a name="section-heading"></a>

| Subsystem | Hyperparameters | Value |
| --------- | --------------- | ----- |
|     _      |                 |       |
|    _       |                 |       |
|       _    |                 |       |

## Case Video
### simulator

https://github.com/user-attachments/assets/c5761339-6b3e-4c0f-892f-1bf5a3897af1

https://github.com/user-attachments/assets/7a20ebef-11e3-460c-98e1-4839877f774c


### real world

https://github.com/user-attachments/assets/11c9cdd9-a8a4-4d2f-b04d-0a8ce47c30c2


https://github.com/user-attachments/assets/0d52ced1-fd2a-4baa-8ef4-d3c330a63623


https://github.com/user-attachments/assets/6c75d5e7-ffc9-4126-8a48-53cec5b4c53a


https://github.com/user-attachments/assets/a06cba6f-cd94-4525-8f24-63e4a1705e29


https://github.com/user-attachments/assets/0d572bbb-96c9-4697-a33b-6f80dedbf8e4


https://github.com/user-attachments/assets/d2093569-524f-44cc-b505-ee049024a560


https://github.com/user-attachments/assets/092d2cf4-3ba0-469b-8e83-c5b3536273a0


