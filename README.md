# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: **CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation**. <!--[🔗](http://)-->

![屏幕截图 2024-09-26 221652](https://github.com/user-attachments/assets/fbad25d8-99fc-4d05-a02e-0d933f31120c)

https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda

https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35

<!--
## Abstract
For visual target navigation tasks, a suitable environment representation plays a vital part in robot system to complete the navigation tasks. 3D Scene Graphs serve as sparse representations that excel at representing environments efficiently compare to dense semantic maps. Typical Scene Graphs are generally constructed based on multi-level semantic labels with hierarchical structures, which may lead to the loss of valuable spatial information, such as spatial topological relations, common sense understanding and visual context information. In this work, we propose **CTSG**, a Hierarchical 3D scene graph mapping approach for visual object navigation. Our graph adopts a dual-layer structure. Besides object layer, a novel conway (short for context information and way topology) layer is introduced, which consists of topological waypoints with rich multi-modal context information. We demonstrate the effectiveness of our method in novel visual target navigation tasks through simulation and real-world experiments across varied environments and instructions.
-->

## Video

<div style="text-align: center;">
   
https://github.com/user-attachments/assets/637966af-dfbb-458e-bb6a-15ddcbc1a3d8

</div>

## Approach

<img src="/img/pipeline.png" />

> todo：有需要再补充

**CTSG** is a dual-layer multi-modal scene graph. The conway layer contains navigable waypoints called conway nodes, which provide contextual information and multi-modal attributes as images and descriptions. 
The connections between conway nodes represent their navigability. 
The object layer includes object nodes, with attributes like individual descriptions and object images, and the connections between conway and object nodes reflect spatial relationships.

Based on our CSTG, we develop an efficient dual-level retrieval strategy for visual target navigation, which leverages VLMs to interpret semantic information and enrich representations. 
We conduct conway-level and object-level retrieval, coupled with a backtracking search to ensure robustness.

<!--
### Scene Graph Construction

<img src="/img/conwaygraph.png" />

> todo：模拟器图gif（图已采集多组数据，GLAQ较差但能体现出pose map作用）

Building the topological structure involves a two-step process: identifying free regions and locating conway nodes.
The obstacle region map represents obstacles spatial distribution.
The pose region map is constructed by route points of fully exploring the environment.
The floor region map is obtained by projecting all floor-wise points into the BEV plane.

Based on above maps, we use the pose region map to complete the floor region map, then subtract obstacle elements from the obstacle map to obtain the navigatable floor free region.
Then, we select the largest area as final floor free region map for noise reduction.

To locate conway nodes, we utilize the Zhang-Suen thinning algorithm to extract the skeleton path of the free region, which is better suited for the localization of our sparse conway nodes. 
For conway nodes sparsity, a lower threshold $\tau$ is used to control the neighbouring nodes distance from the skeleton path.
Conway nodes are initially fully connected. 
We set an upper limit $\theta$ on the length of node-to-node distances and filter out edges passing through obstacle areas, ultimately deriving the sparse conway-layer.

<img src="/img/semantic.png" />

The description of conway nodes and object nodes are generated by VLMs.
To obtain the context information of conway nodes, we capture certain amount of perception views from each conway node.
The captured views are then stitched together to form a panorama, as visual contextual information of conway nodes from the surrounding environment. 

We employ a VLM, such as LLaVA-1.5, to generate detailed and comprehensive textual descriptions for each panorama, corresponding to components, locations and attributes.
These textual descriptions of conway nodes, together with the nodes' surrounding panoramas, form the multi-modal attributes of the conway nodes.

### Visual Target Navigation with Scene Graph

> **[M]**整体架构图+retrieval整体思路优势描述

#### Textualization of Visual Query

> todo: 图/case

Visual target navigation utilizes a single image as query.
We first use GPT-4o to obtain textual descriptions of query images, enriching their semantic information.
The target image, along with its contextual description, serves as a query unit for retrieval.

Then a distinguished prompting strategy is adopted based on the hierarchical characteristics of the nodes.
At the Conway level, our focus is on the contextual and positional information of the environment.
At the object level, we emphasize the detailed characteristics of individual objects.

#### Environment Database Construction
For each node in our graph, we use CLIP to encode its textual descriptions and associated visual image, serving as the semantic information stored in the vector databases.

#### Conway-level Retrieval

> todo: 图/case

We encode conway-level query unit using CLIP, compute its feature similarity with all conway-level nodes, and select the top-k candidate conway nodes.
Additionally, we retrieve surrounding conway nodes connected to these top-k candidates and calculate their similarity to the query unit, and select the top-3 secondary viewpoints for each.

We then input both the retrieved conway node information and the query unit into GPT-4o, leveraging its advanced analytical capabilities to select candidate conway node with the highest similarity as the optimal result.
The remaining candidate conway nodes are retained for potential backtracking in case of retrieval failure.

#### Object-level Retrieval

> todo: 图/case

Once the highest-possibility conway node is determined, we retrieve the object nodes associated with the chosen conway node.
The image and detailed object descriptions are used as the object-level query unit for retrieval. 
Similar to conway-level retrieval, the similarity between the CLIP features of the query unit and features of the object nodes is computed for selecting the top-3 most similar object nodes.

Then, we leverage GPT-4o model to analyze the multi-modal semantic information from the retrieved object nodes, referring to the query unit to re-rank the candidate object nodes and provide plausible candidates.

#### Backtracking Mechanism

> todo: 图/case

If the LLM cannot find any plausible candidate object, the retrieval process backtracks and select other conway-level alternatives until the query object is accurately identified.

#### Path Planning on Conway-layer

> todo: 图/case

Once a specific object node is located, the path planning module uses the corresponding conway node information to determine an optimal navigation path within the connected conway node graph using Dijkstra's algorithm.
This path is represented as a sequence of conway nodes coordinates.
Visual target navigation is accomplished by reaching the target in query image following the computed path.

-->

## Experiment & Analysis
我们从几个方面对我们的方法进行了评测与分析：
[parameter_list](#section-heading)
<!--
### 1. Dataset

1. scene data

   HM3D

   （Matterport 3D）

2. task data

   HM3D
-->
### [TODO: 艳平加总起句介绍评测了什么+为什么评测（我们的优势）+实验结果+分析（加上conceptgraph）] 1. Scene Graph Evaluation
？如何合理引入conceptgraph：常见构建方法
本部分中评测了构建的场景图的准确性，包括物体类别及空间位置。我们与（同类型的）conceptgraph进行了比较，
证实了1. node布局合理 2. 构图方法的有效性。

   #### Graph Location
   
   ##### HM3D

   | Scenes | GON/SON | Accuracy(%) | Position Error(m) |
   |:--------:|:---------:|:-------------:|:-------------------:|
   |BAbdmeyTvMZ | 63/370 | 57.14 | 0.2682 |
   |Dd4bFSTQ8gi | 344 / 392 | 53.49 |  0.2319 |
   |QaLdnwvtxbs& | 238 / 238| 42.72 | 0.2964 |
   |svBbv1Pavdk | 286 / 372 | 47.20 | 0.4720 |
   | Mean        |           | 50.14 | 0.3171|

<!--重复的instance可能会影响acc-->
<!--mean的计算方式/根据episode-->
      
   #### Matterport3D
   
   | Scenes | GON/SON | Accuracy(%) | Position Error(m) |
   |:--------:|:---------:|:-------------:|:-------------------:|
   |2azQ1b91cZZ | 695 / 862 | 44.75 | 0.3096 |
   |EU6Fwq7SyZv | 295/438 | 52.63 | 0.3228 |
   |oLBMNvg9in8 | 483/770 | 52.59 | 0.2554 |
   |TbHJrupSAjP | 421/641 | 60.57 | 0.2833 |
   |x8F5xyUWy9e | 93/276 | 41.92 | 0.2725 |
   |Z6MFQCViBuw | 152/562 | 58.55 | 0.3184 | 
   | Mean        |           | 52.91 | 0.2956|
   

The accuracy of the constructed scene graph is evaluated on the Matterport3D and HM3D datasets. GON represents the number of predicted objects in the scene graph, SON represents the number of objects in the real scene, and Accuracy indicates the percentage of correctly predicted objects in the scene graph. The evaluation details are as follows: the object categories and object coordinates in the scene graph are matched with the ground truth categories and coordinates. An object prediction in the scene graph is considered correct only if the category labels match and the distance between the object coordinates in the scene graph and the ground truth coordinates is less than a threshold (1 meter). Position Error represents the average distance error between the correctly predicted objects in the scene graph and the ground truth objects.
   
To further demonstrate the accuracy of our proposed CSTG in constructing scene graphs, we include a comparative experiment with ConceptGraph. The experimental results are presented in the table.
> todo conceptgraph
<!--

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
-->


### 2. Visual Target Navigation
1. We compared our navigation results with the state-of-the-art IEVE method on our evaluation dataset. 相比它训练的模型，我们的0-shot方法表现具有竞争力
2. 为了验证conway层在navigation任务中的实际效果，我们仅使用物体层执行Visual Target Navigation，指标下降6.1%，体现了conway层在任务中的重要作用

 <!--
 检索时间
 -->

<div style="text-align: center;">

| Scene       | IEVE  |        | Ours  |        | w/o Conway nodes      |        |
|-------------|-------|--------|-------|--------|-----------------------|--------|
|             | sr(%) | spl    | sr(%) | spl    | sr(%)                 | spl    |
| Dd4bFSTQ8gi | 33.6  | 0.0753 | 41.33 | 0.6029 | 24.00                 | 0.7360 |
| QaLdnwvtxbs | 66.4  | 0.2681 | 44.44 | 0.7471 | 48.15                 | 0.5256 |
| VBzV5z6i1WS | 44.8  | 0.1517 | 48.96 | 0.6090 | 34.38                 | 0.6562 |
| ziup5kvtCCR | 45.6  | 0.1332 | 41.03 | 0.5979 | 43.59                 | 0.5086 |
| Nfvxx8J5NCo | 35.2  | 0.1236 | 47.37 | 0.6264 | 25.64                 | 0.6327 |
| svBbv1Pavdk | 57.6  | 0.2126 | 42.42 | 0.6214 | 17.54                 | 0.6283 |
| BAbdmeyTvMZ | 60.8  | 0.3492 | 42.70 | 0.6452 | 39.40                 | 0.7001 |
| mv2HUxq3B53 | 37.6  | 0.1208 | 22.67 | 0.6423 | 40.00                 | 0.6248 |
| Mean        | 47.9  | 0.1823 | 40.19 | 0.6365 | 34.09                 | 0.6265 |
</div>

Initially, we built a object-baseline model that directly searches for the target instance from the list of all detected object instances in the scene. After identifying the target, a navigation graph is used to plan the path and complete the task. However, this flat retrieval approach, which focuses only on the object itself and ignores the surrounding context, limits accurate recognition of the target object to longer distances, reducing navigation success rates.

We also compared our navigation results with the state-of-the-art IEVE method on our evaluation dataset. While IEVE slightly outperforms our method in navigation performance, it relies on a training process to optimize model parameters. In contrast, our method uses a training-free approach, aiming to evaluate its generalization ability by directly applying the model to new environments, offering a more flexible solution for real-world applications.

> todo double check


### 3. Retrieval Strategy Evaluation

<!--
We designed a model to eliminate uncertainties inherent in Large Visual and Language Models (LVLMs) to precisely evaluate the retrieval performance of our scene graph, basing all node selections just on CLIP similarity decisions. During the model's construction, we removed all LVLM-related modules.
-->

For inherented uncertainties elimination, we removed all Large Visual and Language Models (LVLMs) related modules. in order to precisely evaluate the retrieval performance of our scene graph, basing all node selections just on CLIP similarity decisions. 

> todo：和我们方法效果上的比较

   #### 1. The Effectiveness of Top-k Selection on Conway Layer Retrieval
   <!--
   为了找到最好的K
   去掉VLM不确定因素，探讨clip结果的precision
   选出最合适的K
   或者
   加上vlm，评测两个内容：1. clip选择的结果，2. VLM对不同K的表现 
   -->
<div style="text-align: center;">

##### Nodes Retrieval Accuracy
| scene name   | TOP1       |         | TOP3  |         | TOP5  |         | TOP7  |         |
|--------------|------------|---------|------------|---------|------------|---------|------------|---------|
|              | conway acc | obj acc | conway acc | obj acc | conway acc | obj acc | conway acc | obj acc |
| BAbdmeyTvMZ. | 48.48      | 15.15   | 57.58      | 15.15   | 66.67      | 18.18   | 72.73      | 15.15   |
| Dd4bFSTQ8gi  | 77.33      | 25.33   | 89.33      | 33.33   | 93.33      | 36      | 97.33      | 36      |
| mv2HUxq3B53  | 41.33      | 12      | 56         | 17.33   | 68         | 25.33   | 77.33      | 25.33   |
| Nfvxx8J5NCo  | 46.15      | 20.51   | 61.53      | 15.38   | 87.18      | 23.08   | 87.18      | 15.38   |
| QaLdnwvtxbs  | 77.78      | 51.85   | 88.89      | 55.56   | 88.89      | 51.85   | 96.30      | 59.26   |
| svBbv1Pavdk  | 77.19      | 26.32   | 91.23      | 28.07   | 91.23      | 28.07   | 98.98      | 22.81   |
| VBzV5z6i1WS  | 59.38      | 29.17   | 88.54      | 31.25   | 92.71      | 31.25   | 94.79      | 30.21   |
| ziup5kvtCCR  | 71.79      | 35.90   | 87.18      | 35.90   | 89.74      | 35.90   | 92.31      | 35.90   |
| **Mean**     | 62.43      | 27.03   | 77.54      | 29      | 84.72      | 31.21   | 89.62      | 30.01   |

</div>	


在检索过程中，第一轮conway node检索的候选节点数量会影响后续的候选物体数量，我们对第一轮选取的节点数量进行了评测，随着k-value上升，候选conway node增加，其中包含正确节点的概率也随之上升，as Conway Nodes Retrieval Accuracy表格。
Result analysis indicates that setting the k-value to 3 or 5 achieves a good balance between retrieval accuracy for Conway and object nodes. However, when the k-value set up to 5, while the object accuracy sees limited improvement, increasing the k-value leads to a 67% rise in the number of tokens input to the visual language model. 

Considering all factors, we have chosen a k-value of 3 as the optimal solution.



   #### 2. The Effectiveness of Conway Information and Multimodal Information in Scene Graphs
   
   ##### Object Nodes Retrieval Accuracy
|             |             | Room-Object |           |             | Conway-Object |           |
|-------------|-------------|-------------|-----------|-------------|---------------|-----------|
| Scene Names | Multi-Modal | Image Only  | Text Only | Multi-Modal | Image Only    | Text Only |
| BAbdmeyTvMZ | 21.21       | 18.18       | 15.15     | 15.15       | 9.09          | 21.21     |
| Dd4bFSTQ8gi | 20          | 13.33       | 20        | 33.33       | 29.33         | 28        |
| mv2HUxq3B53 | 36          | 13.33       | 26.67     | 17.33       | 10.67         | 25.33     |
| Nfvxx8J5NCo | 25.61       | 25.64       | 33.33     | 15.38       | 15.38         | 33.33     |
| QaLdnwvtxbs | 59.26       | 62.96       | 40.74     | 55.56       | 62.96         | 44.44     |
| svBbv1Pavdk | 22.81       | 15.79       | 28.07     | 28.07       | 14.04         | 28.07     |
| VBzV5z6i1WS | 22.91       | 19.79       | 32.29     | 31.25       | 30.21         | 21.88     |
| ziup5kvtCCR | 38.46       | 23.08       | 30.77     | 35.90       | 43.59         | 58.97     |
| **Mean**    | 30.78       | 24.01       | 28.38     | 29          | 26.90         | 32.65     |


We compared the performance of two different graph structures—the room-object graph and the Conway-object graph—under consistent experimental conditions. 对room-object结构我们把检测出的object层连接到groundtruth的房间节点中，首先根据节点标签筛选room节点，由此观察conway信息的作用。同时，我们对图节点的模态信息进行消融实验，探索不同模态信息的利用效果。
The results are detailed in the table above.
可以看出基于我们的conway层的图在单模态检索中普遍优于传统的房间-物体结构，room结构中，object多模态信息融合表现出更好的效果。但对Conway-object结构，多模态信息相对处理稍弱，与我们的实际模型之间（40.19%）存在较大差异，对原因进行分析，主要差异在于conway层的信息融合。conway层图像包含复杂全景图和复杂文本信息的处理。我们实际采用的方法中应用了LVLM进行推理分析，但此处仅采用了基于CLIP的特征提取，提取效果难以保证。即使在此种不利情况下，相比确定正确的room信息，conway层仍然表现出明显优势，再次证明了我们提出方法的可行性。


         
<!--### 4. 多任务task--> 

<!--
## 参数表<a name="section-heading"></a>

| Subsystem | Hyperparameters | Value |
| --------- | --------------- | ----- |
|     _      |                |       |
|    _       |                 |       |
|       _    |                 |       |
-->

## Appendix
### Floor Free Map
![maptrans](https://github.com/user-attachments/assets/72fa2b88-f56d-4d82-a1aa-2e52e1756cc5)

<!--
https://github.com/user-attachments/assets/76698ad4-1275-4d69-8722-f32298db99e0
-->

### Instance Segmentation Model Selection

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

> todo:重写分析

> we report various semantic segmentation results on Replica dataset. The combination of the RAM and SAM series demonstrates a significant advantage over MaskCLIP and Mask2former. Although its performance metrics are slightly lower than those of fine-tuned models, the open-vocabulary segmentation model demonstrates a clear advantage in scene transferability. There is no significant performance difference between the SAM and SAM2 models, but SAM is more widely-used and adaptable to other models. Therefore, we selected the RAM+SAM combination for object segmentation in scenes.





