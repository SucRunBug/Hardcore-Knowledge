参考链接：

https://openai.com/index/sora/ （OpenAI官方的报告，香港节点进不去，实测新加坡节点可以）
https://blog.mlq.ai/openai-sora-overview/ 
https://labelyourdata.com/articles/explaining-openai-sora（这篇博客中有很多示例图可以使用）
https://www.woshipm.com/aigc/6015159.html 这是一篇sora团队专访

参考视频：

https://www.bilibili.com/video/BV1Um411S7SZ/（genji讲的sora，比较泛）

https://www.bilibili.com/video/BV14F4m1c7RC*（批注：这篇视频特别好，讲的原理特别细致，采访了专业人员，目前最后的内容在进度条14分钟左右）*

https://www.bilibili.com/video/BV1aC411a7pp/（剪辑素材）

## 标题：

Sora正式发布！视频生成的iPhone时刻来了！（GPT3时刻来了）

中国版的Sora还需多久？

国产AI视频生成能打过Sora吗？

Sora来了！对哪些专业有影响呢？AI时代我们怎么才能不会被AI背刺，而是利用好AI，让AI成为自己的伙伴

如何制作出山海经里的画面？

## 提问

### 事物

> sora是什么？干什么用的？应用场景？整活场景？

sora是OpenAI推出的文字生成视频的系统

宣称可以<u>最多生成60秒的视频</u>（[来源](https://x.com/OpenAI/status/1758192957386342435)）

sora受日语“天空”的启发，该模型的名字反映了其无限创造力的潜力。

首先致敬传奇吃面王威尔史密斯https://www.youtube.com/watch?v=XQr4Xklqzw8，连一刻也没有为他哀悼，立刻赶到战场的是Sora

https://www.youtube.com/watch?v=TRIrr3Aar44 进度条6分04

Sora模型最突出的地方是，它能够理解和模拟真实的物理世界，并理解物理动力学和美学。

现状：经过了“红色团队”的测试与纠错，sora马上就能上线了。

应用场景：

- 自然语言生成视频

输入简单的文本描述，比如“在沙滩上散步的男人”， AI 会理解文本中的内容、场景和意图，生成高度一致的短视频。（OpenAI油管上有很多示例，素材可以选那上面的）

- 结合图像生成技术

Sora 使用了类似 DALL-E 和其他生成模型的技术，可以生成高质量的场景、人物和物体，并将这些静态图像合成到流畅的视频中

### 技术

> sora背后有哪些技术？又是如何训练出来的？有哪些特性？

OpenAI的 **Sora** 文本生成视频模型基于先进的机器学习架构，如 **Transformer** 和 **扩散模型（Diffusion Model）**，能够从文本提示生成连贯的视频。

模型的生成过程采用 **扩散式生成**：从一开始带有噪声的图像逐步去噪，使图像逐渐逼近文本描述。Sora 类似于 OpenAI 的 DALL-E 图像生成模型，将视频帧分解成“视觉块”（visual patches），类似语言模型的“词汇单元”，这样既保证了视频的高质量输出，同时能处理各种长度、分辨率和比例的视频。

- 时序一致性

生成视频最大的挑战之一是要保持连续帧之间的时序一致性，使动作流畅。Sora 通过改进扩散模型中的时间建模，使帧与帧之间能够根据时间顺序合理变化，确保物体和背景元素在帧序列中的位置与姿态逐渐变化，以实现连贯的视频效果。

- 文本到视觉映射

模型使用了一种叫做文本到视觉映射（text-to-visual mapping）的技术，将文字描述转化为视觉特征。这种技术基于 CLIP（Contrastive Language-Image Pretraining）等模型，通过大量的跨模态数据训练，使模型能够理解“沙滩”、“日落”等语义词汇并将其映射到合适的视觉表现上。（这里的举例也可以根据最终选择的素材进行切换）

- 视频生成与融合

在生成每一帧图像后，Sora 会将这些静态图像合成为视频。在视频生成阶段，模型会利用生成对抗网络（GAN）和卷积神经网络（CNN），结合时间信息逐帧生成，从而形成自然的动作和场景转换效果。

关于训练数据：

然而，OpenAI没有透露用于Sora的训练数据的来源。

他们提到需要大量的字幕视频

Sora利用了多样化和广泛的数据集，包括不同长度、分辨率和长宽比的视频和图像。

该模型重新创建虚拟环境的能力表明，其训练数据可能包括游戏片段和来自游戏引擎的模拟。



### 思想

sora涉及的人工智能算法有哪些？



反向扩散原理通俗讲解：一滴油滴到水中，会分散开来，混合到水中，这是扩散，diffusion就是反过来，从无规则的油水混合物的状态下逆转为油水分离的情况，让整体变得有序。

画面生成技术中也是类似，先将规则的画面打乱成无规则的色块，再从这堆马赛克中，复原成有规则的图样，最终结果也可能和一开始不一致，有多种可能性，既可能是红薯，也可能是红鼠，是受到prompt提示词的形象



Transformer通俗讲解：类似于GPT中的token，将文本分成一个个单词或短语，每一个单元就被叫做token；transformer是将图片拆成小块，小块被叫做patch，因为是视频，所以存在时间的维度，是四维的数据（图片的长和宽，色彩空间，时间），通过这些patch，可以来预测出新视频

### 基础科学

这些算法需要什么数学基础？



## 背景

之前有哪些AI生成视频的技术（Runway也出到Gen3了、Pika最近有捏爆何同学的）——时间很短（3-4秒）、质量糟糕（不能理解物理规律、手部缺陷或者记忆之前的主体），在sora出现前都没有让AI视频引发很大的关注

为什么很难做文生视频：

生成视频不光要需要考虑四个维度：画面的长和宽、色彩空间和时间

还需要解决画面的一致性、连贯性、物理合理性，还有逻辑合理性等等一系列的复杂问题

所以需要强大的算力和算法

## Sora特性

### 时间长

在此之前，它们只能应用一些快节奏的短篇，而其他需要长素材的地方，AI视频是不能满足的

sora能够爆火出圈，是因为能生成60秒的视频，大大延长了生成视频的长度

### 画面一致

而想要强制延长runway或pika生成3-4秒的视频，就会出现变形，导致素材完全没法用

但sora生成的画面可以非常一致（放openai油管上的新视频）

随着空间的移动和旋转，Sora视频中出现的人物和物体，也会保持场景一致性地移动

### 输入

Sora可以通过输入文字或图片来生成视频

### 距离相干性和时间连贯性

Sora的距离相干性和时间连贯性更强了

Sora能够很好地记住，视频中的人和物体，即使被暂时挡住或者移出画面之后，再出现的时候，也能够按照物理逻辑的让视频保持连贯性

（找到相关的视频演示）

### 状态模拟

sora模型可以记住发生过的状态

比如说画家在画布上留下新的笔触，这些笔触会随着时间的推移而持续的存在，或者一个人吃汉堡的时候，会留下汉堡上面的咬痕

这意味着模型能够理解运动中的物理世界，也能够预测到画面的下一步会发生什么

### 存在的逻辑性错误

虽然Sora也会出现一些逻辑错误，比如说小猫出现了3只爪子，人在跑步机上的方向反了等等

## Sora背后的技术

OpenAI受GPT的启发，在Sora上也同样想要大力出奇迹，通过堆算力来做视频生成（这里有个英文短语可以更好描述）

通过扩散模型和语言模型的整合，做出了Sora

### AI视频的发展史

早期是GAN（生成式对抗网络）和VAE（变分自编码器），但分辨率太低，已经被淘汰了

后来演变出两种技术路线，分别是扩散（Diffusion）模型和Transformer模型

扩散模型被广为人知的AI绘画Stable Diffusion使用，它的原始模型就是前面提到runway和慕尼黑大学团队发布的（论文链接https://openaccess.thecvf.com/content/CVPR2022/papers/Rombach_High-Resolution_Image_Synthesis_With_Latent_Diffusion_Models_CVPR_2022_paper.pdf）

扩散模型也不难理解，扩散是物体从有序变成无序的状态，比如一滴油混合到水中，这个过程本在物理学里不可逆的，而计算机可以学习扩散的过程，从而反过来将无序的状态逆转回有序，就像让水凝固成冰雕，一堆无意义的马赛克和噪点恢复为为美丽的风景

训练扩散模型的过程也就是给一张图片不断给噪声，直到变成马赛克，然后让计算机学习如何将结果恢复为原始的图片

详细来说，有四个步骤

1.初始化

输入一个随机的图像

2.扩散

将输入的图像，执行前向扩散过程（forward diffusion process），给图像添加噪声，让它逐渐模糊不清

3.逆扩散

将扩散结果执行反向扩散过程(Backward Diffusion Process)，这里会有卷积神经网络CNN的UNet结构参与进来，预测出第二步扩散中，可能添加的噪声，去除它们，从而让噪声图像变得更清楚

4.重复

不断执行前面三个步骤，直到图像的噪声去除效果达到要求

以上是扩散模型使用图片生成图片或视频的步骤了

但如果想从文本直接生成视频，就需要在此基础上多加几步

当你输入提示词时，模型会将它们拆分为多个token（标记），然后传递给编码器(Text Encoder)进行编码（Encoding），转化成embeddings（嵌入），也就是将提示词变成了机器可以理解的内容，将嵌入给到图像生成器（Image Generator），生成一张低分辨率的图像，再使用超分辨率扩散模型将它变得清晰

（逻辑不通）

生成视频和生成图片原理还是一样的，视频只是多了一个时间轴

那视频生成难在哪里呢？

对于视频生成模型，在学习扩散的逆过程时，就相当于从以前的2D逆过程，变成一个3D的逆过程

那图片上存在的问题，比如说人脸不太真实，手指畸变，视频也一样会存在这样的问题



扩散模型比起之前的GAN模型来说有三个优点

第一就是稳定性好，在训练过程不容易陷入模式塌陷等问题
第二就是生成图像质量会更高
第三就是无需特定的架构，扩散模型不依赖于特定的网络结构，兼容性好，可以用很多不同类型的神经网络

然而扩散模型也有两大主要的缺点
首先是训练成本高，与其他模型相比，扩散模型的训练会更昂贵
因为它需要在不同噪声程度的情况下学习，去噪需要训练的时间更久
其次是生成花费的时间更多，因为生成的时候，需要逐步的去找生成图像或者视频，而不是一次性的生成整个样本

现在无法生成更长的视频，一个很重要原因就是显存是有限的
你生成一张图片
是可能占了那些部分的现存
然后你如果生成16张图片
那那就占了
可能差不多就把这现存给占满了
当你需要生成更多张图片的时候
你就得想办法
怎么去继续考虑
之前已经生成的这些信息
然后再去预测
后面该生成什么样的信息
就是它是在首先在模型上面
就提供一个更高的要求
当然算力上面也是一个问题
就是或许过很多年之后
我们的险存会非常的大
可能我们也就不存在这样的问题了
也是有可能的
但是就目前来说
当下我们是需要一个更好的一个算法
但是可能这个问题如果有更好的意见
可能这个问题就不存在
所以呢这注定了
目前的视频扩散模型
本身可能不是最好的算法
虽然让位和Pika Laps等代表的公司啊
一直在优化其算法