# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: **CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation**. <!--[🔗](http://)-->

![屏幕截图 2024-09-26 221652](https://github.com/user-attachments/assets/fbad25d8-99fc-4d05-a02e-0d933f31120c)

> https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda

> https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35
>
<!--
## Abstract
For visual target navigation tasks, a suitable environment representation plays a vital part in robot system to complete the navigation tasks. 3D Scene Graphs serve as sparse representations that excel at representing environments efficiently compare to dense semantic maps. Typical Scene Graphs are generally constructed based on multi-level semantic labels with hierarchical structures, which may lead to the loss of valuable spatial information, such as spatial topological relations, common sense understanding and visual context information. In this work, we propose **CTSG**, a Hierarchical 3D scene graph mapping approach for visual object navigation. Our graph adopts a dual-layer structure. Besides object layer, a novel conway (short for context information and way topology) layer is introduced, which consists of topological waypoints with rich multi-modal context information. We demonstrate the effectiveness of our method in novel visual target navigation tasks through simulation and real-world experiments across varied environments and instructions.
-->

## Video
https://github.com/user-attachments/assets/637966af-dfbb-458e-bb6a-15ddcbc1a3d8


## Approach

<img src="/img/pipeline.png" />

> todo：有需要再补充

CTSG is a dual-layer multi-modal scene graph. The conway layer contains navigable waypoints called conway nodes, which provide contextual information and multi-modal attributes like images and descriptions. 
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
### [TODO: 艳平修改标题，加总起句介绍评测了什么+为什么评测（我们的优势）+实验结果+分析（加上conceptgraph）]1. Scene Graph 
？如何合理引入conceptgraph：常见构建方法

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
    
   
The accuracy of the constructed scene graph is evaluated on the Matterport3D and HM3D datasets. GON represents the number of predicted objects in the scene graph, SON represents the number of objects in the real scene, and Accuracy indicates the percentage of correctly predicted objects in the scene graph. The evaluation details are as follows: the object categories and object coordinates in the scene graph are matched with the ground truth categories and coordinates. An object prediction in the scene graph is considered correct only if the category labels match and the distance between the object coordinates in the scene graph and the ground truth coordinates is less than a threshold (1 meter). Position Error represents the average distance error between the correctly predicted objects in the scene graph and the ground truth objects.
   
To further demonstrate the accuracy of our proposed CSTG in constructing scene graphs, we include a comparative experiment with ConceptGraph. The experimental results are presented in the table.



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

 <!--
   conway整体方法（包括构图、检索）有效性

   ​	1. with/without：在原本的基础上去掉conway检索=只对所有object列表做clip+vlm

   > 2. room cluster：在原本的基础上把conway换成room（聚类得到，有图文），其他不变
   -->

<div style="text-align: center;">

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

</div>

Initially, we built a object-baseline model that directly searches for the target instance from the list of all detected object instances in the scene. After identifying the target, a navigation graph is used to plan the path and complete the task. However, this flat retrieval approach, which focuses only on the object itself and ignores the surrounding context, limits accurate recognition of the target object to longer distances, reducing navigation success rates.

We also compared our navigation results with the state-of-the-art IEVE method on our evaluation dataset. While IEVE slightly outperforms our method in navigation performance, it relies on a training process to optimize model parameters. In contrast, our method uses a training-free approach, aiming to evaluate its generalization ability by directly applying the model to new environments, offering a more flexible solution for real-world applications.
> todo double check


### 3. Retrieval Strategy Evaluation

<!--
We designed a model to eliminate uncertainties inherent in Large Visual and Language Models (LVLMs) to precisely evaluate the retrieval performance of our scene graph, basing all node selections just on CLIP similarity decisions. During the model's construction, we removed all LVLM-related modules.
-->

We removed all Large Visual and Language Models (LVLMs) related modules during model's construction for elimination inherented uncertainties in order to precisely evaluate the retrieval performance of our scene graph, basing all node selections just on CLIP similarity decisions. 

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

| K-Selection  |  Top 1  |  Top 3  |  Top 5  |  Top 7  |
|:------------:|:-------:|:-------:|:-------:|:-------:|
| Conway Acc   |  49.09  |  47.69  |  46.93  |  44.64  |
| Object Acc   |  27.03  |  29.00  |  31.21  |  30.01  |

</div>	
   <!-- 实验设置：本研究设计了一种模型，该模型排除了大型视觉语言模型（LVLM）中的不确定性因素，所有节点的选择均基于CLIP相似度进行决策。在构建模型时，我们移除了与所有LVAM相关的模块。在Conway节点的选择上，我们采用了一种策略：将所有候选Conway节点下挂载的物体节点的CLIP特征与目标物体图片的CLIP特征进行相似度匹配，选择最相似的物体节点，将其连接的Conway节点作为导航点以规划路径。实验结果如上表所示。
   分析结果表明，当k值设定为3或5时，可以较好地平衡Conway和物体的检索正确率。然而，当k值增至5时，虽然物体准确率的提升有限，但导航准确率却有所下降。此外，考虑到我们的方法会调用LVLM，k值的增大会导致输入到视觉语言模型的token数量增加67%。综合考虑，我们选择k值为3作为最优解-->
<!--
> For the selection of Conway nodes, we adopted a strategy where we matched the CLIP features of all candidate Conway nodes' attached object nodes with the CLIP features of the target object image to find the most similar object node. The Conway node connected to this object node was then used as the navigation point to plan the path. The experimental results are shown in the table above.
>
> Analysis of the results indicates that setting the k value to 3 or 5 can achieve a good balance between the retrieval accuracy of Conway and object nodes. However, when the k value is increased to 5, although the improvement in object accuracy is limited, the navigation accuracy decreases. Moreover, considering that our method invokes LVLMs, an increase in the k value leads to a 67% increase in the number of tokens input to the visual language model. Taking all factors into account, we have chosen a k value of 3 as the optimal solution.

For Conway nodes selection, we used a strategy where the CLIP features of candidate Conway nodes' attached object nodes were matched with the CLIP features of the target object image to identify the most similar object node. The Conway node linked to this object node was then chosen as the navigation point for path planning. The experimental results are shown in the table above.

-->
在检索过程中，第一轮conway node检索的节点候选数量会影响xxxxxxxxx
Result analysis indicates that setting the k-value to 3 or 5 achieves a good balance between retrieval accuracy for Conway and object nodes. However, when the k-value set up to 5, while the object accuracy sees limited improvement, navigation accuracy also declines. 
Additionally, since our method invokes LVLMs, increasing the k-value leads to a 67% rise in the number of tokens input to the visual language model. 

Considering all factors, we have chosen a k-value of 3 as the optimal solution.


   #### 2. The Effectiveness of Multimodal Information in Scene Graphs
   
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
<!--
> We compared the performance of two different graph structures—the room-object and conway-object graphs—under uniform experimental conditions. The results of the experiment are detailed in the aforementioned table. Preliminary observations indicate that regardless of whether the room-object or conway-object graph form is utilized, augmenting the nodes with multimodal information significantly enhances the accuracy of object retrieval.
>
> Further analysis revealed that under identical experimental configurations, the conway-object graph outperformed the room-object graph in retrieval performance. This finding fully confirms the significant advantages of the multimodal conway-object scene graph we proposed in retrieval tasks.
-->

We compared the performance of two different graph structures—the room-object graph and the Conway-object graph—under consistent experimental conditions. 

The results are detailed in the table above.

>todo: 实验结果分析
         
<!--### 4. 多任务task-->

      


## 参数表<a name="section-heading"></a>

| Subsystem | Hyperparameters | Value |
| --------- | --------------- | ----- |
|     _      |                |       |
|    _       |                 |       |
|       _    |                 |       |

## Appendix
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





