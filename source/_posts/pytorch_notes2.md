---
title: PyTorch DataSet 与 DataLoader
date: 2020-07-06 16:25:53
tags:
  - PyTorch
categories:
  - Machine Learning
  - Deep Learning 
---

读一下 PyTorch 的数据导入部分的源码，因为自己如果做自己的数据集，这部分是必须的。

参考： [PyTorch中文手册](https://pytorch-cn.readthedocs.io/zh/latest/)

## DataSet 类
```python
class torch.utils.data.Dataset
```
源码部分

```python
class Dataset(object):
    """An abstract class representing a Dataset.

    All other datasets should subclass it. All subclasses should override
    ``__len__``, that provides the size of the dataset, and ``__getitem__``,
    supporting integer indexing in range from 0 to len(self) exclusive.
    """

    def __getitem__(self, index):
        raise NotImplementedError

    def __len__(self):
        raise NotImplementedError

    def __add__(self, other):
        return ConcatDataset([self, other])
```

这是所有数据集的父类，所有其他数据集都应该进行子类化，所有子类都需要自定义(override) `__len__`和`__getitem__`，前者提供数据集大小，后者支持整数索引。在最新版的PyTorch里`__len__`变成了选做。

以DeepGlobe数据集构建DataSet为例：

```python

class DeepGlobe(data.Dataset): # 声明类为DataSet类子类
    """input and label image dataset"""

    def __init__(self, root, ids, label=False, transform=False):
        super(DeepGlobe, self).__init__()
        """
        Args:
        root:  directory with all the input images.
        transform(callable, optional): Optional transform to be applied on a sample
        """
        self.root = root
        self.label = label
        self.transform = transform
        self.ids = ids
        self.classdict = {1: "urban", 2: "agriculture", 3: "rangeland", 4: "forest", 5: "water", 6: "barren",
                          0: "unknown"}

        self.color_jitter = transforms.ColorJitter(brightness=0.3, contrast=0.3, saturation=0.3, hue=0.04)
        self.resizer = transforms.Resize((2448, 2448))

    def __getitem__(self, index):   # 获取数据集中的某对图片或者NLP中的某个片段
        sample = {}   # 字典，包含三对  id, image, label 
        sample['id'] = self.ids[index][:-8]
        image = Image.open(os.path.join(self.root, "Sat/" + self.ids[index]))  # w, h
        sample['image'] = image
        # sample['image'] = transforms.functional.adjust_contrast(image, 1.4)
        if self.label:
            label = Image.open(os.path.join(self.root, 'Label/' + self.ids[index].replace('_sat.jpg', '_mask.png')))
            sample['label'] = label
        if self.transform and self.label:
            image, label = self._transform(image, label)
            sample['image'] = image
            sample['label'] = label
        # return {'image': image.astype(np.float32), 'label': label.astype(np.int64)}
        return sample

    def _transform(self, image, label): # 定义一个数据增强方法
        if np.random.random() > 0.5:
            image = transforms.functional.hflip(image)
            label = transforms.functional.hflip(label)

        if np.random.random() > 0.5:
            degree = random.choice([90, 180, 270])
            image = transforms.functional.rotate(image, degree)
            label = transforms.functional.rotate(label, degree)

        return image, label

    def __len__(self):    # 返回长度
        return len(self.ids)
```

## DataLoader 类
DataLoader 导入数据集，源码太长不放源码了，看一下定义
```python
CLASS torch.utils.data.DataLoader(dataset, batch_size=1, shuffle=False, 
sampler=None, batch_sampler=None, num_workers=0, collate_fn=None, pin_memory=False, drop_last=False, 
timeout=0, worker_init_fn=None, multiprocessing_context=None)
```

`dataset` 为之前自定义的 DataSet 类实例化之后数据接口
`batch_size`，批训练量
`shuffle(bool, optional)` 是否打乱数据
`sampler(Sampler, optional)` 从数据集中提取样本的策略，如果指定，`shuffle`必须为False，一般默认不指定。
`batch_sampler(Sampler, optional)`和batch_size\shuffle等参数互斥，一般不指定，采用默认
`num_wokers`这个参数必须大于0，表示通过多个进程来导入数据
`collate_fn(callable, optional)`合并样本清单以形成小批量，用来处理不同情况下的输入dataset的封装，一般采用默认即可
`pin_memory(bool,optional)`： 数据加载器将张量复制到CUDA内存中，然后返回它们，也就是一个数据拷贝问题。
`drop_last(bool, optional)`: 如果数据集数目不是batch_size的整数倍，那么最后一个batch不是一个完整的batch，是否把最后一个batch删掉，默认false
`timeout(numeric, optional)`设置数据读取超时，如果超过这个时间还没有找到数据的话就会报错。一般不用。
`worker_init_fn(callable, optional)`每个worker的初始化函数，默认为`None`，一般也用`None`
`multiprocessing_context`基于进程的并行，新出的功能，目前资料不多，一般也用`None`。

