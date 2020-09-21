# algorithm
## 运行时间
与以下有关：
1. 输入（如排序问题中顺序输入和随机输入）
2. 输入的大小规模
一般我们还想知道算法运行的upper bound，运行时间上限。
## 算法分析
最坏情况分析 T(n) = max time on any input of size n
平均情况分析 T(n) = expected time over all inputs of size n （需要关于输入分布概率的假设）
最好情况分析（bogus假象） T(n) = min time on any input of size n

渐进分析asymptotic analysize 关注随输入规模增大，运行时间的增长
## 渐进分析Asymptotic notation
\Theta：忽略所有低次项和常数项
随着输入规模趋近于无限大，次数越大的算法，运行时间一定比次数小的算法慢，但在一定输入范围内，由于低次项的影响，有可能次数大的算法运行更快。