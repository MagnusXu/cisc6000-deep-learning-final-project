# cisc6000-deep-learning-final-project

## Reference
### Deep Reinforcement Learning of Volume-guided Progressive View Inpainting for 3D Point Scene Completion from a Single Depth Image

思路：文章将一个2D的图像修复问题变成了一个在3D空间上进行序列决策的问题，它将待修复的深度图投影成3D点云，然后使用强化学习算法寻找在点云的各个视角下看起来最差的一个深度图，然后使用2D的图像修复网络来对这个最差深度图进行补全。同时作者还使用了SSCNet把深度图变成了体素化后的3D场景，并在这个场景中的相应视角得到另一张的深度图，为2D的图像修复网络提供了一个全局的语义指导。

步骤大概是：

1、深度图D0生成点云，然后使用强化学习方法在点云上寻找一个深度图看起来最差的视角V，得到这个最差的深度图D

2、将深度图D0使用SSCNet体素化，生成体素化后的场景S，然后将场景S旋转至V视角下，得到相应的深度图D_T

3、将D和D_T concat到一起，使用2DCNN网络进行Inpaint，得到修复后的深度图D_X

4、回到步骤1，令 D0=D_X，继续循环修复

文章的网络有四个个模块：

1、a volume completion network（SSCNet）

2、a depth inpainting network（2DCNN）

3、 a differentiate projection layer

4、multi-view convolutional neural network (MVCNN，强化学习网络)

那么它是怎么训练的呢？首先独立的训练SSCNet，然后固定SSCNet的参数，训练2DCNN inpainting 网络，最后再对这两个网络进行联合训练。

文章的重点是在强化学习上，作者觉得在三维点云下，各个视角的深度图修补顺序会对三维场景重建的效果产生很大的影响，所以采用强化学习的策略来对深度图的修复顺序进行规划
