# FCN
## patchwise training
patchwise training是与全卷积网络不同的一种方法。主要指对每一个感兴趣的像素，以它为中心取一个patch，然后输入网络，输出则为该像素的标签，训练时就将一个个patch组成一个batch作为网络输入。由于一整张图片直接输入可能高度重叠，所以需要一些sampling方法对选取一些patches作为训练集，一方面平衡类别，另一方面解决空间相关性等问题。这种patchwise training确保输入具有足够的方差并且是训练数据集的有效表示（小批量应该具有与训练集相同的分布）。这种技术还有助于更快地收敛并平衡类。在本文中，他们声称没有必要使用patchwise training，如果你想平衡类别，你可以加权或抽样。
## Shift-and-stitch是稀疏滤波
Shift-and-stitch是从overfeat迁移过来的trick。可以从粗略输出产生密集预测而无需插值(本实验中使用的是上采样而不是本方法)。
如果以f为下采样因子(网络最后输出h= H/f), 则输入向右(左侧和顶部填充xy)移动x个像素，向下移动y个像素, $(0\le x,y\le f-1)$，这$f^2$个输入每个都通过convnet运行, 并且输出交错, 以便预测对应于其接受场中心的像素。本作者分析认为只更改滤波器和stride可以产生与此Shift-and-stitch技巧相同的输出：
&emsp;&emsp;考虑一个具有输入步幅s的图层(卷积或池化), 以及一个带滤波器权重f~ij~的后续卷积图层(这里与深度无关暂不考虑)。 将低层的输入stride设置为1, 通过上采样因子s来采样输出, 就像Shift-and-stitch一样. 然而, 将原始滤波器与上采样输出进行卷积并不会产生与Shift-and-stitch相同的结果, 因为原始滤波器只能看到其(现在上采样)输入的缩减部分. 为了重现这个技巧, 通过把滤波器扩大为如下形式,来稀疏滤波器(空洞卷积，i、j都是从0开始)。
```mathjax
$$f_{i j}^{\prime}=\left\{\begin{array}{ll}{f_{i / s, j / s}} & {\text { if } s \text { divides both } i \text { and } j} \\ {0} & {\text { otherwise }}\end{array}\right.$$
```
&emsp;&emsp;这种Shift-and -stitch技巧是一种折衷做法：输出更加密集，且没有减小filter的感受野，但是相对于原始的设计filter不能感受更精细的信息。
&emsp;&emsp;本文未使用本方法因为上采样更有效。
## 上采样是反向步长卷积
一种关联粗略输出到密集像素的方法是插值(interpolation)，如简单的双线性插值等等。

而上采样其实可以看做stride = 1/s 的卷积。这种卷积与插值的区别在于插值的参数是固定的（比如双线性插值参数是周围最近的4个值）而卷积则是参数可学习的方法。
upsampling =  backwards convolution = deconvolution = transposed convolution
反卷积是卷积的逆过程
Full padding, transposed(图中的反卷积, input是2×2, output是4×4)
![transposed convolution](_v_images/20190516203605501_26910.gif)
Zero padding, non-unit strides, transposed(input3×3, 转化后是5×5, output是5×5)
![transposed convolution](_v_images/20190516203625913_21590.gif)

## Patchwise training是对loss的采样
patchwise和全卷积能被用来产生任意分布，尽管他们相对的计算效率依赖于重叠域和minibatch的大小。在由所有单元的感受野组成的每一个批次, 基于一幅图的损失之下（或图像的集合）, 整张图像的全卷积训练等同于patchwise训练。
&emsp;&emsp;patcheswise中的采样能纠正分类失调和减轻密集空间相关性的影响。在全卷积训练中，平衡分类也能通过给损失赋权重实现，对损失采样能被用来标识空间相关。本文研究表明patchwise并没有比全卷积更好。
## 架构
使用逐像素的多项逻辑损失$\frac{1}{m}\sum\limits_{i=1}^{m}[y^{(i)}\ln (\hat{y^{(i)}}) + (1-y^{(i)}) \ln (1-\hat{y^{(i)}})]$，评判标准用MIoU(mean pixel intersection over union)也是集合交并集之比。
### 从分类器到dense FCN
以VGG16为例(本文VGG19和16表现相当)去掉分类头，全连换为卷积，附加了一个1*1的、通道维数为(class+1(background)）的卷积来预测在每个粗糙的输出位置每个分类的得分，后面紧跟一个反卷积层用来双线性上采样粗糙输出到像素密集输出。
### 判断“是什么”和“在哪里”
因为有32stride的原因，使得上采样还原图像的时候得到的图像很粗糙。本文提出将最后输出层与低层stride更小的特征结合起来。这样便将一个线性拓扑结构变成了一个有向无环图(DAG)。
![](_v_images/20190517101654993_22697.png =660x)
以FCN-16s为例：
1. 在pool4（stride=16）的顶部增加一个1*1的卷积层来产生附加的类别预测；
2. 将conv7（fc7的卷积化，stride=32）通过增加一个2×的上采样层后和上一步pool4的结果加起来（初始化这个2×上采样为参数可学习的双线性插值）；
3. 将步长为16的预测层上采样回图像尺寸。

同理也可以对pool3开始操作融合，这样得到的是FCN-8s。网络的初始化使用预训练的VGG参数，但由于后两层由全连接层转化过来，没有预训练参数，使用零初始化。

### 评判标准
![](_v_images/20190518212934927_8401.png =650x)