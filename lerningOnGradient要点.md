### 对于gan的效果好
尝试通过梯度学习的反方向，从图像获得梯度。对于gan模型泛化性比较高。（diffusion的检测有待商榷）
### 模型结构 （详细见论文图2）
一个变换网络，输入图片可以获得梯度。（是一个矩阵）该变换模型的实现多样：Inour experiments, we adopt various popular CNN models to implement the transformation model, including the classifi-
cation model, segmentation model, discriminator of GAN, contrastive learning model, and GAN Inversion。
然后将该矩阵输入一个二维分类器。
### 实验结果：
见文章
### 消融实验：
1. 转换模型的影响(transformation mdoel)：StyleGAN-bedroom的效果最好
2. 数据影响：
   36K效果最好86% 不过9K 的效果已经十分接近 82% 更多数据集不会有更好效果。
作者明确提到，对于非gan效果不好！
# 代码结构分析
#### img2grad 分析：
models文件夹包含所有的generator 和 discriminator架构代码。__init__.py 定义了建立一个model class 的函数。
默认提供代码的model是一个discriminator:
```python
model = build_model(gan_type='stylegan',
    module='discriminator',
    resolution=256,
    label_size=0,
    # minibatch_std_group_size = 1,
    image_channels=3)
model.load_state_dict(torch.load(modelpath), strict=True)
```
批次化处理。读取所有的img文件（各种后缀），搬到cuda。关键步骤：
```python
grad = torch.autograd.grad(pre.sum(), img_cuda, create_graph=True, retain_graph=True, allow_unused=False)[0]
```
grad 形状为：具体通道尺寸可能变化：  
(batch_size, 3, 256, 256)
然后是CNNDetection 就是一个CNN检测网络

### 具体复现：
作者使用的img2grad是基于tensorflow1.x的，然而我尝试在autodl云端部署时遇到问题，显示.Demension的代码已经弃用，我尝试使用docker，然而貌似因为autodl本身在docker上面跑，不能再docker。
