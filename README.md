# CTSG
This repository is the official implementation of the paper: CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation <!--[🔗](http://)-->

![屏幕截图 2024-09-26 120312](https://github.com/user-attachments/assets/54d4b915-feff-4cb0-919d-7e9cbd406640)

> https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda

> https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35
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
#### Instance Segmentation Model Selection

We evaluate our instance segmentation on the Replica dataset, following the same evaluation protocol as described in Conceptgraph. The segmentation performance of current models is compared using two metrics: Mean Accuracy (mAcc) and Frequency-weighted Intersection over Union (F-mIoU).

| Models      | Type      | mAcc  | F-mIOU |
|:-----------:|:---------:|:-----:|:------:|
| CLIPSeg     |           | 28.21 | 39.84  |
| LSeg        | Fine Tune | 33.39 | 51.54  |
| OpenSeg     |           | 41.19 | 53.74  |
| MaskCLIP    |           | 4.53  | 0.94   |
| Mask2former | Zero Shot | 10.42 | 13.11  |
| RAM + SAM   |           | 34.19 | 37.27  |
| RAM + SAM2  |           | 35.80 | 38.35  |



> we report various semantic segmentation results on Replica dataset. The combination of the RAM and SAM series demonstrates a significant advantage over MaskCLIP and Mask2former. Although its performance metrics are slightly lower than those of fine-tuned models, the open-vocabulary segmentation model demonstrates a clear advantage in scene transferability. There is no significant performance difference between the SAM and SAM2 models, but SAM is more widely-used and adaptable to other models. Therefore, we selected the RAM+SAM combination for object segmentation in scenes.


   #### Graph Location
   
   ##### HM3D
   | Scenes | GON/SON | Accuracy(%) | Position Error(m) |
   |:--------:|:---------:|:-------------:|:-------------------:|
   |BAbdmeyTvMZ | 63/370 | 57.14 | 0.2682 |
   |Dd4bFSTQ8gi | 344 / 392 | 53.49 |  0.2319 |
   |QaLdnwvtxbs& | 238 / 238| 42.72 | 0.2964 |
   |svBbv1Pavdk | 286 / 372 | 47.20 | 0.4720 |
   | mean        |           | 50.14 | 0.3171|
   
   #### Matterport3D
   | Scenes | GON/SON | Accuracy(%) | Position Error(m) |
   |:--------:|:---------:|:-------------:|:-------------------:|
   |2azQ1b91cZZ | 695 / 862 | 44.75 | 0.3096 |
   |EU6Fwq7SyZv | 295/438 | 52.63 | 0.3228 |
   |oLBMNvg9in8 | 483/770 | 52.59 | 0.2554 |
   |TbHJrupSAjP | 421/641 | 60.57 | 0.2833 |
   |x8F5xyUWy9e | 93/276 | 41.92 | 0.2725 |
   |Z6MFQCViBuw | 152/562 | 58.55 | 0.3184 | 
   | mean        |           | 52.91 | 0.2956|
    
   
   
> The accuracy of the constructed scene graph is evaluated on the Matterport3D and HM3D datasets. GON represents the number of predicted objects in the scene graph, SON represents the number of objects in the real scene, and Accuracy indicates the percentage of correctly predicted objects in the scene graph. The evaluation details are as follows: the object categories and object coordinates in the scene graph are matched with the ground truth categories and coordinates. An object prediction in the scene graph is considered correct only if the category labels match and the distance between the object coordinates in the scene graph and the ground truth coordinates is less than a threshold (1 meter). Position Error represents the average distance error between the correctly predicted objects in the scene graph and the ground truth objects.
   
> To further demonstrate the accuracy of our proposed CSTG in constructing scene graphs, we include a comparative experiment with ConceptGraph. The experimental results are presented in the table.





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
 <!--
   conway整体方法（包括构图、检索）有效性

   ​	1. with/without：在原本的基础上去掉conway检索=只对所有object列表做clip+vlm

   > 2. room cluster：在原本的基础上把conway换成room（聚类得到，有图文），其他不变
   -->
   
 | Scene        | Navigation |          |             |     |       |          |
|:------------:|:----------:|:--------:|:-----------:|:---:|:-----:|:--------:|
|              | IEVE       |          | Object Only |     | Ours  |          |
|              | sr(%)      | spl      | sr(%)       | spl | sr(%) | spl      |
| Dd4bFSTQ8gi  | 33.6       | 0.0753   | 24.00       |     | 41.33 |  0.6029  |
| QaLdnwvtxbs  | 66.4       |  0.2681  | 48.15       |     | 44.44 |  0.7471  |
| VBzV5z6i1WS  | 44.8       | 0.1517   | 34.38       |     | 48.96 |  0.6090  |
| ziup5kvtCCR  | 45.6       |  0.1332  | 43.59       |     | 41.03 |  0.5979  |
| Nfvxx8J5NCo  | 35.2       |  0.1236  | 25.64       |     | 47.37 |  0.6264  |
| svBbv1Pavdk  | 57.6       |  0.2126  | 17.54       |     | 42.42 |  0.6214  |
| BAbdmeyTvMZ  | 60.8       |  0.3492  | 39.40       |     | 42.70 | 0.6452   |
| mv2HUxq3B53  | 37.6       |  0.1208  | 40.00       |     | 22.67 |  0.6423  |
| Mean         | 47.9       |  0.1823  | 34.09       |     | 40.19 |  0.6365  |



   <!--实验设置：首先，我们构建了一个基线模型，该模型直接在场景中所有检测到的物体实例列表中搜索目标实例, 找到目标实例后，利用导航图规划路径完成任务。然而，这种扁平化的检索方法仅关注物体本身而忽略了其周围环境的上下文信息时，它只能在较远的距离上才能准确地识别出目标物体，导致了导航成功率的下降。此外，我们将自己的导航结果与公开可用的最先进方法IEVE在我们的评估数据集上进行了比较，尽管IEVE算法在导航性能上略胜一筹，但其依赖于训练过程以优化模型参数。相比之下，我们的方法采用无需训练的策略，旨在通过直接应用模型于新场景来评估其泛化能力，从而在实际应用中提供更灵活的解决方案。-->
> Initially, we constructed a baseline model that directly searches for the target instance within the list of all detected object instances in the scene. Once the target instance is identified, a navigation graph is utilized to plan a path and complete the task. However, this flattened retrieval method, which focuses solely on the object itself and neglects the contextual information of its surrounding environment, can only accurately recognize the target object at a greater distance, leading to a decrease in navigation success rates.
>
>  Furthermore, we compared our navigation results with the publicly available state-of-the-art method, IEVE, on our evaluation dataset. Although the IEVE algorithm slightly outperforms ours in navigation performance, it relies on a training process to optimize model parameters. In contrast, our method employs a training-free strategy, aiming to assess its generalization capability by directly applying the model to new scenes, thereby providing a more flexible solution in practical applications.

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







