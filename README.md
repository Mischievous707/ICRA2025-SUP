# CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation
This repository is the official implementation of the paper: CTSG: Context and Topology based Multi-Modal Scene Graph for Visual Target Navigation[🔗](http://)

<img src="C:\Users\Lynn\Desktop\我的\Typora\拓展\CSTG\img\intruduction.png" alt="github-pic"  />

> - [ ] **[L]** CTSG is a hierarchical 3D scene graph mapping method designed for visual object navigation. It features a dual-layer structure: an object layer and a novel *Conway* layer (short for *context* and *way* topology), composed of topological waypoints enriched with multi-modal context information.
>
> - [ ] **[3]** cases video--现实世界 最好的
>

## Abstract
For visual target navigation tasks, a suitable environment representation plays a vital part in robot system to complete the navigation tasks. 3D Scene Graphs serve as sparse representations that excel at representing environments efficiently compare to dense semantic maps. Typical Scene Graphs are generally constructed based on multi-level semantic labels with hierarchical structures, which may lead to the loss of valuable spatial information, such as spatial topological relations, common sense understanding and visual context information. In this work, we propose **CTSG**, a Hierarchical 3D scene graph mapping approach for visual object navigation. Our graph adopts a dual-layer structure. Besides object layer, a novel conway (short for context information and way topology) layer is introduced, which consists of topological waypoints with rich multi-modal context information. We demonstrate the effectiveness of our method in novel visual target navigation tasks through simulation and real-world experiments across varied environments and instructions.

## Video
<video controls>
  <source src="C:\Users\Lynn\Desktop\new1.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>

## Approach

<img src="C:\Users\Lynn\Desktop\我的\Typora\拓展\CSTG\img\pipeline.png" />

> 描述-优势

### Scene Graph Construction

<img src="C:\Users\Lynn\Desktop\我的\Typora\拓展\CSTG\img\conwaygraph.png" />

> 描述

<img src="C:\Users\Lynn\Desktop\我的\Typora\拓展\CSTG\img\semantic.png" />

> 描述

### Visual Target Navigation with Scene Graph

> **[M]**整体架构图+retrieval details描述

## Experiment

### 1. Graph

obj location acc: 对比ConceptGraph，hovsg?在gt环境下评测

1. gt从habitat-sim如何获取
2. 如何对比（和competetive的xx方法对比）
3. 结果：

|              |      |      |
| ------------ | ---- | ---- |
| ConceptGraph |      |      |
| ours         |      |      |

分析：





## 参数表

| Subsystem | Hyperparameters | Value |
| --------- | --------------- | ----- |
|           |                 |       |
|           |                 |       |
|           |                 |       |

