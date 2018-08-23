## Relation Networks for Object Detection

> 论文链接：https://arxiv.org/abs/1711.11575
>
> 代码链接：https://github.com/msracver/Relation-Networks-for-Object-Detection

### 1. 论文阅读

这篇paper是CVPR 2018的文章，可能有利于我们目前使用的目标检测中，这篇文章引入了object的关联信息，在神经网络中对object的relations进行建模。

提出了一种relation module，可以在以往常见的物体特征中融合进物体之间的关联性信息，同时不改变特征的维数，能很好地嵌进目前各种检测框架，提高性能。提出了一种特别的代替NMS的去重模块，可以避免NMS需要手动设置参数的问题 。

#### 摘要

在目标检测领域，model object之间的关系能够提高检测的准确度，但是这种方法在基于深度学习的模型中还没有很好的work。当下主流的目标检测的方法还是对各个物体进行单独的检测，本文提出了一种object relation module，通过引入不同物体之间的外观和集合关系做interaction，实现对物体之间relation的建模。relation module参数不多，in-place，可以用来提高目标检测的性能以及实现duplicate removal（通过可以end-to-end学习的module替换nms）,实现完全的end-to-end目标检测器。 

#### Object relation module

作者的relation module和google的一篇做机器翻译的论文“attention is all your need”思想类似。模块的的应用可以增强fc输出的特征以及做duplicate removal，如下图所示：  

![](./1.png)

这里的relation module是in-place的，也就是说，比如在instance recognition里面，输入的是300个object proposal的特征，经过relation module之后，输出的还是300个对应object proposal的特征，只不过这些特征都融合了其他object proposal的信息。 

具体的，假设一个object proposal用他的几何特征 $f_G$(4维的bounding box) $f_A$(典型的1024维)表示。

