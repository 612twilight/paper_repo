+ Joint entity recognition and relation extraction as a multi-head selection problem

联合关系抽取模型，NER任务使用bilstm+CRF，关系抽取转为一个多头选择问题，即将句子中的每个token与其余的token做sigmod，得到一个多标签分类的结果，每个结果代表了这两个token的关系，可以允许两个token有多个关系。为了简化运算，实体对的关系loss只去各实体最后一个token计算得到的关系loss

缺点是计算代价依然非常大。

+ Joint Extraction of Entities and Relations Based on a Novel Tagging Scheme

统一了实体和关系标注框架，直接以关系标签进行BIOES标注。head实体序号为1，tail实体序号为2；

比如B-CP-1,I-CP-1,I-CP-1    B-CP-2,I-CP-2 表示了前一个实体和后一个实体存在CP关系，subject和object用1和2来指代

这里其实和SPO同意标注框架一致，可以认为是先驱。

存在问题：
不能关系重叠问题，比如一个实体存在于多种关系中的情况。这是一个致命的bug。

+ Extracting Multiple-Relations in One-Pass with Pre-Trained Transformers

pipline模式，实体是事先给出的，这里可以允许识别多个关系，可以使用one-pass预测出所有的关系

简单来说，将实体对进行拼接，然后接入全连接层进行关系识别，同时提出entity-aware的技巧，

不过这里能够识别的最多的关系是提前给定的，超过的就不能识别了

+ Joint Extraction of Entities and Overlapping Relations Using Position-Attentive Sequence Labeling

这个框架也是联合框架，最终是给每个位置建立了一个标注结构，每个标注结构代表了一个位置，比如某个位置有实体，那么该位置会有BIO标注，同时其余位置与其有关系的实体也有BIO标注，与该位置实体没关系的不会给出。

其具体结构如下：

首先使用RNNs去获取每个位置的表征，这里也加入了cha-level级别的表征，然后再表征的基础上增加了一个 position-aware 的attention去给各位置进行表征。