# Mask R-CNN
MRCNN采用和Faster R-CNN相同的两个阶段，具有相同的第一层(即RPN)，第二阶段，除了预测种类和bbox回归，并且并行的对每个RoI预测了对应的二值掩膜(binary mask)用于实例分割。
![](_v_images/20190509192946341_29529.png =660x)
主要提出：
1. Roi Pooling 层替换成了 RoiAlign，使得像素一一对齐；
2. 添加并列的 Mask 层，并认为将mask与分类预测分开处理(decouple)效果更好，即排除类的竞争；
3. 由RPN网络转变成FPN网络 
## RoIAlign
### 从RoIPooling谈起
在faster rcnn中，anchors经过proposal layer升级为proposal，需要经过ROI Pooling进行尺寸归一化后才能进入全连接网络，也就是说ROI Pooling的主要作用是将proposal调整到统一大小。
RoIPool中出现了两次Quantization(个人理解是指小数变整数的离散化过程)。使得池化后的feature map与原来的像素不一一对应。两次量化分别是：
1. 将候选框边界量化为整数点坐标值。从roi proposal到feature map的映射时，取[x/16]，这里x是原始roi的坐标值，而方框代表四舍五入。 
2. 将量化后的边界区域平均分割成 k x k 个单元(bin), 对每一个单元的边界进行量化，每个bin使用max pooling。

![](_v_images/20190509212704384_4895.png =640x)
eg：输入一张800\*800的图片，图片上有一个665\*665的包围框，图片经过主干网络提取特征后，特征图缩放步长（stride）为32。因此，图像和包围框的边长都是输入时的1/32。800正好可以被32整除变为25。但665除以32以后得到20.78，带有小数，于是ROI Pooling 直接将它第一次量化成20。
接下来把框内的特征池化为7\*7的大小，因此将上述包围框平均分割成7\*7个矩形区域。显然，每个矩形区域的边长为2.86，又含有小数。于是ROI Pooling 第二次把它量化到2。
### RoIAlign主要思想
取消量化操作，使用双线性内插的方法获得坐标为浮点数的像素点上的图像数值。
流程：
1. 遍历每一个候选区域，保持浮点数边界不做量化；
2. 将候选区域分割成k x k个单元(bin)，每个单元的边界也不做量化；
3. 在每个单元中计算固定四个坐标位置，用双线性内插的方法计算出这四个位置的值，然后进行最大池化操作。(此时采样点数为4，即把这个单元平均分割成四个小方块以后它们分别的中心点，论文中采样点也设为过1。关于双线性插值见优化技巧中笔记)

![](_v_images/20190509213531302_16139.png =412x)

## 损失函数
$L=L_{c l s}+L_{b o x}+L_{m a s k}$。其中分类和bounding box损失沿用之前的。
$L_{m a s k}$：对于每一个RoI，mask分支定义一个K\*m^2^维的矩阵。表示这m*m的经过池化后的feature map上，对K个不同分类的二元mask。应用逐像素sigmoid，并将L~mask~定义为平均二进制交叉熵损失。对于真值为第i类物体的RoI，L~mask~仅在第i个mask上定义（其他mask输出不会导致损耗）。这样每个类都对用一个mask，算loss的时候只用到特定mask就不会有类之间进行竞争。
其中的二进制交叉熵损失定义如下：(其中out指sigmoid输出，t为目标真值)
```mathjax
$$
cross\_entropy(t, out)=-(t \cdot \log (out)+(1-t) \cdot \log (1-out))
$$
```
## 网络结构
加了一个全卷积网络，除了上采样反卷积是2\*2，stride2，其他都是3\*3的。
![](_v_images/20190509221154495_20244.png =640x)