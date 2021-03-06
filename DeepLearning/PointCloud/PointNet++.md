# PointNet++
PointNet基本思想是学习每个点的空间编码，然后将所有单个点特征聚合到全局点云上。
在设计上无法捕获点的局部结构。本文改进引入分层网络结构，将PointNet递归应用于输入点集的嵌套分区。通过度量点之间的空间，学习局部特征（距离度量定义了可能表现出不同属性的局部邻域）。CNN的成果证明局部邻域的作用很重要。

另外，点云的分布往往是不均匀的，其分布密度差异较大。

首先通过底层空间距离为度量，将点集划分为重叠的局部区域。从小的领域中提取精细的几何结构局部特征。再用这局部特征进一步分组形成大一些的单元来产生高级特征。重复这一过程直到全局特征都被提取。即可利用PointNet来提取划分的局部点云。

TODO：如何对点集分区；如何提取本地特征。

对于分区问题，使用点球模型，其参数包括中心点位置和球的大小，从全部数据集中选出若干质心点，然后选取半径，完成覆盖整个数据集的任务。在质心点的选取上，采用的是FPS算法（farthest point sampling），即随机选取一个点，然后选择离这个点最远的点加入到结果集中，迭代这个过程，直到结果集中点的数量达到某个给定值。
而由于点云分布不均匀，半径的选取也应该不同。
## hierarchical structure 
层次结构由许多集合抽象层级（level）组成，在每一层级处理一组点并提取出一个新的点集with fewer elements。这样的处理层级由三部分构成：采样层，分组层和PointNet层。
### 采样层
采样层从输入中选取一组点作为局部地区的中心。

输入点集$\{x_1,x_2,\ldots,x_n\}$，采用FPS算法选子集$\{x_{i_{1}},x_{i_{2}},\ldots,x_{i_{m}}\}$使得第i个子集中的第j个点离前面的所有点都最远。相较于随机采样，这种方法在中心点数量相同时有更好的收敛性，以数据相关的方式产生感受野。
#### 关于FPS最远点采样
最远点采样的算法是这样: 每个batch先随机初始化一个点加入备选点集, 然后不断迭代找到离当前备选点集中最后一个点(即刚加入的那个点)最远的点加入备选点集,一直迭代到备选点集中的点数量达到要求

```python
# 当已知ind,找原始点xyz数据时:
# xyz [B, N, C], batch_indices [B], farthest存最远点的ind [B],输出 [B, 1, 3],相当于将两个B维结合成一个B维二维数组
batch_indices = torch.arange(B, dtype=torch.long)
centroid = xyz[batch_indices, farthest, :].view(B, 1, 3)
```
### 分组层
分组层通过在中心点附近找近邻点建立局部区域。

分组层的输入仍是原点集$\{x_1,x_2,\ldots,x_n\}$，尺寸$N\times (d+C)$（N个点，d维之前提取的特征，C维坐标），同时还已知采样组得到的中心点坐标，其尺寸$N'\times d$，输出是以这些中心点为基础的尺寸为$N'\times K \times (d+C)$的一组点集,其中K表示中心点的近邻的K个点，但不同组间的K值可能不同，但接下来的PointNet层可以处理这一问题转化为固定长度的局部特征向量。
这里类比于CNN中，一个像素和它的周围像素分为一组，这是靠的index间的曼哈顿距离。

分组策略使用的是ball query。该方法就是以中心点为基础，以一定的半径画圆，圆以内的点都分在组内（在实现中给K值设定了上限）。当然你也会问为什么不用KNN。作者对两种方法进行了比较，并表示KNN虽然找的是固定K个点，但其覆盖区域的scale肯定会不同，而ball query却能保证固定的比例能保证区域特征更容易泛化，这对需要局部模式识别的任务如语义分割更有利。
### PointNet层
PointNet层用一个mini-PointNet网络编码局部区域为特征向量。

输入是$N'$个局部区域，其数据尺寸为$N'\times K \times (d+C)$，输出尺寸为$N'\times (d+C')$。

1. 首先将局部区域点的坐标转化为相对于中心点的局部坐标：$x_{i}^{(j)}=x_{i}^{(j)}-\hat{x}^{(j)} for\space i = 1,2,\ldots,K; \space\space j=1,2,\ldots,d$，$\hat{x}$表示中心点坐标。
2. 使用PointNet框架进行提取特征。$f\left(x_{1}, x_{2}, \ldots, x_{n}\right)=\gamma\left(\max_{i=1, \ldots, n}\left\{h\left(x_{i}\right)\right\}\right)$

这样一个层级的输入是$N\times K\times (d+C)$的矩阵（这时的N相当于batch, K个点，d维坐标，C维点特征），输出矩阵$N'\times (d+C')$，含有$N'$个下采样点和新的$C'$维局部特征向量。
## 非均匀采样密度下的鲁棒特征学习
理想状况下，我们想尽可能精密地捕获密集采样区的精细细节。然而在点云密度低的地方这样操作显然是不行的，在这时需要感受野大一些，因而提出density adaptive PointNet层，在输入密度变化时会学着结合来自不同尺度区域的特征。这样的网络称为PointNet++。

上一节中已经介绍过每一个level有三个部分，但提取的都是单尺度信息，在PointNet++中每个level都会提取多个尺度并根据局域密度而结合到一起。本文提出了两种结合方法。
![](_v_images/20190729112532227_26172.png =300x)
### Multi-scale grouping (MSG)
最简单的想法就是在分组层时，采用不同的半径进行采样，再将不同尺度的特征拼接起来。

另外采取了random input dropout的方法，对每一组数据进行随机丢弃一些输入点：对每一个训练点集，选取一个dropout ratio $\theta$，而$\theta$是从$[0,p]$上均匀采样的，本文$P=0.95$，对每一个点，用概率$\theta$决定是否丢弃某一个点，这样便得到了具有不同稀疏性的训练集。实验证明这个DP的方法帮助很大。
### Multi-resolution grouping (MRG)
MSG因为需要在不同尺度下计算，因而计算开支和时间支出较大，因而提出本想法。

如上图示层$L_i$的特征由两个向量构成，偏左的向量来自于上一层$L_{i-1}$的特征，另一个来自在本层使用一个PointNet的特征。这样，在密度不均匀的时候，可以自动学习到相应的权重（密度小的时候，后一个向量更重要；vice versa）
## 用于语义分割
在上面的结构中，不断对点集进行了下采样，使点变少，这对语义分割中要对每个点标签是不适用的。解决办法是使用插值和skip links将下采样的特征传递到原来的点上去。
![单尺度下网络结构](_v_images/20190730113038934_14524.png =800x)

设某个level的输入点：$N_{l-1}\times(d+C)$，输出点数量$N_{l}$（$N_{l-1}\ge N_{l}$）。通过在$N_{l-1}$的点坐标处插值$N_{l}$点的特征值$f$。插值方法使用基于KNN的反距离加权平均，公式如下（in default $p = 2, k = 3$）：
```mathjax
$$
f^{(j)}(x)=\frac{\sum_{i=1}^{k} w_{i}(x) f_{i}^{(j)}}{\sum_{i=1}^{k} w_{i}(x)} \quad \text { where } \quad w_{i}(x)=\frac{1}{d\left(x, x_{i}\right)^{p}}, j=1, \ldots, C
$$
```
对$N_{l-1}$个点插值后得到的特征与抽象出来的的skip linked点特征拼接起来，经过一个unit PointNet（类似于CNN中的1\*1卷积），然后应用一些全连层和ReLU来更新每个点的特征向量。重复以上过程直到还原到原始点集数量。

在分类和语义分割任务中MSG效果比MRG好。而在非欧氏空间中，将距离尺度改为沿曲面的 测地距离也能完成任务。

## 网络结构
其中$SA(K,r,[l_1,\dots,l_d ])$表示set abstraction level，有K个中心点，半径为r，使用PointNet全连接层的宽度$l_i$；$SA([l_1,\dots,l_d ])$表示全局的提取；用$SA(K,[r^{(1)},\dots,r^{(m)}],[[l^{(1)}_1,\dots,l^{(1)}_d],\dots,[l^{(m)}_1,\dots,l^{(m)}_d]])$表示MSG中m个scale；$FC(l,dp)$表示全连层l，dp表示dropout ratio；$FP([l_1,\dots,l_d ])$为d个全卷积层的feature propagation，用于更新插值和skip link的拼接特征向量。除了最后一个输出分出预测的层，所有的全连层后都跟着BN和ReLU。

分类SSG：
```mathjax
$$
\begin{array}{l}{S A(512,0.2,[64,64,128]) \rightarrow S A(128,0.4,[128,128,256]) \rightarrow S A([256,512,1024]) \rightarrow} \\ {F C(512,0.5) \rightarrow F C(256,0.5) \rightarrow F C(K)}\end{array}
$$
```
MSG：
```mathjax
$$
\begin{array}{l}{S A(512,[0.1,0.2,0.4],[(32,32,64],[64,64,128],[64,96,128]) \rightarrow} \\ {S A(128,[0.2,0.4,0.8],[[64,64,128],[128,128,256],[128,128,256]] \rightarrow} \\ {S A([256,512,1024]) \rightarrow F C(512,0.5) \rightarrow F C(256,0.5) \rightarrow F C(K)}\end{array}
$$
```
MRG有三个分支，下面的branch 1和branch 2拼接起来送入branch 4，branch 3和branch 4的输出拼接起来送入$FC(512,0.5) \rightarrow FC(256,0.5) \rightarrow FC(K)$：
```mathjax
$$
\begin{array}{l}{\text { Branch } 1 : S A(512,0.2,[64,64,128]) \rightarrow S A(64,0.4,[128,128,256])} \\ {\text { Branch } 2 : S A(512,0.4,[64,128,256]) \text { using } r=0.4 \text { regions of original points }} \\ {\text { Branch } 3 : S A(64,128,256,512) \text { using all original points. }} \\ {\text { Branch } 4 : S A(256,512,1024)}\end{array}
$$
```
用于语义场景分割（FP中的最后两个全连接层跟着有dropout=0.5层）：
```mathjax
$$
\begin{array}{l}{S A(1024,0.1,[32,32,64]) \rightarrow S A(256,0.2,[64,64,128]) \rightarrow} \\ {S A(64,0.4,[128,128,256]) \rightarrow S A(16,0.8,[256,256,512]) \rightarrow} \\ {F P(256,256) \rightarrow F P(256,256) \rightarrow F P(256,128) \rightarrow F P(128,128,128,128, K)}\end{array}
$$
```
用于物体分割（FP中的最后两个全连接层跟着有dropout=0.5层）：
```mathjax
$$
\begin{array}{l}{S A(512,0.2,[64,64,128]) \rightarrow S A(128,0.4,[128,128,256]) \rightarrow S A([256,512,1024]) \rightarrow} \\ {F P(256,256) \rightarrow F P(256,128) \rightarrow F P(128,128,128,128, K)}\end{array}
$$
```

## 代码详解
[pointnet++](https://blog.csdn.net/weixin_39373480/article/details/88934146)