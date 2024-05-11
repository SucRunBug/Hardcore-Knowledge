2024.5.11

- 本机配置

CPU i5 12400f 6核12线程

GPU Nvidia GeForce 4060ti 显存 16GB

内存 32GB DDR4 频率3200

硬盘 2TB

# 入门基础

AI绘图无法做到：无法根据一个立绘，画出各种动作角度。one shot？

训练模型：人物学习最低10张，画风学习最低50张。

## 文生图基础

由文字描述直接生成图片

### 界面描述

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240511192532647.png" alt="image-20240511192532647" style="zoom:50%;" />

### Tag

正面Tag：masterpiece, best quality

反面Tag：lowres, bad anatomy, bad hands, text, error, missing fingers, extra digit, fewer digits, cropped, worst quality, low quality, normal quality, jpeg artifacts, signature, watermark, username, blurry

整合包内一般都会带一个自动补全 Tag 的插件，如果你不知道那些 Tag 好，可以使用标签超市https://tags.novelai.dev/ 

另外，你可能会看到别人发的 Tag 里面会有一些符号？比如大小括号等等。这属于进阶用法，这里仅仅简单提及一下。以 girl 这个 Tag 作为例子。

(girl) 加权重，这里是1.1倍。括号是可以叠加的，如（(girl)) 加很多权重。1.1*1.1=1.21倍

[girl] 减权重，一般用的少。减权重也一般就用下面的指定倍数。

(girl:1.5) 指定倍数，这里是1.5倍的权重。还可以 (girl:0.9) 达到减权重的效果

### 其他参数

**采样步数**不需要太大，一般在50以内。通常28是一个不错的值。

**采样器**没有优劣之分，但是他们速度不同。全看个人喜好。推荐的是图中圈出来的几个，速度效果都不错

**提示词相关性（CFG Scale）**代表你输入的 Tag 对画面的引导程度有多大，可以理解为 “越小AI越自由发挥”。太大会出现锐化、线条变粗的效果。太小AI就自由发挥了，不看 Tag 

**随机种子**是：生成过程中所有随机性的源头。每个种子都是一幅不一样的画。默认的 -1 是代表每次都换一个随机种子。由随机种子，生成了随机的噪声图，再交给AI进行画出来。

Q&A：我用文生图的时候想要生成一男一女，可结果不是动作反了就是服装反了，更有甚者男的长出胸部来了，请问这种问题up是怎么解决的啊？

有个插件叫latent couple，也可以用更高级的comfyui自己解决