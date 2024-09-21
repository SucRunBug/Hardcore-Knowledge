# 视频设置

## 前言

本条目用于游戏画质优化，以实现在相同的硬件条件下，获得更好的画质或性能表现。

条目作者从前均使用GeForce Experience软件进行自动调优，奈何此软件并不能迅速适配优化新游戏，或者无法检测出某些游戏，于是打算自行研究游戏性能调优。

参考视频链接：https://www.bilibili.com/video/BV11G4y1a7d8

阿狸豪堪。但此视频时间在40系显卡发售以前，可能存在时效性问题，比如DLSS3没有被应用，但可以参考性能近似的显卡设置。

如下图：

![alt text](<Screenshot 2024-08-14 at 13.47.40.png>)

比如我可以参考视频中3060ti的设置。

> 一些值得降低的选项

有些选项对帧数影响较大，但对画质影响较小，如：体积云，树叶可见距离、水物理质量。

该视频主题是：改善模糊，提升画质。

下面是游戏实操

### 荒野大镖客2

运动时会出现模糊。

#### 抗锯齿

游戏采用的抗锯齿是TAA(Temporal Anti-Aliasing，时间性抗锯齿)，高速运动时，画面会出现残影。如果换为FXAA（快速近似锯齿），锐度降低，远程植被闪烁。如果换为MSAA（多重采样抗锯齿），效果很好，但4倍以上性能开销极大。

#### NV滤镜

开启方式alt+F3


#### 优化实例

![alt text](<Screenshot 2024-08-14 at 13.51.20.png>)

下面一种有些奇怪，我以为是A卡使用，但无光追单元的N卡也能用。

![alt text](<Screenshot 2024-08-14 at 13.59.26.png>)

具体步骤是给大镖客打上FSR2.0补丁，从而在游戏中出现DLSS的选项，开启后是实现的FSR的抗锯齿效果。

其次是这样回合NV滤镜冲突，如需开启，需要使用Reshade软件。

![alt text](<Screenshot 2024-08-14 at 14.01.47.png>)

下面是总的设置单：

![alt text](<Screenshot 2024-08-14 at 14.11.46.png>)

![alt text](<Screenshot 2024-08-14 at 14.13.02.png>)

#### 超采样

以3060ti为实例

![alt text](<Screenshot 2024-08-14 at 14.17.30.png>)

设置后可以将分辨率拉高，比如即便显示器为1080P，可以改为4k，画质提升巨大（即便显示器不是4k的）

![alt text](<Screenshot 2024-08-14 at 14.20.08.png>)

如果是其他不支持DLSS或FSR的游戏，可以将DSR改为1.25或1.5倍

### 锁帧游戏

诸如魂类、米家游戏等，会进行强制锁帧，那就可以只提升画质。

但可能出现拉满画质后，GPU依旧不能满载的情况，可以使用NV滤镜提高画质，2个选项如下图：

![alt text](<Screenshot 2024-08-14 at 14.26.27.png>)

开启即可。

下面是设置汇总：

![alt text](<Screenshot 2024-08-14 at 14.33.34.png>)

![alt text](<Screenshot 2024-08-14 at 14.35.40.png>)

视频的最后还有针对A卡驱动的专门讲解，我就不记录了。也是类似N卡超采样。

评论区也收集了一些有用的评论：

> 顶我上去让大伙看到
> reshade大多数游戏我都建议用奥德赛的realistic reshade 不掉帧且画质增强很多 并且可以home键呼出挑锐化；nv滤镜的话 使明朗（强度56 忽底56）曝光（曝光15 对比度31 亮点-18 阴影35 灰度0）详细信息（使明朗0 明亮度44 hdr-14 泛光0）锐化（强度13 细节-16）颜色（色调20 强度15 温度 亮丽0）相信我 打开新世界

但也会降低帧数。

第二个参考视频链接：

https://www.bilibili.com/video/BV1Zz4y1U7aB

喵喵折，好！非常硬核的一期视频，并且在视频的结尾有给出视频总结和优化实例。

纹理质量（材质）在显存足够的情况下，可以拉满。