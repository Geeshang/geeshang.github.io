# PyTorch实现Mask R-CNN笔记
## RoIAlign实现细节
1. 使用bilinear interpolation得到采样点的值

参考：Jaderberg et al. Spatial transformer networks

## 网络架构

### backbone
1. 最好的结果来自使用ResNet-FPN 

## loss
loss = l_box + l_cls + l_mask

l_mask = 

l_box l_cls 参考： Girshick. Fast R-CNN

参考: Lin et al. Feature pyramid networks for object detection

## Training
1.  mask target是RoI与ground-truth mask的交集像素
2. 短边800像素，minibatch 2 / GPU，512 RoI for backbone ResNet-FPN 正负比例1:3， lr=0.02 一段时间后除以10

## Inference
1. resion proposal 1000 for backbone FPN
2. box prediction之后运行non-maximum suppression，去最高的100个送给mask head
3. m x m像素的mask resize to RoI size