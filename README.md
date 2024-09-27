# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation <!--[🔗](http://)-->

<<<<<<< HEAD
![屏幕截图 2024-09-26 221652](https://github.com/user-attachments/assets/fbad25d8-99fc-4d05-a02e-0d933f31120c)
=======
<img src=".\img\intruduction.png" alt="github-pic"  />
>>>>>>> graph_location

> https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda

> https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35
>
<!--
## Abstract
For visual target navigation tasks, a suitable environment representation plays a vital part in robot system to complete the navigation tasks. 3D Scene Graphs serve as sparse representations that excel at representing environments efficiently compare to dense semantic maps. Typical Scene Graphs are generally constructed based on multi-level semantic labels with hierarchical structures, which may lead to the loss of valuable spatial information, such as spatial topological relations, common sense understanding and visual context information. In this work, we propose **CTSG**, a Hierarchical 3D scene graph mapping approach for visual object navigation. Our graph adopts a dual-layer structure. Besides object layer, a novel conway (short for context information and way topology) layer is introduced, which consists of topological waypoints with rich multi-modal context information. We demonstrate the effectiveness of our method in novel visual target navigation tasks through simulation and real-world experiments across varied environments and instructions.

## Video
<video controls>
  <source src="./CSTG_Final.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Approach

<img src="./img/pipeline.png" />

> 描述-优势

### Scene Graph Construction

<img src=".\img\conwaygraph.png" />

> 描述---换成gif

<img src="./img/semantic.png"/>

> 描述

### Visual Target Navigation with Scene Graph

> **[M]**整体架构图+retrieval details描述
-->

## Experiment

### Instance Segmentation Model Selection

We evaluate our instance segmentation on the Replica dataset, following the same evaluation protocol as described in Conceptgraph. The segmentation performance of current models is compared using two metrics: Mean Accuracy (mAcc) and Frequency-weighted Intersection over Union (F-mIoU).

> Segmentation Model Selection 表

we report various semantic segmentation results on Replica dataset. The combination of the RAM and SAM series demonstrates a significant advantage over MaskCLIP and Mask2former. Although its performance metrics are slightly lower than those of fine-tuned models, the open-vocabulary segmentation model demonstrates a clear advantage in scene transferability. There is no significant performance difference between the SAM and SAM2 models, but SAM is more widely-used and adaptable to other models. Therefore, we selected the RAM+SAM combination for object segmentation in scenes.


### 1. Graph


obj location acc: 对比ConceptGraph，hovsg?在gt环境下评测
[parameter_list](#section-heading)
<!--
### 1. Dataset

1. scene data

   HM3D

   （Matterport 3D）

2. task data

   HM3D
-->
### 1. Scene Graph 
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





#### HM3D+Matterport3D评测表格

> HM3D and  Matterport3D 评测表格
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
 


The accuracy of the constructed scene graph is evaluated on the Matterport3D and HM3D datasets. GON represents the number of predicted objects in the scene graph, SON represents the number of objects in the real scene, and Accuracy indicates the percentage of correctly predicted objects in the scene graph. The evaluation details are as follows: the object categories and object coordinates in the scene graph are matched with the ground truth categories and coordinates. An object prediction in the scene graph is considered correct only if the category labels match and the distance between the object coordinates in the scene graph and the ground truth coordinates is less than a threshold (1 meter). Position Error represents the average distance error between the correctly predicted objects in the scene graph and the ground truth objects.

To further demonstrate the accuracy of our proposed CSTG in constructing scene graphs, we include a comparative experiment with ConceptGraph. The experimental results are presented in the table.

> conceptgraph 和 ours 在HM3D数据集上测评表



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

### 2. Visual Target Navigation

1. Navigation Evaluation
 <!--
   conway整体方法（包括构图、检索）有效性

   ​	1. with/without：在原本的基础上去掉conway检索=只对所有object列表做clip+vlm

   > 2. room cluster：在原本的基础上把conway换成room（聚类得到，有图文），其他不变
   -->

|    Scene     | IEVE       |          | Object Only |     | Ours  |          |
|:------------:|:----------:|:--------:|:-----------:|:---:|:-----:|:--------:|
|              | sr(%)      | spl      | sr(%)       | spl | sr(%) | spl      |
| Dd4bFSTQ8gi  | 33.6       | 0.0753   | 24.00       | 0.7360    | 41.33 |  0.6029  |
| QaLdnwvtxbs  | 66.4       |  0.2681  | 48.15       |   0.5256  | 44.44 |  0.7471  |
| VBzV5z6i1WS  | 44.8       | 0.1517   | 34.38       |   0.6562  | 48.96 |  0.6090  |
| ziup5kvtCCR  | 45.6       |  0.1332  | 43.59       |  0.5086   | 41.03 |  0.5979  |
| Nfvxx8J5NCo  | 35.2       |  0.1236  | 25.64       |  0.6327   | 47.37 |  0.6264  |
| svBbv1Pavdk  | 57.6       |  0.2126  | 17.54       |  0.6283   | 42.42 |  0.6214  |
| BAbdmeyTvMZ  | 60.8       |  0.3492  | 39.40       |   0.7001  | 42.70 | 0.6452   |
| mv2HUxq3B53  | 37.6       |  0.1208  | 40.00       |  0.6248   | 22.67 |  0.6423  |
| Mean         | 47.9       |  0.1823  | 34.09       |    0.6265  | 40.19 |  0.6365  |



   <!--实验设置：首先，我们构建了一个基线模型，该模型直接在场景中所有检测到的物体实例列表中搜索目标实例, 找到目标实例后，利用导航图规划路径完成任务。然而，这种扁平化的检索方法仅关注物体本身而忽略了其周围环境的上下文信息时，它只能在较远的距离上才能准确地识别出目标物体，导致了导航成功率的下降。此外，我们将自己的导航结果与公开可用的最先进方法IEVE在我们的评估数据集上进行了比较，尽管IEVE算法在导航性能上略胜一筹，但其依赖于训练过程以优化模型参数。相比之下，我们的方法采用无需训练的策略，旨在通过直接应用模型于新场景来评估其泛化能力，从而在实际应用中提供更灵活的解决方案。-->
> Initially, we constructed a baseline model that directly searches for the target instance within the list of all detected object instances in the scene. Once the target instance is identified, a navigation graph is utilized to plan a path and complete the task. However, this flattened retrieval method, which focuses solely on the object itself and neglects the contextual information of its surrounding environment, can only accurately recognize the target object at a greater distance, leading to a decrease in navigation success rates.
>
>  Furthermore, we compared our navigation results with the publicly available state-of-the-art method, IEVE, on our evaluation dataset. Although the IEVE algorithm slightly outperforms ours in navigation performance, it relies on a training process to optimize model parameters. In contrast, our method employs a training-free strategy, aiming to assess its generalization capability by directly applying the model to new scenes, thereby providing a more flexible solution in practical applications.

### 3. Retrieval Evaluation
   1. Top K Accuracy
   <!--
   为了找到最好的K
   去掉VLM不确定因素，探讨clip结果的precision
   选出最合适的K
   或者
   加上vlm，评测两个内容：1. clip选择的结果，2. VLM对不同K的表现 
   -->

   |            | top1  | top3  | top5  | top7  |
   |:----------:|:-----:|:-----:|:-----:|:-----:|
   | conway acc | 49.09 | 47.69 | 46.93 | 44.64 |
   | object acc | 27.03 | 29.00 | 31.21 | 30.01 |

   <!-- 实验设置：本研究设计了一种模型，该模型排除了大型视觉语言模型（LVAM）中的不确定性因素，所有节点的选择均基于CLIP相似度进行决策。在构建模型时，我们移除了与所有LVAM相关的模块。在Conway节点的选择上，我们采用了一种策略：将所有候选Conway节点下挂载的物体节点的CLIP特征与目标物体图片的CLIP特征进行相似度匹配，选择最相似的物体节点，将其连接的Conway节点作为导航点以规划路径。实验结果如上表所示。
   分析结果表明，当k值设定为3或5时，可以较好地平衡Conway和物体的检索正确率。然而，当k值增至5时，虽然物体准确率的提升有限，但导航准确率却有所下降。此外，考虑到我们的方法会调用LVLM，k值的增大会导致输入到视觉语言模型的token数量增加67%。综合考虑，我们选择k值为3作为最优解-->

> We designed a model that eliminates the uncertainties inherent in Large Visual and Language Models (LVAMs), with all node selections based on CLIP similarity decisions. When constructing the model, we removed all modules related to LVAMs. For the selection of Conway nodes, we adopted a strategy where we matched the CLIP features of all candidate Conway nodes' attached object nodes with the CLIP features of the target object image to find the most similar object node. The Conway node connected to this object node was then used as the navigation point to plan the path. The experimental results are shown in the table above.
>
> Analysis of the results indicates that setting the k value to 3 or 5 can achieve a good balance between the retrieval accuracy of Conway and object nodes. However, when the k value is increased to 5, although the improvement in object accuracy is limited, the navigation accuracy decreases. Moreover, considering that our method invokes LVLMs, an increase in the k value leads to a 67% increase in the number of tokens input to the visual language model. Taking all factors into account, we have chosen a k value of 3 as the optimal solution.

   2. The effectiveness of multimodal graphs and Conway layers
   
   | Method        | Modal       | Top1  | Top3  | Top5  |
   |:-------------:|:-----------:|:-----:|:-----:|:-----:|
   |               | Multi-Modal | 7.53  | 14.93 | 19.07 |
   | Room-Object   | Image Only  | 4.26  | 14.28 | 17.29 |
   |               | Text Only   | 4.35  | 13    | 15.38 |
   |               | Multi-Modal |       |       |       |
   | Conway-Object | Image Only  |       |       |       |
   |               | Text Only   |       |       |       |

<!-- 单模态多模态 1. 去掉vlm的我们的方法，纯评估graph： clip conway top 3 -> expand to 9 -> clip obj top 1 2.去掉文本模态：clip conway top 3 -> expand to 9 -> clip obj（only image） top 1 3. 去掉图像模态： clip conway top 3 -> expand to 9 -> clip obj（only text） top 1 5. conway 换成gt room clip room label top 3 -> clip obj top 1 6. 去掉图像模态（论文baseline）clip room label top 3 -> clip obj（only text） top 1 -->
<!--实验设置：我们对room-object和conway-object两种不同的形式的graph, 在相同实验设置下做对比，实验结果如上表。首先，我们可以观察到，不论是room-object形式还是conway-object形式的场景graph，增加节点上的模态信息可以提升物体检索的准确率。其次，在相同实验配置下，conway-object形式的graph比room-object形式的graph检索的结果要好的多。这个实验充分证明了我们提出的多模态conway-obj场景图的优势。-->

> We compared the performance of two different graph structures—the room-object and conway-object graphs—under uniform experimental conditions. The results of the experiment are detailed in the aforementioned table. Preliminary observations indicate that regardless of whether the room-object or conway-object graph form is utilized, augmenting the nodes with multimodal information significantly enhances the accuracy of object retrieval.
>
> Further analysis revealed that under identical experimental configurations, the conway-object graph outperformed the room-object graph in retrieval performance. This finding fully confirms the significant advantages of the multimodal conway-object scene graph we proposed in retrieval tasks.
         
<!--### 4. 多任务task-->
      > 或者
      >
      > 加上vlm，评测两个内容：1. clip选择的结果，2. VLM对不同K的表现

   2. conway  & room？多模态or单模态？
      
      单模态多模态

      1. 去掉vlm的我们的方法，纯评估graph：

         clip conway top 3 --> expand to 9 --> clip obj top 1、

         | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
         |:------------:|:---------:|:---------:|:---------:|
         | BAbdmeyTvMZ | 15.15     | 24.24     | 33.33     |
         | Dd4bFSTQ8gi  | 33.33     | 58.67     | 68        |
         | mv2HUxq3B53  | 17.33     | 32        | 33.33     |
         | Nfvxx8J5NCo  | 15.38     | 35.90     | 46.15     |
         | QaLdnwvtxbs  | 55.56     | 70.37     | 74.07     |
         | svBbv1Pavdk  | 28.07     | 45.61     | 63.16     |
         | VBzV5z6i1WS  | 31.25     | 53.13     | 66.67     |
         | ziup5kvtCCR  | 35.90     | 69.23     | 76.92     |
         | mean         | 29        | 48.64     | 57.70     |
         
      2.去掉文本模态：

      | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
      |:------------:|:---------:|:---------:|:---------:|
      | BAbdmeyTvMZ  | 9.09      | 15.15     | 21.21     |
      | Dd4bFSTQ8gi  | 29.33     | 52        | 58.67     |
      | mv2HUxq3B53  | 10.67     | 26.67     | 34.67     |
      | Nfvxx8J5NCo  | 15.38     | 25.64     | 30.77     |
      | QaLdnwvtxbs  | 62.96     | 77.78     | 81.48     |
      | svBbv1Pavdk  | 14.04     | 29.82     | 52.63     |
      | VBzV5z6i1WS  | 30.21     | 48.96     | 54.17     |
      | ziup5kvtCCR  | 43.59     | 61.54     | 71.79     |
      | mean         | 26.90     | 42.20     | 50.67     |
         

         clip conway top 3 --> expand to 9 --> clip obj（only image） top 1

      3. 去掉图像模态：

      | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
      |:------------:|:---------:|:---------:|:---------:|
      | BAbdmeyTvMZ  | 21.21     | 39.39     | 42.42     |
      | Dd4bFSTQ8gi  | 28        | 60        | 72        |
      | mv2HUxq3B53  | 25.33     | 38.67     | 48        |
      | Nfvxx8J5NCo  | 33.33     | 51.28     | 56.41     |
      | QaLdnwvtxbs  | 44.44     | 74.07     | 74.07     |
      | svBbv1Pavdk  | 28.07     | 52.63     | 70.18     |
      | VBzV5z6i1WS  | 21.88     | 52.08     | 61.46     |
      | ziup5kvtCCR  | 58.97     | 74.36     | 82.05     |
      | mean         | 32.65     | 55.31     | 63.32     |
         

         clip conway top 3 --> expand to 9 --> clip obj（only text） top 1
      
      4. conway 换成gt room

         clip room label top 3 --> clip obj top 1

         | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
         |:------------:|:---------:|:---------:|:---------:|
         | BAbdmeyTvMZ  | 21.21     | 33.33     | 39.40     |
         | Dd4bFSTQ8gi  | 20        | 34.67     | 46.67     |
         | mv2HUxq3B53  | 36        | 56        | 72        |
         | Nfvxx8J5NCo  | 25.61     | 51.28     | 66.67     |
         | QaLdnwvtxbs  | 59.26     | 92.59     | 92.59     |
         | svBbv1Pavdk  | 22.81     | 28.07     | 33.33     |
         | VBzV5z6i1WS  | 22.91     | 46.88     | 56.25     |
         | ziup5kvtCCR  | 38.46     | 53.85     | 61.54     |
         | mean         | 30.78     | 49.58     | 58.56     |


      5. 去掉文本模态（论文baseline）

         | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
         |:------------:|:---------:|:---------:|:---------:|
         | BAbdmeyTvMZ  | 18.18     | 24.24     | 39.39     |
         | Dd4bFSTQ8gi  | 13.33     | 36        | 46.67     |
         | mv2HUxq3B53  | 13.33     | 32        | 40        |
         | Nfvxx8J5NCo  | 25.64     | 48.72     | 48.72     |
         | QaLdnwvtxbs  | 62.96     | 81.48     | 91.3      |
         | svBbv1Pavdk  | 15.79     | 22.81     | 31.58     |
         | VBzV5z6i1WS  | 19.79     | 36.46     | 52.08     |
         | ziup5kvtCCR  | 23.08     | 48.72     | 56.41     |
         | mean         | 24.01     | 41.30     | 50.77     |

      6. 去掉图像模态（论文baseline）

         | scanes name  | acc(TOP1) | acc(TOP3) | acc(TOP5) |
         |:------------:|:---------:|:---------:|:---------:|
         | BAbdmeyTvMZ  | 15.15     | 33.33     | 45.45     |
         | Dd4bFSTQ8gi  | 20        | 44        | 54.67     |
         | mv2HUxq3B53  | 26.67     | 56        | 60        |
         | Nfvxx8J5NCo  | 33.33     | 64.10     | 74.36     |
         | QaLdnwvtxbs  | 40.74     | 88.89     | 96.30     |
         | svBbv1Pavdk  | 28.07     | 36.84     | 45.61     |
         | VBzV5z6i1WS  | 32.29     | 57.29     | 64.58     |
         | ziup5kvtCCR  | 30.77     | 48.72     | 71.79     |
         | mean         | 28.38     | 53.65     | 64.1      |


3. 多任务task

      


## 参数表<a name="section-heading"></a>

| Subsystem | Hyperparameters | Value |
| --------- | --------------- | ----- |
|     _      |                |       |
|    _       |                 |       |
|       _    |                 |       |







