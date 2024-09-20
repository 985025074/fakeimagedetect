### 有望通过少量训练实现跨模型鉴别
gan的不适用于diffusion。
为了纠正这种不理想的行为，我们提出使用不专门训练来区分真假图像的特征来进行真伪图像分类。
### 在训练过程中数据增强很重要
以往的一些工作，在训练过程中，使用了复杂的数据增强方案，包括高斯模糊和JPEG压缩，经验表明这些增强对于模型的泛化能力至关重要。
### 在gan上训练的对于识别gan-generated图像的很强，足以区分开diffiusion上的真假图，原因：
作者通过一些处理的频谱分析（, we start by performing a high pass
filtering for each image by subtracting from it its median
blurred image.）可以发现在类gan的频谱图中有一些重复模式。具体见文章，对于频谱的CV方面不是很懂。
### Key Concept
The key, we believe, is that the classification process should
happen in a feature space which has not been learned to
separate images from the two classes.
->new feature space
## 特征选择：
一个$\phi$变换网络.
ϕ应该接触过大量图像.  
选择了一种视觉Transformer的变体ViT-L/14.VIT 忘得差不多了，大概就是切分图片成小块，类似句子。输网络。
## 方法：
最近邻，线性分类。这部分建议阅读原文，很清楚。