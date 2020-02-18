---
title: DeepGlobe Land Cover Classification Challenge 土地利用分类竞赛笔记
date: 2019-11-19 18:37:27
tags:
  - Machine Learning
  - Image Segmentation
  - TensorFlow
  - Deep Learning
  - Computer Vision
categories:
  - Machine Learning
  - Deep Learning
mathjax:
---
## 如何复现此工作

### 文件结构组织


* deepglobe_land
  * dataset
    * land_train  (存放下载下来的原始数据 )
    * onechannel_label (内部数据通过运行 rgb2label.py 生成)
    * voc_train_all.record (通过运行 create_tf_record_all.py 生成 )
  * ini_checkpoints
      * resnet_v2_101  (resnet_v2_101.ckpt and train.graph)
  * utils (工具包)
  * rgb2label.py
  * create_tf_record_all.py
  * deeplab_model.py


2. **create_tf_record_all.py**  建立 tfrecord 文件，该代码执行后，直接将 land_train文件夹里的所有 id_sat.jpg 文件与 onechannel_label 里的 id_label 文件打包生成一个 voc_train_all.record文件，该文件中包含了数据集和验证集，不需要手动区分数据集验证集，训练过程中会随机选择一部分作为数据集，另一部分作为验证集。

3. **deeplab_model.py**  deeplab模型基本不用动

4. **train.py** 需要在里面进行一些参数设置，类别，训练集合验证集的数据比，这里我们设置训练集723张图片，验证集80张。

### 代码执行顺序与注意事项

1. **reg2label.py** 数据标注文件 id_mask.png 是 RGB 三通道图像，输入训练集的时候必须首先转为单通道图像，故该代码用于执行，图像到单通道图像的转换，此处原始卫星图像 id_sat.jpg, 标注图像 id_mask.png，单通道标注图像 id_label.png 。  **这段代码可以作为一段工具代码在其他地方使用。**

   代码中
   ```python
   one_channel_label_path = 'dataset/land_train'
   test_mask_path = 'dataset/onechannel_label'
   ```
   这两行指定了输入输出路径。


2. **create_tf_record_all.py**  建立 tfrecord 文件，该代码执行后，直接将 land_train文件夹里的所有 id_sat.jpg 文件与 onechannel_label 里的 id_label 文件打包生成一个 voc_train_all.record文件，该文件中包含了数据集和验证集，不需要手动区分数据集验证集，训练过程中会随机选择一部分作为数据集，另一部分作为验证集。
    ```python
    parser.add_argument('--data_dir', type=str, default='./dataset/',
                        help='Path to the directory containing the PASCAL VOC data.')

    parser.add_argument('--output_path', type=str, default='./dataset',
                        help='Path to the directory to create TFRecords outputs.')

    parser.add_argument('--image_data_dir', type=str, default='land_train',
                        help='The directory containing the image data.')

    parser.add_argument('--label_data_dir', type=str, default='onechannel_label',
                        help='The directory containing the augmented label data.')
    ```

    这一段设置了默认的输入输出路径，如需修改可直接改源代码，或在命令行指定参数。



3. **train.py** 需要在里面进行一些参数设置，类别，训练集合验证集的数据比，这里我们设置训练集723张图片，验证集80张。
    **参数解释**
    **epoch**:
    > In Deep Learning, an epoch is a hyperparameter which is defined before training a model. One epoch is when an entire dataset is passed both forward and backward through the neural network only once.

    1个 epoch 意味着所有数据通过神经网络一遍
    **batch_size**:
    > A batch is the total number of training examples present in a single batch and an iteration is the number of batches needed to complete one epoch.

    训练数据不可能一次性全部通过神经网络，所以需要分为不同的 batch。batch_size 大小将影响占用内存的数量。
    **iteration**:
    > If we divide a dataset of 2000 training examples into 500 batches, then 4 iterations will complete 1 epoch

    迭代几个循环

    **max_iter**:

    最大迭代次数，也是训练停止的条件。

    **learning rate**:
    > Learning rate might be the most important hyper parameter in deep learning, as learning rate decides how much gradient to be back propagated.

    学习率，决定模型收敛速度的一个重要超参数，学习率大，下降快，但可能陷入震荡，学习率小，下降慢，却容易获得收敛。
    ![学习率](https://i.loli.net/2019/11/20/jpqVeSfFwrgkBO5.png)

    **learning_rate_policy**:
    学习率函数，可选参数为 poly(polynomial function 多项式函数), piece wise





### 支撑代码
1. **utils** 自定义的 package，包含两个工具文件


2. **deeplab_model.py**  deeplab模型基本不用动
    函数 **atrous_spatial_pyramid_pooling**:空洞卷积空间金字塔池化
    atrous_rate 空洞卷积膨胀率，空洞卷积的核心指标。

    函数 **deeplab_v3_plus_generator** deeplabv3+ 网络生成器

    函数 **deeplabv3_plus_model_fn** 模型方程



---
## 致谢
- [rishizek's repo tensorflow-deeplab-v3-plus](https://github.com/rishizek/tensorflow-deeplab-v3-plus)
- [DeepGlobe_2018_A_CVPR_2018_paper](http://openaccess.thecvf.com/content_cvpr_2018_workshops/w4/html/Demir_DeepGlobe_2018_A_CVPR_2018_paper.html)
- [deepglobe](http://deepglobe.org/).
