# 1. FPN
利用ConvNet特征层的金字塔形状，创建一个在所有尺度上都具有强大语义的特征金字塔。将低分辨率，强语义的特征与高分辨率，弱语义的特征相结合，通过自上而下的路径和横向连接。
## 1.1. Bottom-up pathway
1. 前向传播中，将一个conv net中相同尺寸的feature map称为同一network stage；
2. 一个stage对应产生一个金字塔层。选择每个阶段的最后一层的输出作为特征映射参考集；  

这里论文中举了一个resnet的例子，如图为其结构。只取$\lbrace C_2, C_3, C_4, C_5 \rbrace$对应conv2, conv3, conv4, 和conv5最后的outputs。conv1没取因为内存占用太大。
![](_v_images/20190513210416102_9624.png =660x)
## 1.2. Top-down pathway and lateral connections
自顶向下的路径通过上采样来自较高金字塔等级的特征映射来产生更高分辨率的特征。这样形成的特征在语义上的信息比较强，但是空间信息很粗糙，分辨率因为是上采样得到的也很粗糙。

于是通过横向连接lateral connections将具有相同尺寸的Bottom-up和Top-down网络融合起来进行增强。
![](_v_images/20190513221804132_15336.png =640x)
具体说来：
1. Top-down网络每一步上采样为2倍，为了简便采用最近邻上采样；
2. Top-down网络的第一层来自$C_5$经过1\*1卷积后得到的特征；
3. Bottom-up和Top-down网络按元素相加，但因为深度方向维度不同，Bottom-up网络需要1\*1卷积降维；
4. 迭代以上过程，直到生成最佳分辨率映射；
5. 在每个合并的映射上添加一个3×3卷积来生成最终的特征映射$\lbrace P_2 , P_3 , P_4 , P_5 \rbrace$，这是为了减少上采样的混叠效应。

Notes：由于金字塔的所有层都共享分类器/回归器用于输出预测，所以在所有特征映射中特征深度需要固定为d。本文取d=256。本文中没有使用非线性层。
## 1.3. 应用
### 1.3.1. 用于RPN区域推荐
在特征金字塔的每个层级上附加一个与RPN相同设计的头部（3x3 conv和两个1x1sibling convs用于预测种类和位置）。而且这些不同层级的头部网络是同一个(共享参数)。本文也做了实验比较是否共享参数带来的不同，结果得到的准确率很相似。共享参数的良好性能表明金字塔的所有层级共享相似的语义级别。

这样，在RPN中就不用给anchor赋多尺度的scale了。每层对应一个尺度即可。本文中，定义锚点$\lbrace P_2, P_3, P_4, P_5, P_6 \rbrace$分别具有$\lbrace 32^2 , 64^2 , 128^2 , 256^2 , 512^2 \rbrace$个像素的面积。同时，每个层级上定义多个长宽比$\lbrace 1:2, 1:1, 2:1 \rbrace$。这样金字塔上共有15个anchor。

同faster RCNN一样，根据anchor和实际边界框的IoU给它们标记正负样本：
如果一个锚点对于一个给定的实际边界框具有最高的IoU或者与任何实际边界框的IoU超过0.7，则给其分配一个正标签，如果其与所有实际边界框的IoU都低于0.3，则为其分配一个负标签。

**消融实验**
![](_v_images/20190514200443886_14338.png =640x)
1. 与原始RPN比较：发现原始RPN中从conv4输入RPN和从conv5输入RPN相比区别不大。这表明单个更高级别的特征映射是不够的，因为存在在较粗分辨率和较强语义之间的权衡。
2. 去掉自上而下网络：结果差于FPN。作者推测这是因为自下而上的金字塔的不同层次之间存在较大的语义差距。
3. 去掉横向连接：这个自顶向下的金字塔具有强大的语义特征和良好的分辨率。但其定位并不精确，因而只在较大目标上效果较好。
4. 不用金字塔结构，即只用P~2~的特征：金字塔层级可以增加对尺度变化的鲁棒性。另外，由于P2较大的空间分辨率，单独使用P2会导致更多的锚点（750k，如表示）。这个结果表明，大量的锚点本身并不足以提高准确率。
### 1.3.2. 用于fast RCNN
通过以下公式将宽度为w和高度为h（在网络上的输入图像上）的RoI分配到特征金字塔的级别P~k~上：
```mathjax
$$k=\lfloor k_0+\log_2(\sqrt{wh}/224) \rfloor$$
```
其中224是ImageNet规范输入大小，k~0~是大小为w×h=224^2^的RoI应该映射到的目标级别。Faster RCNN中从C~4~开始，所以k~0~设置为4。
该方程意味着如果RoI的尺寸变小了（比如224的1/2)，它应该被映射到一个更精细的分辨率级别（比如k=3)。也就是说，以上操作是将前面预测的RoI根据尺寸重新分配给更适合它的层级。

我们在所有级别的所有RoI中附加预测器头部（在Fast R-CNN中，预测器头部是特定类别的分类器和边界框回归器）。并且同样所有级别共享参数。

采用RoI池化在RoI中提取7×7特征，并在最终的分类层和边界框回归层之前附加两个隐藏单元为1024维的全连接（fc）层（每层后都接ReLU层）。这些层是随机初始化的，因为ResNets中没有预先训练好的fc层。请注意，与标准的conv5头部相比，我们的2-fc MLP头部更轻更快。

**消融实验**
![](_v_images/20190514201650031_20480.png =640x)
一、为避免区域推荐带来的误差，这里所有的推荐区域均来自FPN+RPN的固定推荐。  
&emsp;&emsp;1. (b)表示全连接层并没有带来优势；  
&emsp;&emsp;2. (d)、(e)结果与RPN中结果类似。不过值得注意的是，去除自上而下的连接（d）显著降低了准确性，表明Fast R-CNN在高分辨率映射中使用了低级特征。  
&emsp;&emsp;3. (f)不用金字塔结构仍得到了较好的结果。作者认为这是因为RoI池化是一种扭曲式的操作，对区域尺度较不敏感。  

二、与faster RCNN整体比较。Faster R-CNN系统中，RPN和Fast R-CNN必须使用相同的骨干网络来实现特征共享。于是这里进行了整体比较。
![](_v_images/20190514203100596_22226.png =640x)
&emsp;&emsp;（a）和（b）的基线比He等人在faster RCNN论文中提供的基线强大得多。
&emsp;&emsp;1. 作者发现以下实现有助于缩小差距：
&emsp;&emsp;&emsp;&emsp;（i）使用800像素的图像尺度，而不是原文的600像素；
&emsp;&emsp;&emsp;&emsp;（ii）训练时每张图像有512个ROIs，而不是原文中的64个，可以加速收敛；
&emsp;&emsp;&emsp;&emsp;（iii）使用5个尺度的锚点，而不是原文中的4个（添加32^2^）；
&emsp;&emsp;&emsp;&emsp;（iv）在测试时，每张图像使用1000个提议，而不是原文中的300个。
&emsp;&emsp;2. 共享特征普遍会加快测试速度(但训练时间增加了)，提高平均召回率AR。
&emsp;&emsp;3. 本文提出的方法比上表中(a)的基线时间快。作者认为是因为虽然通过FPN中的额外层引入了较小的额外成本，但具有更轻的头部。