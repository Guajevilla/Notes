# STD:Sparse to Dense
两阶段:第一阶段以每个点为基础预测椭圆anchor，这样主要是想保留每个点的局部信息。对每个产生的提议特征使用PointsPool来将稀疏的内在点特征转化到高维特征空间中去。增加IoU预测支路。