# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation[🔗](http://)

<img src="/img/intruduction.png" alt="github-pic"  />

> - [ ] **[L]** CTSG is a hierarchical 3D scene graph mapping method designed for visual object navigation. It features a dual-layer structure: an object layer and a novel *Conway* layer (short for *context* and *way* topology), composed of topological waypoints enriched with multi-modal context information.
>
> https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda
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
   
   | scene / SR  |     IEVE  | Object Only | Ours    |
   |:-------------:|:-------------:|:-------------:|:---------:|
   | Dd4bFSTQ8gi | 33.6      | 24.00       | 41.33   |
   | QaLdnwvtxbs | 66.4       | 48.15       | 44.44   |
   | VBzV5z6i1WS | 44.8       | 34.38       | 48.96   |
   | ziup5kvtCCR | 45.6       | 43.59       | 33.33   |
   | Nfvxx8J5NCo | 35.2       | 25.64       | 41.03   |
   | svBbv1Pavdk | 57.6       | 17.54       | 47.37   |
   | BAbdmeyTvMZ | 60.8       | 39.40       | 42.42   |
   | mv2HUxq3B53 | 37.6       | 40.00       | 22.67   |
   | mean       | 47.9       | 34.09      | 40.19   |


   <!--实验设置：首先，我们构建了一个基线模型，该模型直接在场景中所有检测到的物体实例列表中搜索目标实例, 找到目标实例后，利用导航图规划路径完成任务。然而，这种扁平化的检索方法仅关注物体本身而忽略了其周围环境的上下文信息时，它只能在较远的距离上才能准确地识别出目标物体，导致了导航成功率的下降。此外，我们将自己的导航结果与公开可用的最先进方法IEVE在我们的评估数据集上进行了比较，尽管IEVE算法在导航性能上略胜一筹，但其依赖于训练过程以优化模型参数。相比之下，我们的方法采用无需训练的策略，旨在通过直接应用模型于新场景来评估其泛化能力，从而在实际应用中提供更灵活的解决方案。-->
   Initially, we constructed a baseline model that directly searches for the target instance within the list of all detected object instances in the scene. Once the target instance is identified, a navigation graph is utilized to plan a path and complete the task. However, this flattened retrieval method, which focuses solely on the object itself and neglects the contextual information of its surrounding environment, can only accurately recognize the target object at a greater distance, leading to a decrease in navigation success rates. Furthermore, we compared our navigation results with the publicly available state-of-the-art method, IEVE, on our evaluation dataset. Although the IEVE algorithm slightly outperforms ours in navigation performance, it relies on a training process to optimize model parameters. In contrast, our method employs a training-free strategy, aiming to assess its generalization capability by directly applying the model to new scenes, thereby providing a more flexible solution in practical applications.

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



https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35



### real world






