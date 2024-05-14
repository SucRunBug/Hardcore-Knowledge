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

### 超分辨率

生成的图片又糊，细节又不好看？直接拉大分辨率又出鬼图？正确的做法是下面的：

AI 基本上无法生成超级大图，想要生成高清图片正确的做法是分辨率调小，比如512x768，然后开启 “高清修复”。

放大倍数请考虑自己的显存大小调整，太大会爆显存。

可以参考图片中的其他参数：
<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240511212719984.png" alt="image-20240511212719984" style="zoom:50%;" />

按照这样，基本上就已经非常高清了。如果你还想继续放大，可以等到出图后点击这里的发送到后期处理，再选择放大算法进行放大。 

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240511212913154.png" alt="image-20240511212913154" style="zoom:50%;" />

## 图生图基础

### 查询图片参数（反推tag）

AI生成图片会自动保存全部参数到原图中，可以在WebUI的 “图片信息” 一栏内通过解析原图查看到。也可以使用我写的这个工具：https://spell.novelai.dev/

非AI生成图片或经过压缩的图片推荐使用 WD14Tagger 来尝试反推tag。

Tagger是一个插件，在新版的整合包内也帮你装好了，可以在顶栏找到。打开后拖入图片，AI会识别出一些tag。如果没有这个插件的话需要你自行安装，安装教程参考下面的插件管理部分。

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240511215906107.png" alt="image-20240511215906107" style="zoom:50%;" />

### 使用ControlNet精细控制画面

为文生图增效，可控制人物姿势、深度（景深），还支持线稿上色。

2023.2.16安装教程1https://www.bilibili.com/video/BV1Wo4y1i77v/?spm_id_from=333.976.0.0&vd_source=86646d951d84826a4640314db345f2aa

>将模型放在 models/ControlNet 里
>2023年10月以后，启动器已支持国内镜像加速预处理器，故不再需要手动安装预处理器

#### 1.0介绍

各种处理方式如下图所示：

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513174235944.png" alt="image-20240513174235944" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513174325843.png" alt="image-20240513174325843" style="zoom:50%;" />

视频里的时间节点还处于测试阶段，要使用需要将WebUI更新到最新版本。

模型安装可以从原项目的hugging face上下载https://huggingface.co/webui/ControlNet-modules-safetensors/tree/main

#### 1.1新功能

由于我实在不想把老的教程给逐字逐句地记录下来，我就直接看最新的1.1教程版本了。

新功能预览如下图：

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513180114066.png" alt="image-20240513180114066" style="zoom:50%;" />

那这样岂不是可以给很多黑白漫画上色了。效果秒杀上面提到的canny，幸亏没再研究旧版本了，不过看完新版本可以再回看一下。

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513180339862.png" alt="image-20240513180339862" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513180457206.png" alt="image-20240513180457206" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513180557824.png" alt="image-20240513180557824" style="zoom:50%;" />

#### 怎么安装并使用？

需要安装的内容：

- 升级ControlNet插件版本（在启动器中升级，如下图所示位置，点击sd-webui-controlnet的左侧的更新，注意更新前需要停止WebUI）

![image-20240513182023546](https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513182023546.png)

- 安装各种预处理器的模型（可选，并且WebUI中已经自带很多种）——本身会在使用的时候自动下载，但可能会出现网络问题

看到这里，我发现我没有下载人家准备好的预处理器，因为根本没分享出来。存放路径里也没找到。但是能正常使用。

存放路径是在extensions/sd-webui-controlnet/annotator

- 安装ControlNet 1.1 模型

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513180649015.png" alt="image-20240513180649015" style="zoom:50%;" />

在webUI的图生图中，功能区（可能需要向下划动）截图如下：

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513181041646.png" alt="image-20240513181041646" style="zoom:50%;" />

建议勾选“完美像素模式”。

对于“控制模式”，选择“更偏向ControlNet”时，需要降低CFG

softEdge预处理对应softEdge模型（以前叫HED）

OpenPose预处理器对应OpenPose模型，Openpose_full模型可以全部搞定手部脸部。

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240514152607758.png" alt="image-20240514152607758" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240514152851319.png" alt="image-20240514152851319" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240514153324809.png" alt="image-20240514153324809" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240514165821532.png" alt="image-20240514165821532" style="zoom:50%;" />

#### 模型命名方式

1.1的模型命名方式如下图：
<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240513181306620.png" alt="image-20240513181306620" style="zoom:50%;" />

模型存放位置：models/ControlNet

#### 给线稿上色

首先在webUI的界面中找到“WD1.4标签器”（版本号可能不一致，也叫Tagger），上传线稿，点击“反推提示词”，再点击“发送到文生图”。

下一步是在跳转到的界面中找到controlnet（可能需要向下滑动），勾选“启用”和“完美像素模式”，并上传之前的线稿，选择预处理器“反色invert”，选择模型lineart_anime进行上色。

最后选择想要的SD模型和尺寸等。

前两次带有黑白标签，生成黑白色彩的图片，去掉如lineart、greyscale和monochrome等标签后，第三次可以生成彩色图片。

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/截屏2024-05-14 15.56.42.png" alt="截屏2024-05-14 15.56.42" style="zoom:50%;" />

#### Pix2Pix

前面提到过的局部改变天气的新功能。

这个非常吃底模，某些模型可能不会生效。

提示词需要写指令，例如make it night变为夜晚。同时，可以也增加一些夜晚的tag。

这个需要调低CFG，低到5以下。

#### Tile模型

神必特性：

- 忽略图像中的细节并生成新的细节
- 如果局部的内容与全局提示词不匹配，则忽略掉提示词，根据周围的图片尝试去推断局部的内容

带来的效果：

- ﻿﻿图生图的功能
- ﻿﻿让画面更好地融合的功能（比如P上去一个物品，Tile可以推断周围融合）
- ﻿﻿增加细节的功能
- ﻿﻿如果你直接拉大分辨率再用Tile，那他就可以有放大图片的功能
- ﻿﻿配合其他图片放大器（例如后处理里面的放大，可以很好的修复因为放大导致的细节问题）

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/截屏2024-05-14 16.42.16.png" alt="截屏2024-05-14 16.42.16" style="zoom:50%;" />

#### OpenPose

建议使用时一定要上传真人的图片。

<img src="https://raw.githubusercontent.com/SucRunBug/img_bed/main/image-20240514165550531.png" alt="image-20240514165550531" style="zoom:50%;" />

#### 三视图

将下载好的文件解压，将其中的lora模型charturnbetalora（后缀SAFETENSORS），放置于models\Lora\下，然后启动webUI

启用controlnet，加载三视图的骨骼图，预处理器不用动，模型选择control_openpose，提示词为

```bash
masterpiece, best quality, 1girl, simple background, (white background:1.5), multiple views,  

# 中间写自己想要的tag
# 例如：
thighhighs,gloves,bow-shaped hair,bare shoulders,shorts,navel,cleavage,long hair,pink hair,bangs,purple eyes,

<lora:charturnbetalora:0.2>
```

启用高清修复，重绘幅度推荐0.6

对于三视图，分辨率需要高度小，宽度大，推荐704x320

还可以不断调整lora的权重，推荐0.2-0.6