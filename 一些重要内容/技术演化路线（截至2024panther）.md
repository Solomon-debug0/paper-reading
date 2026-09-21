传统 MIL
        ↓
Deep Sets / Set Representation
        ↓
Attention MIL
        ↓
病理中的 Prototype / Cluster MIL
        ↓
AttnMISL
        ↓
无监督 WSI representation
       ↙             ↘
     H2T              HPL
 deep feature       proportion
       ↘             ↙
      Prototype-based set representation
              ↓
       Optimal Transport / OTK
              ↓
       Differentiable EM / DIEM
              ↓
             PANTHER

$\boxed{ \text{Dietterich MIL} }$
解决：
> 一个 bag 有很多 instance，但只有 bag label 怎么办？

$\boxed{ \text{Deep Sets} }$
解决：
> variable-size unordered set 怎么变成 vector？

$\boxed{ \text{Attention MIL} }$
解决：
> 不同 instances 重要性不同怎么办？

$\boxed{ \text{AttnMISL} }$
解决：
> 先按 morphology-like clusters 分组再聚合（得到feature）会不会更好？

$\boxed{ \text{H2T} }$
解决：
> 能不能不依赖 task label，直接构建 prototype-based WSI embedding？

$\boxed{ \text{HPL} }$
强调：
> 能不能无监督发现 morphology，并量化它们的比例？

$\boxed{ \text{OTK} }$
提供：
> variable set 与 fixed prototypes 如何 soft matching？

$\boxed{ \text{DIEM} }$
提供：
> 能不能把整个 set 当作 mixture distribution，并用 EM 参数表示这个 set？

最终：
$\boxed{ \text{PANTHER} }$
> **那就把一张 WSI 看成若干 morphological distributions 的 mixture。**
于是：
$\boxed{ WSI \approx \left\{ (\pi_1,\mu_1,\Sigma_1), \dots, (\pi_C,\mu_C,\Sigma_C) \right\} }$

### 额外部分的详细解释
**H2T**（Handcrafted Histological Transformer）
保存prototype中的patch feature长什么样（description）
**HPL**（Histomorphological Phenotype Learning）
每种phenotype有多少
于是出现了是否能设计出一个统一的表达式，保存”有多少“和“长什么样”

**OTK**（Optimal Transport Kernel Embedding​）
和K-means找最近的prototype不一样，OTK是将整个patch set做一次全局匹配，让**总的运输成本更低**，第一版的prototype得到的方式**1.K-means**；2.将prototype当作训练参数然后采用**神经网络**得到，但后续的**分配迭代过程**则是由**OTK**进行
但由于是根据整体来分配迭代，导致把每个prototype应该接受多少patch预先规定的过于平均（每个patch提供相同的”质量“1/N，且每个prototype接受相同的”质量“1/K）（**balanced assignment**），但每个**小小的difference**在病理中却是十分关键的（HPL保留了abundance，在这方面是优势的）

于是后面有了**GMM**
将属于这个morphology的patch**有多少，平均长什么样，内部变化多大**统一
其中对于patch assignment也比较美观
patch xi属于prototypec的概率![[Pasted image 20260919160402.png|123]]
soft assignment![[Pasted image 20260919160243.png|173]]


#### 这里panther几乎就是
H2T+HPL+DIEM/GMM-style set modeling+pathology morphology​



                一张 WSI = 大量 patch 的 SET
                           │
                           ↓
              怎么把 set 压缩成固定表示？
                           │
              ┌────────────┴────────────┐
              │                         │
          H2T 思想                  HPL 思想
              │                         │
       prototype 内部                prototype
       “长什么样？”                 “有多少？”
              │                         │
       deep feature                 proportion
              │                         │
              └────────────┬────────────┘
                            ↓
             Prototype-based Set Representation
                            │
             一个 prototype 应描述：
                            │
              ┌─────────────┼─────────────┐
              ↓             ↓             ↓
         Description    Cardinality    Variation
              ↓             ↓             ↓
             μ              π             Σ
              └─────────────┬─────────────┘
                            ↓
                通用 set 表示方法
                     OTK / DIEM
                     ↓       ↓
                matching   mixture model
                         \   /
                          ↓
                       PANTHER
                          ↓
             pathology-specific GMM prototypes