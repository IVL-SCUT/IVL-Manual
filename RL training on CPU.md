# RL training on CPU

`# 2024.03.10-7 10:52`

在以往的观察中发现，RL训练在CPU的速度显著快于GPU，这引导了对新工作站硬件的选择。为了加快训练速度，我们购买了一台搭载两颗AMD EPYC 7Y43的工作站，每颗CPU有48核心96线程，总共96核心192线程。为了验证这一点，我们使用了 [OmniSafe](https://github.com/PKU-Alignment/omnisafe) 做了一些测试。结果如下：

```shell
# create a new conda environment
conda create -n test python=3.10
conda activate test
# install omnisafe
pip install omnisafe
# make a directory for the test
mkdir test && cd test
# use 16 threads with CPU
omnisafe train --algo PPO --total-steps 100000 --torch-threads 16
# use 1 threads with GPU
omnisafe train --algo PPO --total-steps 100000 --torch-threads 1 --device cuda:0
```

| CPU               | GPU        | Device | Thread | Time [s] |
| :---------------- | :--------- | :----- | :----- | -------: |
| EPYC 7Y43\@2.3GHz | RTX 3090   | CPU    | 1      |       77 |
|                   |            |        | 8      |       96 |
|                   |            |        | 16     |      103 |
|                   |            |        | 96     |      133 |
|                   |            |        | 192    |      196 |
|                   |            | GPU    | 1      |      198 |
| R9 5950X\@3.4GHz  | RTX 3080Ti | CPU    | 1      |       61 |
|                   |            |        | 16     |       72 |
|                   |            | GPU    | 1      |      149 |

根据上表，有几点结论：

1. 使用CPU训练速度快于GPU。
2. 线程数越少，训练速度越快。
3. 基准频率对训练速度的提升不是线性的。

关于多线程更慢的问题，根据网上的一些资料，推测可能是对于我们的强化学习训练场景，计算负载较低，多线程会导致线程切换的开销超过计算开销。

- [Model is getting slower on CPU when I increased the number of threads](https://discuss.pytorch.org/t/model-is-getting-slower-on-cpu-when-i-increased-the-number-of-threads/68942)
- [Pytorch is slower when with multiple threads ( on CPU)](https://discuss.pytorch.org/t/pytorch-is-slower-when-with-multiple-threads-on-cpu/55583)

`# 2024.03.18-1 09:24`
我们重新购置了一台搭载单路Intel i9-14900K的工作站，CPU有24(8+16)核心32线程。再次进行测试。

| CPU               | GPU      | Device | Thread | Time [s] |
| :---------------- | :------- | :----- | :----- | -------: |
| i9-14900K\@3.2GHz | RTX 3090 | CPU    | 1      |       37 |
|                   |          |        | 8      |       37 |
|                   |          |        | 16     |       50 |

由此可见，就OmniSafe框架而言，单线程明显快于多线程，有时间应多测试一些其他框架得到更一般的结论。
