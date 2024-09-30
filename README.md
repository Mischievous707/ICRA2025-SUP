# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: **CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation**. <!--[🔗](http://)-->

![屏幕截图 2024-09-26 221652](https://github.com/user-attachments/assets/fbad25d8-99fc-4d05-a02e-0d933f31120c)

https://github.com/user-attachments/assets/ff1d11a1-2961-4fb1-8d08-ae9c999f9cda

https://github.com/user-attachments/assets/20362768-7054-4c69-925a-1913e1fb4b35


## Video

<div style="text-align: center;">
   
https://github.com/user-attachments/assets/637966af-dfbb-458e-bb6a-15ddcbc1a3d8

</div>

## Approach

<img src="/img/pipeline.png" />

**CTSG** is a dual-layer multi-modal scene graph. The Conway layer contains navigable waypoints called Conway nodes, which provide contextual information and multi-modal attributes as images and descriptions. 
The connections between Conway nodes represent their navigability. 
The object layer includes object nodes, with attributes like individual descriptions and object images, and the connections between Conway and object nodes reflect spatial relationships.

Based on our CSTG, we develop an efficient dual-level retrieval strategy for visual target navigation, which leverages VLMs to interpret semantic information and enrich representations. 
We conduct Conway-level and object-level retrieval, coupled with a backtracking search to ensure robustness.


## Experiment & Analysis
We evaluated and analyzed our approach from several aspects: the accuracy of graph construction, the practical effectiveness of visual target navigation, and the role of graph information in retrieval tasks.
[parameter_list](#section-heading)

### 1. Scene Graph Evaluation [TODO: 加总起句介绍评测了什么+为什么评测（我们的优势）+实验结果+分析（加上conceptgraph）] 
？如何合理引入conceptgraph：常见构建方法
本部分中评测了构建的场景图的准确性，包括物体类别及空间位置。我们与（同类型的）conceptgraph进行了比较，
证实了1. node布局合理 2. 构图方法的有效性。
   1. HM3D

   | Scenes | GON/SON | Accuracy(%) | Position Error(m) |
   |:--------:|:---------:|:-------------:|:-------------------:|
   |BAbdmeyTvMZ | 63/370 | 57.14 | 0.2682 |
   |Dd4bFSTQ8gi | 344 / 392 | 53.49 |  0.2319 |
   |QaLdnwvtxbs& | 238 / 238| 42.72 | 0.2964 |
   |svBbv1Pavdk | 286 / 372 | 47.20 | 0.4720 |
   | Mean        |           | 50.14 | 0.3171|

<!--重复的instance可能会影响acc-->
<!--mean的计算方式/根据episode-->
      
  2. Matterport3D
   
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


### 2. Visual Target Navigation

We compared our navigation results with the state-of-the-art IEVE method on our evaluation dataset. 

While IEVE relies on a training process to optimize model parameters, our method uses a training-free approach, aiming to evaluate its generalization ability by directly applying the model to new environments, offering a more flexible solution for real-world applications, also demonstrating competitive performance.

To verify the practical impact of the Conway layer in the navigation task, we performed Visual Target Navigation using only the object layer, resulting in a 6.1% drop in performance, highlighting the critical role of the Conway layer in the task.
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



### 3. Retrieval Strategy Evaluation

To eliminate inherent uncertainties, we removed all modules related to Large Visual and Language Models (LVLMs). This allowed us to accurately assess the retrieval performance of our scene graph, with all node selections based solely on CLIP similarity decisions.


1. The Effectiveness of Top-k Selection on Conway Layer Retrieval

<div style="text-align: center;">

##### Nodes Retrieval Accuracy
| scene name   | TOP1       |         | TOP3  |         | TOP5  |         | TOP7  |         |
|--------------|------------|---------|------------|---------|------------|---------|------------|---------|
|              | Conway acc | obj acc | Conway acc | obj acc | Conway acc | obj acc | Conway acc | obj acc |
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

During the retrieval process, the number of candidate nodes in the first round of Conway node retrieval affects the subsequent pool of candidate objects. We evaluated the impact of the number **TopK** of selected nodes in the first round. As the k-value increases, the number of candidate Conway nodes rises, and the likelihood of including the correct node also increases, as shown in the Conway Nodes Retrieval Accuracy table.

Result analysis indicates that setting the k-value to 3 or 5 strikes a good balance between retrieval accuracy for Conway and object nodes. However, when the k-value is set to 5, while object accuracy shows limited improvement, the number of tokens input to the visual language model increases by 67%.

Taking all factors into account, we have selected a k-value of 3 as the optimal solution.



2. The Effectiveness of Conway Information and Multimodal Information in Scene Graphs
   
**Object Nodes Retrieval Accuracy**
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


We compared the performance of two different graph structures—the room-object graph and the Conway-object graph—under consistent experimental conditions. For the room-object structure, we connected detected object layers to ground truth room nodes, filtering room nodes based on their labels to observe the impact of Conway information. We also conducted ablation experiments on the modal information of graph nodes to explore the effectiveness of different modalities.

The results are shown in the table above. 
It is evident that our Conway-based graph generally outperforms the traditional room-object structure in single-modality retrieval. In the room-object structure, the fusion of multimodal object information yielded better results. 

However, in the Conway-object structure, multimodal information processing was relatively weaker, with a significant difference (40.19%) compared to our full model. Upon analysis, the main discrepancy lies in the integration of information in the Conway layer. The Conway layer handles complex panoramic images and rich textual information. In our actual approach, we used LVLM for reasoning, whereas in this comparison, we relied only on CLIP-based feature extraction, which may have compromised performance. 

Despite these limitations, the Conway layer still showed a clear advantage over determining the correct room information, further demonstrating the feasibility of our proposed method.


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

We present various semantic segmentation results on the Replica dataset. 

The combination of the RAM and SAM series shows a significant advantage over MaskCLIP and Mask2Former. While its performance metrics are slightly lower than those of fine-tuned models, the open-vocabulary segmentation model demonstrates a clear edge in scene transferability. 

There is no notable performance difference between the SAM and SAM2 models, but SAM is more widely used and adaptable to other models. Therefore, we chose the RAM+SAM combination for object segmentation in scenes.




