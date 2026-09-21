先看懂病理再去解决具体病理任务
input一个patch x
ouput一个embedding z=f（x）

注意：pathology FM不一定是WSI FM
**第一类patch-level FM**
解决一个patch如何表现得更好
传统的pipeline：WSI-patch-FM-patchbeddings-MIL-predict

**第二类WSI-level FM**
EX. Prov-GigaPath：WSI-tiles-tile encoder-slide encoder

## 性质（贵）
1.大规模训练
2.task-agnostic（无任务限制）
不只能完成某一个任务
3.transferability：在原有的FM基础上加入100patient可以完成新的任务

### 训练方式
1.self-supervised learning
2.MAE（masked autoencoder）：将部分patch遮住，让模型恢复剩下的内容（让模型理解上下文关系）
3.Vision-Language Foundation Model：WSI+pathologyreport
类似**CLIP**（Contrastive Language-Image Pre-training图像 - 文本对比学习基础模型）

### 为什么其用于处理rare phenotype效果好
传统的小task-specific model可能都没见过micrometasis
但FM已经看过数十亿patch，所以不是从0开始学习某个模型（注意：**看过不代表学过**）