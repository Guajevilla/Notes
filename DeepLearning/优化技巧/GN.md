# GN
## What's wrong with BN？
Batch Normalization，是一种归一化方式，而且是以batch的维度做归一化，那么问题就来了，此归一化方式对batch是独立的，过小的batch size会导致其性能下降，一般来说每GPU上batch设为32最合适。  
但是对于一些其他深度学习任务batch size往往只有1-2，比如目标检测，图像分割，视频分类上，输入的图像数据很大，较大的batchsize显存吃不消。
![101604sr3tllljtja6t5zu](_v_images/20190317105734597_2549.jpg =540x)  
## How GN work
BN是将特征图沿每个channel做的归一化，然而事实上，不同的channel间并不是完全独立的，某两个不同的channel相关的可能性仍很高。传统的SIFT，HOG，GIST等方法都是分组提取，而每组channel都是由某种直方图构建而成的，由此启发想到了GN。

![](_v_images/20190317105957524_1479.png =540x)  
深度网络中的数据维度一般是[N, C, H, W]或者[N, H, W，C]格式，压缩H/W至一个维度，其三维的表示如上图。  
BN是沿着channel方向求均值方差进行的归一化，即共享同一通道的所有像素一同归一化（计算(N,H,W)轴的均值）；
LN沿batch方向，同一batch计算(C,H,W)的均值；
IN对每个样本和通道计算(H,W)的均值；
在GN中，G表示组数，C//G即每组样本数量，相当于reshape为[N, G, C//G, H, W]的新张量，沿(C//G, H, W)求均值。如上图便是分为2组，每组3通道。
## 算法表示
![](_v_images/20190317150346919_17590.png =576x)  
其中G一般取32