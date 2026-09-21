### 仍然存在的问题
1.官方说的两个问题
**域偏移**（domain shift）时，所有的patch会被分配到一个prototype中
其中的phenotype是用K-means固定好的，当新的domain是在特征空间的另一块时，整体所有patch可能被分配到一个phenotype中。
**原文中的morphology的数量C固定**，不同患者可能需要不同的数量的phenotype
2.pather虽然可以画回WSI得到heatmap，但representation本质还是GMM statistics，说明不了在哪和在哪边附近
3.panther会弱化rare phenotype，例如忽略micrometastasis patch（扩散的）
4.原文中采用的GMM矩阵是对角的，只保留方差忽略cov的
5.MMP（已经有人做出来了），我们研究的是WSI-morphological prototype，以此延展出了RNA，histology的
6.26WSIsum给panther提供另一种思路