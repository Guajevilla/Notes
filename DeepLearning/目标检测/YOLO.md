# YOLO
&emsp;&emsp;以前的目标检测工作重新利用分类问题中的分类网络来执行检测。而YOLO将目标检测重新看作单一的回归问题，直接从图像像素到边界框坐标和类概率。
&emsp;&emsp;与最先进的检测系统（如R-CNN）相比，YOLO产生了更多的定位误差，但不太可能在背景上预测出假阳性。这主要是因为YOLO在训练期间和测试时会看到整个图像，所以它隐式地编码了关于类的上下文信息，跟不容易在背景上误检。
&emsp;&emsp;YOLO使用单个卷积网络同时预测多个边界框及其类概率。在全图像上训练并直接优化检测性能。且检测速度快。
&emsp;&emsp;YOLO会学到目标的泛化表示。当训练在自然图像上，测试在人工图像上时，YOLO的准确率很高。
## Unified Detection
1. 将输入图像分成$S\times S$的网格。如果一个目标的中心落入一个网格单元中，该网格单元负责检测该目标。
2. 每个网格单元预测$B$个边界框及其置信度分数。置信度分数反映了该模型对BB是否包含目标的信心，以及它认为预测BB的准确程度。在形式上，我们将置信度定义为$\Pr(\textrm{Object}) * \textrm{IOU}_{\textrm{pred}}^{\textrm{truth}}$。如果该单元格中不存在目标，则置信度分数应为零。否则，我们希望置信度分数等于预测框与真实值之间联合部分的交集（IOU）。
3. 每个边界框包含5个预测：$x$，$y$，$w$，$h$和置信度。$(x，y)$坐标表示边界框相对于网格单元边界框的中心，置信度表示预测框与实际边界框之间的IOU。
4. 每个网格单元还预测$C$个条件类别概率$\Pr(\textrm{Class}_i | \textrm{Object})$。每个网格单元只预测一组类别概率，而不管边界框的的数量$B$是多少。
5. 在测试时，将前两项置信度和条件概率相乘，得到每个框中特定类别的置信度分数。这些分数编码了该类出现在框中的概率以及预测框拟合目标的程度。
```mathjax
$$
\operatorname{Pr}\left(\text { Class }_{i} | \text { Object }\right) * \operatorname{Pr}(\text { Object }) * \operatorname{IOU}_{\text { pred }}^{\text { truth }}=\operatorname{Pr}\left(\text { Class }_{i}\right) * \operatorname{IOU}_{\text { pred }}^{\text { truth }}
$$
 ```
6. 最终获得的预测是$S\times S\times(B*5+C)$张量(一共$S\times S$个单元，每个单元$C$个类别概率，$B$个边界框，每个边界框有五个边界框参数)。

 本文为了在Pascal VOC上评估YOLO，使用$S=7$，$B=2$。Pascal VOC有20个标注类，所以$C=20$。最终的预测是$7\times 7 \times 30$的张量。
 ![](_v_images/20190519153916588_5990.png =670x)
## 网络设计
共有24个卷积层，后接两个全连接层。受GoogLeNet启发大量使用1\*1卷积降维。
![](_v_images/20190519111510071_31744.png =660x)
另外还有快速版本的YOLO，区别在于为了加速减少了卷积层数量(9conv)
## 训练
利用前20层卷积层后接全连接层在ImageNet上训练。另外额外加上的4个卷积层和2个全连接层随机初始化。由于定位任务需要更加细腻的信息，将输入图片的分辨率由224提升到448。
&emsp;&emsp;最后一层输出的参数处理比较重要，处理如下：
1. $w$，$h$用原图的$W$，$H$归一化到0-1之间；
2. 边界框$(x，y)$坐标相对于对应的网格归一化到0-1之间；
3. 最后一层使用线性激活函数，其它层用leaky ReLu；
```mathjax
$$
\phi(x) =
\begin{cases}
x, if x > 0 \\
0.1x, otherwise
\end{cases}
$$
```
使用平方和误差计算损失，易于优化。但平方和误差存在一定的问题：
1. 8维的localization error和20维的classification error同等重要显然是不合理的；在每张图像中，许多网格单元不包含任何对象，因此这些单元格的“置信度”分数接近于零，压倒了包含目标的单元格的梯度。这可能导致模型不稳定，从而导致训练早期发散。于是使用参数$\lambda_\textrm{coord}=5$来增加边界框坐标预测损失，和参数$\lambda_\textrm{noobj}=0.5$以减少不包含目标边界框的置信度预测损失。
2. 平方和误差也会平衡大BB和小BB间的加权误差，然而事实上，大盒子小偏差的重要性不如小盒子小偏差的重要性。为了部分解决这个问题，直接预测边界框宽度和高度的平方根。(原因是$\sqrt{w}$函数在[0,1]上，在w比较小的时候变化更剧烈，即连续单增且二次导恒小于0)
3. 一个网格会预测多个bounding box，在训练时我们希望每个object（ground truth box）只有一个bounding box专门负责（一个object 一个bbox）。具体做法是与ground truth的IOU最大的bounding box 负责该ground truth的预测。这种做法称作bounding box predictor的specialization(专职化)。每个预测器会对特定（sizes,aspect ratio or classed of object）的ground true box预测的越来越好。每个预测器可以更好地预测特定大小，方向角，或目标的类别，从而改善整体召回率。

损失函数如下：
```mathjax
$$
\lambda_\textbf{coord}
\sum_{i = 0}^{S^2}
\sum_{j = 0}^{B}
\mathbb{1}_{ij}^{\text{obj}}
\left[
\left(
x_i - \hat{x}_i
\right)^2 +
\left(
y_i - \hat{y}_i
\right)^2
\right]
\\
+ \lambda_\textbf{coord}
\sum_{i = 0}^{S^2}
\sum_{j = 0}^{B}
\mathbb{1}_{ij}^{\text{obj}}
\left[
\left(
\sqrt{w_i} - \sqrt{\hat{w}_i}
\right)^2 +
\left(
\sqrt{h_i} - \sqrt{\hat{h}_i}
\right)^2
\right]
\\
+ \sum_{i = 0}^{S^2}
\sum_{j = 0}^{B}
\mathbb{1}_{ij}^{\text{obj}}
\left(
C_i - \hat{C}_i
\right)^2
\\
+ \lambda_\textrm{noobj}
\sum_{i = 0}^{S^2}
\sum_{j = 0}^{B}
\mathbb{1}_{ij}^{\text{noobj}}
\left(
C_i - \hat{C}_i
\right)^2
\\
+ \sum_{i = 0}^{S^2}
\mathbb{1}_i^{\text{obj}}
\sum_{c \in \textrm{classes}}
\left(
p_i(c) - \hat{p}_i(c)
\right)^2
$$
```
&emsp;&emsp;其中$\mathbb{1}_i^{\text{obj}}$表示目标是否出现在网格单元$i$中，$\mathbb{1}_{ij}^{\text{obj}}$表示网格单元$i$中的第$j$个边界框预测器“负责”该预测。
&emsp;&emsp;如果目标存在于该网格单元中（前面讨论的条件类别概率），则损失函数仅惩罚分类错误。如果预测器“负责”实际边界框（即该网格单元中具有最高IOU的预测器），则它也仅惩罚边界框坐标错误。
&emsp;&emsp;损失函数设计图（图中公式有误，以上式为准）：
![](_v_images/20190520103514022_10909.png =670x)
&emsp;&emsp;学习率在第一个epoch缓慢从$10^{-3}$提高到$10^{-2}$。因为如果从高学习率开始，模型往往会由于不稳定的梯度而发散。继续以$10^{-2}$的学习率训练75个迭代周期，然后用$10^{-3}$的学习率训练30个迭代周期，最后用$10^{-4}$的学习率训练30个迭代周期。
&emsp;&emsp;同时使用dropout和数据增强以避免过拟合。
&emsp;&emsp;一些大的目标或靠近多个网格单元边界的目标可以被多个网格单元很好地定位，使用NMS进行修正。
## 缺点
1. YOLO对相互靠的很近的物体，还有很小的群体 检测效果不好，这是因为一个网格中只预测了两个框，并且只属于同一类。
2. YOLO采用了多个下采样层，网络学到的物体特征并不精细，因此也会影响检测效果。
2. 同一类物体出现的新的不常见的长宽比和其他情况时，泛化能力偏弱。
3. 由于损失函数的问题，定位误差是影响检测效果的主要原因。尤其是大小物体的处理上(用平方根函数并不能完全解决问题)，还有待加强。
4. 与Fast R-CNN相比，YOLO在定位上更不准确，但Fast R-CNN的背景错误比较多（将背景识别为某物体）；于是下一步想到结合YOLO和Fast R-CNN
5. 
结合YOLO和Fast R-CNN：
&emsp;&emsp;对于R-CNN预测的每个边界框，检查YOLO是否预测到一个类似的框。如果是这样，根据YOLO预测的概率和两个bbox之间的重叠来对这个预测进行提升。