# PyramidNet
This repository contains the code for the paper "Deep Pyramidal Residual Networks" (https://arxiv.org/abs/1610.02915). 


The code is based on Facebook's implementation of ResNet (https://github.com/facebook/fb.resnet.torch).

## Abstract
 Deep convolutional neural networks (DCNNs) have shown remarkable performance in image classification tasks in recent years. Generally, deep neural network architectures are stacks consisting of a large number of convolution layers, and they perform downsampling along the spatial dimension via pooling to reduce memory usage. At the same time, the feature map dimension (i.e., the number of channels) is sharply increased at downsampling locations, which is essential to ensure effective performance because it increases the capability of high-level attributes. Moreover, this also applies to residual networks and is very closely related to their performance. In this research, instead of using downsampling to achieve a sharp increase at each residual unit, we gradually increase the feature map dimension at all the units to involve as many locations as possible. This is discussed in depth together with our new insights as it has proven to be an effective design to improve the generalization ability. Furthermore, we propose a novel residual unit capable of further improving the classification accuracy with our new network architecture. Experiments on benchmark CIFAR datasets have shown that our network architecture has a superior generalization ability compared to the original residual networks.

<img src="https://cloud.githubusercontent.com/assets/22743125/19235579/7e7e33c6-8f2d-11e6-9397-1b505688e92a.png" width="960">

Figure 1: Schematic illustration of (a) basic residual units, (b) bottleneck, (c) wide residual units, and (d) our pyramidal residual units. 

<img src="https://cloud.githubusercontent.com/assets/22743125/19235610/bb3d5fd0-8f2d-11e6-84bd-46c9b7a4797a.png" width="640">

Figure 2: Visual illustrations of (a) additive PyramidNet, (b) multiplicative PyramidNet, and (c) comparison of (a) and (b).

## Usage

0. Install Torch (http://torch.ch) and ResNet (https://github.com/facebook/fb.resnet.torch).
1. Add the files addpyramidnet.lua and mulpyramidnet.lua to the folder "models".
2. Change the learning rate schedule in the file train.lua: "decay = epoch >= 122 and 2 or epoch >= 81 and 1 or 0" to "decay = epoch >= 225 and 2 or epoch >= 150 and 1 or 0".
3. Train our PyramidNet, by running main.lua as below:

To train additive PyramidNet-110 (alpha=16) on CIFAR-10 dataset:
```bash
th main.lua -dataset cifar10 -depth 110 -nEpochs 300 -LR 0.5 -netType addpyramidnet -batchSize 128 -shareGradInput true
```
To train multiplicative PyramidNet-110 (alpha=1.68) with 4 GPUs on CIFAR-100 dataset:
```bash
th main.lua -dataset cifar100 -depth 110 -nEpochs 300 -LR 0.5 -nGPU 4 -nThreads 8 -netType mulpyramidnet -batchSize 128 -shareGradInput true
```

##Results

Top-1 error rates on CIFAR-10 and CIFAR-100 datasets.  "alpha" denotes the widening factor; "add" and "mul" denote the results obtained with additive and multiplicative pyramidal networks, respectively.

| Network                           | # of parameters |  CIFAR-10   |  CIFAR-100  |
| --------------------------------- | --------------- | ----------- | ----------- |
| PyramidNet-110 (mul), alpha=1.68  | 1.7M            | 4.62        | 23.16       |
| PyramidNet-110 (add), alpha=16    | 1.7M            | 4.62        | 23.31       |
| PyramidNet-110 (mul), alpha=2     | 3.8M            | 4.50        | 20.94       |
| PyramidNet-110 (add), alpha=28    | 3.8M            | 4.27        | 20.21       |
| PyramidNet-110 (mul), alpha=3     | 28.3M           | 4.06        | 18.79       |
| PyramidNet-110 (add), alpha=90    | 28.3M           | **3.77**    | **18.29**   |

##Notes

0. The parameter "alpha" can only be changed in the files addpyramidnet.lua and mulpyramidnet.lua (Line 28).
1. We recommend to use multi-GPU when training additive PyramidNet with alpha=90 or multiplicative PyramidNet with alpha=3.  Otherwise you may get "out of memory" error.
2. We are currently testing our code in the ImageNet dataset.  We will upload the result when the training is completed.

## Contact
Dongyoon Han (dyhan@kaist.ac.kr),
Jiwhan Kim (jhkim89@kaist.ac.kr),
Junmo Kim (junmo.kim@kaist.ac.kr)
