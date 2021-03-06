---
title: 《3D Point Cloud Registration for Localization using a Deep Neural Network Auto-Encoder》读书笔记
date: 2017-11-13 22:34:22
tags: [笔记]
categories: [笔记]
---

[3D Point Cloud Registration for Localization using a Deep Neural Network Auto-Encoder](https://www.researchgate.net/publication/316455393_3D_Point_Cloud_Registration_for_Localization_Using_a_Deep_Neural_Network_Auto-Encoder)

题目：基于深度神经网络自编码的3D激光点云匹配定位方法
>介绍：
本文将介绍一种3D点云定位方法，该方法需要一个全局的3D点云地图，以及几次连续扫描得到局部的点云子集，而无需考虑先验信息。

>局部的点云子集又称为超点集。超点集是连续几次扫描得到的、外围具有重合区域的点云，并初步过滤了低质量的、不重要的区域。它们表示环境的几何结构信息。
>
>通常的方法是人工设计关键点描述子的进行粗略匹配，本文利用超点集代替了关键点，这样可以充分利用点云的几何结构关系，进行更准确仿射变换。并借鉴了机器视觉的研究趋势，将自编码深度神经网络应用于局部3D点云。本方法经过许多具有挑战性的点云数据检验，相比以往的方法，在有噪音、缺少数据的情况下，更加鲁棒。


##摘要
 
 　　我们提供了一种在大规模点云和近距离扫描点云之间的匹配的算法，提供了一个完全独立于两个点云坐标系统初始位置信息的定位解决方案。该算法表示为“LORAX”，它选择用我们称之为“超点集”的局部点云子集和"低维描述符"描述每个点的几何结构。然后使用这些描述符来推断潜在的匹配区域以进行有效的粗略匹配处理，然后进行微调阶段。通过覆盖重叠的点云来选择超点集合，然后过滤出低质量或不明显的区域。“描述符”是使用最先进的无监督机器学习来计算的，利用基于深度神经网络的自编码技术。
 　　
　　针对普遍的使用手动设计的关键点描述符来进行粗点云匹配，这个新颖的框架提供了一个强有力的替代方法。利用超点集而不是关键点集，可以更好地利用可用的几何数据来找到正确的转换。在编码局部三维几何结构时使用深度神经网络自编码器代替传统的“描述符”继续在其他计算机视觉应用中看到的趋势，并且确实取得优异的结果。该算法在具有挑战性的点云匹配数据集上进行了测试，与之前的方法比较其优点体现在对密度变化，噪声和缺失数据的鲁棒性。 

##１．引言
###1.1概况
 　　点云与图像类似，含有描述我们周围的世界的对象的语义信息。 与图像数据不同的是，图像数据在一个固定的网格场景保存了二维目标，点云是一组的在统一的坐标系中无组织的三维点，它捕捉的是3D空间信息。点云数据分析方法已经在过去的几十年里的到了发展[^footnote]，并且这个领域正在不断取得显著性突破的一个重要原因是经济实惠高质量的3D扫描技术的进步[^footnote]，机器学习突破和新的有趣的应用程序。
        
　　点云匹配的定义是找到在两个独立的点云坐标系之间的转换公式。它是“即时定位与地图构建”（SLAM）[^footnote] [^footnote]，场景的三维重建[^footnote]等的关键。它已成为基于视觉的自主驾驶的的核心问题[^footnote]。点云匹配取得了很大的进展，但仍然有重大的进展挑战，如大规模点云的匹配，具有低场景重叠且没有先前的位置信息。
　　 
 　　今天的户外定位严重依赖于GPS技术，在地面增强系统的辅助下，提高准确性 。卫星定位需要接收来自多个卫星的信号，准确度受到卫星数量，天气和物理障碍物阻塞或改变信号路径的影响。这项技术在在一些卫星地面基础设施不完善的地区是不准确的[^footnote]。 
 　　
　　在本文中，我们重点介绍一种定位技术，它依靠匹配一个全局点云和一个在同一个场景内不同时间扫描得到的局部点云技术。匹配在云的初始坐标系中与邻近信息无关。我们定义了两种类型的点云：一个“全局点云”，由一个大型户外场景扫描得到的点云，它的坐标是现实世界的地理坐标系统；另外一个是“ 局部点云”，由一个小得多的点云组成，它处在在全局点云场景内的未知位置和未知方向。在局部点云和全局点云之间的转换公式是使用一个机器学习的几何分析进行计算而得到的。这种技术作为一种高质量的定位方法在户外环境是完全与GPS的独立的。『 参见图1』 


![这里写图片描述](https://www.researchgate.net/profile/Gil_Elbaz/publication/316455393/figure/fig1/AS:486979151372288@1493116285586/Figure-1-Registration-between-a-close-proximity-point-cloud-colored-and-a-large-scale.ppm)
          *图1 绿色为超点集，灰色为全局的3D点云地图*
　　

###1.2相关工作

 　　匹配算法分为粗匹配和精细匹配。粗略的匹配算法对点云的位置没有先前的临近假设，只是一个粗略的对齐，这意味着他的一个损失函数比较宽松。精细的匹配算法假定输入点云大致对齐; 因此它们利用点之间的初始接近度来调整点云坐标系之间的对齐。当连续获取两个场景之间大量重叠时，或者作为粗略匹配过程的后续，可以使用精细匹配 。

　　虽然已经有大量的点云粗匹配方法[^footnote]，但粗匹配仍然是一个开放的挑战，有很大的改进空间。在快速点特征直方图[^footnote][^footnote]（FPFH）算法中，一个针对点云内的每个点在多个尺度上计算基于直方图的描述符。多尺度计算中的显著持续直方图被标记为关键点，然后将其匹配以找到点云之间的配准。其他描述符也用于定位和描述关键点。参见[^footnote]进行调查。一些例子是3D-SIFT [^footnote]，NARF [^footnote]和SHOT [11]。许多复杂的手工编码特征被提出，其目标是旋转和平移不变，对噪声具有鲁棒性。

　　在二维计算机视觉领域，由于深度学习领域的突破性研究 [^footnote]，类似的手动编码特征的发展时期已经戛然而止。使用深度学习方法，从数据中计算出更先进的特征（具有超越人性化设计的复杂性），推进了二维计算机视觉领域的主要领域，如检测，分类，分割，定位和配准 [^footnote]。这些方法几乎专注于二维数据。非结构化的，连续的和大的点云数据集产生了这样的极端的问题，这个问题是2维数据不能直接的适应3维空间。为了在我们的方法中利用三维数据，点云被密集采样，并且每个局部表面的2.5维数据被捕获和组合。深度学习的先进工具被应用到这些数据中，以无监督机器学习的形式来实现高质量降维 [^footnote]，作为粗略点云匹配的关键阶段 。
　　
　　针对机载LIDAR点云的粗略配准，开发了一种基于线性平面匹配的不同方法[^footnote]。 依靠线性结构的存在，这种方法仅限于特定的数据集类。
　　
　　点云之间精细配准的问题已经被深入研究，目前在线应用如SLAM存在高质量的解决方案[3,4]。 解决方案围绕迭代最近点（ICP）[^footnote]算法及其改进[^footnote]。 一个值得注意的基于傅里叶域[^footnote]扩展高斯图像相关性的精细配准方法被提出来作为ICP的一个替代方案，尽管最后一个阶段再次依靠ICP迭代来进行微调。 良好的匹配并不是本研究的重点，尽管为了实现端到端匹配，标准的ICP算法在其最终阶段被使用。
　　
　　上述所有匹配方法都是针对输入点云对进行设计的，这些输入点云对数量级相差不大，数量较少（低于100万）。

##1.3.贡献
　　这项工作提出并测试了两种原创的方法，首次提出基于点云的匹配方法。
　　
　　1.使用超级点（由随机球体覆盖集选择）作为匹配的基本单位，而不是常用的关键点或局部线性结构。 这利用了更广泛的几何结构，并更好地利用可用数据来发现正确的转换。 此外，它将算法的其余部分的复杂性转化为与点云场景中覆盖的表面积相关，而不是场景中的点数。 它非常简单，快速，并且具有可扩展性。
　　
　　2.使用深度神经网络自动编码器编码局部3D几何结构。 该方法在图像分析应用程序中提供了最先进的编码。 通过对数据进行调整并在开发的算法流水线中应用这种方法，可以从数据中创建出功能优于人工设计的局部几何特征。
　　
　　我们在这里展示，结合这些想法在多个具有挑战性的数据集上产生达到期望的的匹配结果。该方法是通用的，它可以处理任何数据，而不管传感器或场景的类型如何。
　　
　　虽然大多数配准算法处理相似的点云，但我们采用点云的独特问题设置，它们的尺寸明显不同，我们设计的算法对大规模扫描数据有效。 虽然算法需要初始阶段，但在线阶段可以高效地并行执行，使其适用于实时应用程序。

#2.基于“LORAX”的点云匹配算法

　　我们专注于两类点云的匹配：描绘大型户外区域的全局点云，以及从全局点云场景内捕获的小型局部点云。全局点云可以包含多达1亿个3D点，而局部点云可以小2-3个数量级。
　　
　　在本节中，我们提出使用基于深自动编码器减少的覆盖集（LORAX）的匹配算法来进行定位。

##2.1.算法概貌
这个算法包含以下几步：
　　1．使用“Random Sphere Cover Set algorithm”算法将点云分为超点集。
　　2.为每一个超点集选择一个归一化的局部坐标系。
　　3.将超点集数据投影到二维深度图上。
　　4.对于超点集进行显著性检测和渗透（ﬁltration）。
　　5.利用深度遗传网络自编码器来降维。
　　6.寻找相关描述符之间的候选匹配。
　　7.利用本地化搜索来进行粗匹配。
　　8.微调迭代最近点。
接下来，算法的每一步将进行详尽的解释和分析。

##2.2.Random Sphere Cover Set (RSCS）“随机球体覆盖集”　　
　　首先超点集（SP）将用作匹配过程的基本单位被定义。 每个超点集是描述局部曲面的点的一个子集。重叠是允许的（即一个点可以包含在几个中超点）。 为了获得云中几乎所有点（〜95％）的覆盖率，我们建议采用下面的迭代过程：（1）随机选择一个不属于任何SP的点“P”; （2）将一个新的SP定义为位于以“P”为中心的固定半径为“Rsphere”的球体内的一组点。
　　![这里写图片描述](https://www.researchgate.net/profile/Gil_Elbaz/publication/316455393/figure/fig2/AS:486979151372290@1493116285843/Figure-2-Coverage-of-points-vs-RSCS-iterations.ppm)
      Figure 2: Coverage of points vs. RSCS iterations

　　这个简单的过程，我们称之为RSCS，具有有趣的属性，可以估计半径“Rsphere”参数。在随机球体填充中，非重叠的球体显示出填充大约64％的封闭3D区域[^footnote]。 假设“Vlocal”是包含局部点云的球体的体积，并且是在算法的最后阶段使用的匹配数量。 为了确保最少m个SP对匹配，我们选择半径“Rsphere”，这样就有可能在体积”Vlocal“内随机打包”2m“个球体。

<img src="http://chart.googleapis.com/chart?cht=tx&chl=R_%7Bsphere%7D%5Capprox%20(%5Cfrac%7B3%7D%7B4*pi%7D*%5Cfrac%7B0.64%7D%7B2*m%7D*V_%7Blocal%7D)%5E%7B%5Cfrac%7B1%7D%7B3%7D%7D" style="border:none;" />              (1)


　　在补充材料中分析了给定局部和全局点云内在参数的RSCS算法创建的SP数量。结果表明，该方法覆盖了点云呈指数衰减的点。 图2显示了作为RSCS迭代函数的覆盖点的百分比。 RSCS算法在全局点云上应用一次，在局部点云上多次应用，以便在后期的鲁棒性。 如图7（a）和（b）所示，从RSCS的多个应用程序在本地点云上找到的<img src="http://chart.googleapis.com/chart?cht=tx&chl=N%5E%7Blocal%7D_%7BSP%7D" style="border:none;" /> SP在算法的下一个阶段被组合成一个代表本地点云的单个集合。

##2.3.为每一个超点集选择一个归一化的局部坐标系
　　一个SP的局部坐标系定义如下：原点被设置为SP的质心，然后在SP内点的估计协方差矩阵上使用奇异值分解（SVD）来设置SP的坐标系 。
　　假设每个SP描述场景表面在这个阶段被利用。曲面的特征值是具有两个大小相近的大特征向量和一个较小的特征向量。这意味着这些点主要分散在两个维度上，而第三维的变化则显着较低。 z轴被设置为第三特征向量。 为了定义x轴，计算SP的不连续径向切片的平均高度并插入到极坐标直方图中。 然后将x轴设置为与最大仓（bin）对应的方向。 这个局部坐标系统创建不变的SP的位置和方向，同时保留其几何特性。

##2.4.深度图投影
　　将每个SP代入本地坐标系后，可以直接进行比较。 然而，结果将是完全不可靠的，因为它们已经受到点密度变化和随机噪声的影响。 降低维度对于减轻这些影响至关重要。 为此，连续点位置数据被转换成离散图像格式（大小为[dim1，dim1]）。将SP缩放到图像dim1的尺寸（我们使用dim1 = 64），之后将每个点的z轴高度投影到每个对应像素的深度图上。 最后，将图像裁剪为[dim2，dim2]（我们使用dim2 = 32），以去除深度图中SP的圆形边缘。 参见图3（a）和（b）

![这里写链接内容](https://www.researchgate.net/profile/Gil_Elbaz/publication/316455393/viewer/AS:486979138789376@1493116282891/background/4.png)
(a) Example SP     (b) The depth map     (c) The reconstruction
Figure 3: Super-point depth map projection  

　　为了减少噪音和变化的密度的影响，最大的过滤器和平均过滤器被应用于图像。SP信息的修改可以通过从深度图重构SP来可视化。 如图3（c）所示，重构可靠地保持与原始SP点云相同的几何形状和质量，同时在未知稀疏区域上创建完整覆盖。

##2.5.显著性检测和过滤

　　为了获得最快和最好的质量的匹配，应该减少通过这条管线的不相关SP的数量。不相关的SP被三个标准过滤：密度，几何特性和显着性水平。
　　**密度测试**：密度是以绝对值和其他SP值来衡量的。这意味着包含少于<img src="http://chart.googleapis.com/chart?cht=tx&chl=N_%7Bd%7D" style="border:none;" />点的SP被滤除。另外，与其最接近的相邻SP（在SP重心之间测量的欧几里德距离）相比，具有相对较少点的SP也被滤除。
　　**几何质量测试**：测量各个局部坐标系内各个SP的高度。过滤掉在表面的低高度SP信号。
　　**显着性测试**：来自全局点云的SP深度图被重新整形成长度为<img src="http://chart.googleapis.com/chart?cht=tx&chl=d%5E%7B2%7D_%7Bim2%7D" style="border:none;" />的列“深度矢量”。对于该组深度矢量执行主成分分析（PCA）。仅使用前三个特征向量精确重构的SP（SP来自于局部和全局点云）已经在数据集中发现了几何特征，因此被滤除。这降低了位于不同点云区域的类似SP的匹配机会 。

##2.6.通过自动编码器来降维
　　该算法的一个关键阶段是比较全局和局部点云内部的SP几何。 即使具有相同的语义含义，高维物体的比较也容易产生较大的噪音和方差。为了比较SP几何的语义，必须在保留最大几何信息的同时减小深度图像的尺寸。本文研究了两种单独的降维方法。 第一个基于PCA，第二个基于深度自动编码器（DAE）。
　　
###2.6.1线性降维
　　PCA方法通过将其投影到较低维的线性超平面上来降低数据的维数。 根据全局点云SP的深度向量（类似于显着性检测，但是这里k> 3）来计算基本向量的基数。 对应于特征向量的特征值定义重建每个SP所需的叠加。 保持SP的几何信息的紧凑表示特征被表示为主分量分析超点特征（PCAF）。 需要注意的是，PCA创建了数据的线性投影，这导致高数据损失和相对较大的简化形式。 这种方法是DAE方法的可比基准。
　　
###2.6.2深度自动编码器降维

　　结果表明，DAE神经网络[16]产生了最先进的图像压缩。 在这里，我们使用这种技术来获得在深度图中捕获的2.5D超点几何体的紧凑表示。
　　DAE神经网络架构由编码器和解码器阶段组成。 编码器阶段从输入层开始，然后连接到隐藏层，逐渐减小直到达到所需的紧凑尺寸。 解码器阶段从数据的紧凑表示开始; 每个后继隐含层的维数较高，直到输出层维数达到输入维数为止。损失函数定义为输入层和输出层之间的像素误差，优化网络实现用图像的最佳紧凑表示。
　　为了设计适合我们应用的DAE，我们进行了大量的实证测试和优化。 我们得出结论，一个具有4个完全连接隐藏层的网络，使用每个层之间的S形非线性激活函数和输入层上的压差（DO），会返回满意的结果。 为了进一步减少网络中的参数数量，第四和第一隐藏层以及第三和第二层以相同权重彼此镜像，同时保留单独学习的偏差值。 这种权重共享意味着反向传播学习算法被约束为编码和解码过程优化相同的权重[^footnote]。 缩小的紧凑尺寸由编码器输出尺寸定义。 该架构使用以下维度：1032（input），[1032,128]（L1），[128,10]（L2），[10,128]（L3），（128,1032）（L4），1032（output）。 参见图4。
　　![这里写图片描述](http://img.blog.csdn.net/20171109204736502?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Figure 4: Deep Auto-encoder architecture

　　利用数据驱动和综合深度图的组合来初始训练深度神经网络。使用100,000个超点集深度图来训练所提出的DAE。 训练阶段是无监督的，即不需要手动注释数据，网络以随机权重初始化。 这个培训过程是有效的，网络可以通过周期性地更新，从扫描的本地云中获取附加的点云数据来改善。 流程训练过程可能很长，但编码部分可以在线并且快速地激活。
　　这种紧凑的低维表示可以被看作是捕获整个SP的几何信息的特征。 它代表一个固定的低维向量的SP，与SP中的点数不相关。 与在每个点上竞争的局部描述符相比，这是在复杂度的降低方面的实质性改进。 基于SP自动编码器的特征（SAF）可以用于许多任务，例如检测或分类3D对象，而在这里我们优化它的匹配。
　　　![这里写图片描述](http://img.blog.csdn.net/20171109204803901?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Figure 5: Visualization of deep auto-encoder input, reduction and reconstruction

　　图5示出了输入到DAE中的深度图的例子，并且被简化为10维SAF（为了更好的可视化而被放大的5×2矩阵），然后通过解码器被重构成原始尺寸。 深度图的高度被转换为颜色：蓝色对应于零高度，深红色对应于最大高度。 重建与输入不相同，但它确实捕获了SP的一般几何形状。 这对噪声的鲁棒性和对我们的应用而言至关重要的小的改变是最佳的，同时捕获重要的SP几何特性。
　　为了进一步显示DAE的有效性，分析从数据内学习到的特征，并将其与从PCA方法计算的特征向量进行比较。为此，将SAF向量中的每个维度的独立激活输入到解码器中以可视化 DAE学到了什么。参见图6。
　　PCA的特征向量和DAE的独立解码器激活是紧凑表示“构建块”的两种创建的方法。 图6（a）显示了每个特征向量的增长的复杂性，它是根据特征值排序的。 这与图6（b）的非结构化DAE激活图像形成对比。 PCA方法可以近似为只有线性函数的单隐层神经网络[^footnote]。 这意味着为了表示复杂的几何图形，需要复杂的特征向量。 由于DAE中数值的多层叠加，复杂的几何体使用相对简单的“构建块”来表示。
　　

![这里写图片描述](http://img.blog.csdn.net/20171109203628375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
(a) PCA eigenvectors

![这里写图片描述](http://img.blog.csdn.net/20171109203728076?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
(b) DAE independent decoder activations
Figure 6: Compact Representation Vectors

##2.7.选择理想的匹配


　　在用SAF向量描述每个SP之后，我们选择一组类似描述的SP对作为匹配的候选。 通过测量SAF特征之间的欧几里得距离，局部点云中的每个SP与来自全局点云（我们将K设置为3）的K个最近邻居配对。 当与i + 1最邻近点相关的距离显着大于与i最近邻点相关的距离时，我们滤出候选值i + 1到K.请注意，<img src="http://chart.googleapis.com/chart?cht=tx&chl=P_%7Bcandidates%20%7D" style="border:none;" />的数量是O（<img src="http://chart.googleapis.com/chart?cht=tx&chl=N%5E%7Blocal%7D%20_%7BSP%7D" style="border:none;" />）的顺序。 为了得到所选择的候选点云集的数量，考虑一个全局点云数量为1000万点，局部点云数为50万的问题。 对于这个集合，我们从本地点云中获得大约200个SP，这意味着选择了大约550个候选者。 参见图7（c）。

![这里写图片描述](http://img.blog.csdn.net/20171111192506338?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
(a) Global point cloud with RSCS (b) Local point cloud
with RSCS
![这里写图片描述](http://img.blog.csdn.net/20171111192603271?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
(c) Matching candidate connections

Figure 7: RSCS and matching candidates (each point is col-
ored according to the last RSCS iteration to cover it)

##2.8.基于本地化搜索的粗匹配
 　　为了找到点云之间的6DoF（6自由度）变换，至少需要3个匹配（为了鲁棒性，我们使用了m = 6）。处理至少<img src="http://chart.googleapis.com/chart?cht=tx&chl=P%5E%7Bcandidates%7D%20_%7Bm%7D%0A" style="border:none;" />的搜索空间大小是不切实际的（对于上面的例子超过<img src="http://chart.googleapis.com/chart?cht=tx&chl=10%5E%7B13%7D" style="border:none;" />）。因此，我们考虑每个迭代只有m个候选对，所有全局云点可以包含在一个体积不超过<img src="http://chart.googleapis.com/chart?cht=tx&chl=V_%7Blocal%7D" style="border:none;" />的球体中。这将转换选项的搜索空间减少了8到9个数量级（将上例中的选项减少到约40,000）。我们使用RANSAC [^footnote]程序，迭代地选择6个候选对，计算变换，并通过测量本地点云中的变换点与其全局点云中的最近邻点之间的平均（物理）距离来检查一致性。 （为了节省运行时间，我们只转换局部点云的稀释版本。）我们测试了10,000个随机选择（约1/4的搜索空间）。我们不是只选择最好的评分变换作为粗略配准步骤的结果，而是记录5个最佳变换（T1，...，T5），其中局部点云不重叠。然后将精细调整步骤（在下一节中介绍）应用于每个调整步骤，最后得到最佳评分细节的调整步骤将被选中。


##2.9.微调最近迭代点
　　简单迭代最近点（ICP）精细调整被执行，由T1，...，T5转换中的每一个进行初始化。选择具有最低ICP损失的配准，定义LORAX输出变换。这个步骤源于我们认识到“最接近的”粗配准结果并不总是与正确的配准结果相关，因为优化函数中存在许多局部最小值。最好的匹配会根据经验显示对应于约75％的案例中最好的粗略匹配，约18％的案例是次优匹配，约4％是第三好的匹配。 这个阶段可以被任何微调方法取代。

##2.10.效率讨论
　　我们目前的实施并未针对实时性能进行优化。然而，考虑到全局点云通过航空LIDAR或立体重建提前捕获，该算法确实有潜力被纳入现场设备并执行实时定位。我们设计了神经网络训练过程和全局点云到降维阶段的计算。来自全局点云的紧凑描述符可以与全局点云的降采样版本一起保存到在线设备中。一旦本地点云从全局场景内的未知位置在线捕获，SP分区，归一化，显着性检测和DAE降维阶段可以独立地为每个SP并行执行。然后，基于KD树[^footnote]，基于RANSAC的局部候选搜索[^footnote]和ICP [^footnote]的KNN候选选择也可以并行地（使用多个CPU和/或GPU）并行地完成。 RSCS方法和SAF描述符的代码可在以下网址获得：[https：//github.com/gilbaz/LORAX](https%EF%BC%9A//github.com/gilbaz/LORAX)

#3.实验及其结果
　　在整个实验中显示了RSCS SP创建优于FPFH持久性关键点检测的优点，以及SAF与FPFH描述符相比的描述性质量。 每个登记阶段都使用“点云配准算法的挑战性数据集”[^footnote]进行了广泛的测试，将近距离点云与两个不同季节拍摄的同一场景的全局大型点云进行匹配。此外， 为了更好地理解不同类型噪声对配准的影响，采用大尺度航空点云进行对比实验。

##3.1.挑战性的数据集配准测试
　　所使用的数据集包含许多户外场景的点云，由地面LIDAR扫描仪捕捉，多个季节。 通过将每个场景中捕获的点云激光扫描拼接在一起，创建出真实的全局点云。 这个点云数据是测试LORAX算法的理想选择。 我们通过将局部点云记录到同一场景的全局点云中进行测试，并在不同季节拍摄。 这种设置确实充满挑战。 场景中包含一些刚性静止的物体，如凉亭结构，灯柱和长凳，还有人，灌木和树枝等不一致的物体。 这个挑战由于扫描角度和遮挡而丢失信息而提高。
　　参见图1为LORAX结果的一个例子。 颜色表示局部点云（匹配之后）中每个点与全局点云中最近邻点之间的相对距离，其中绿色更接近，红色更远。
　　我们测试和比较RSCS超点集与关键点的匹配表现，还有FPFH描述符与PCAF的匹配表现，以及FPFH描述符与SAF的匹配表现。结果总结在表1中。
![这里写图片描述](http://img.blog.csdn.net/20171111201208718?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



　　为了比较基于关键点的方法，我们遵循[9] 。 “RSCS + FPFH”方法计算与RSCS超点集中心点对应的FPFH描述符。报告的每个结果是12个局部点云的平均性能，来自“Gazebo”数据集的9个和来自“Wood”数据集的3个。对于每个记录结果，我们测量[^footnote]中使用和定义的相对平移误差（RTE）和相对旋转误差（RRE）。当RTE低于预定义的阈值（我们使用1米）时，匹配结果被定义为二进制“成功”。对于每个测试，我们报告二进制成功率和成功测试的平均RTE和RRE分数。所有方法使用相同的微调程序，因此实现相似的RTE，但是它们具有不同的结果RRE和二进制成功率，表示质量以及粗略匹配的鲁棒性。

　　表1显示，使用RSCS组合的点云细分和DAE来创建SAF会产生最稳健和最高质量的匹配结果。从RSCS获得的鲁棒性在KP + FPFH与RSCS + FPFH的比较中是明显的。 RSCS + FPFH到RSCS + PCAF和RSCS + SAF的比较显示了使用基于机器学习的功能优于手动设计的功能的优点，以及SAF优于PCAF的优点。

##3.2.噪声，遮挡和密度敏感度测试

　　为了进一步测试LORAX的质量和局限性，我们使用了一些由[^footnote]提供的城市户外场景点云。这些点云大约150万个点，描绘了250平方米的大面积。参见图8的例子。图8：描绘与周围的房屋和道路的小山的空中扫描点云。
　　![这里写图片描述](http://img.blog.csdn.net/20171111201938808?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Figure 8: Aerial scanned point cloud depicting hill with sur-
rounding houses and roads. An example from [30] dataset

　　为了能够控制噪声，密度和遮挡的不同参数，我们进行半合成实验，从大的原始点云中裁剪半径为15-50米的小点云.将LORAX和KP + FPFH配准算法在局部点云的改动版本上进行测试，并将其与原始全局点云进行匹配。
　　噪声修改包括：（1）随机移动10％，20％，50％ （2）随机抽取10％，20％，50％的点，测试对云密度的敏感性;（3）模拟去除局部随机球内的10％， 20％，50％的遮挡。 参见图9。
![这里写图片描述](http://img.blog.csdn.net/20171111202755641?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

(a) Original cropped local point cloud
(b) Same cloud with simulated noise and occlusions
Figure 9: Simulating noise, density change, and occlusions

　　对3个全景场景中的50个随机剪切点云进行了测试，分析了下采样（密度变化）（DS），随机重定位噪声（RN）和遮挡（OC）对各自的影响。图10总结了结果。为了澄清，在给定噪声指定的情况下，图上的每个点代表50个配准测试的平均二进制成功率。在这个实验中，二进制成功率由2.5米的RTE阈值定义（由于全局点云的大尺度）。
　　
　　![这里写图片描述](http://img.blog.csdn.net/20171111204639218?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM3MjUxMDQ0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Figure 10: Noise, Occlusion, and Density Sensitivity Tests

　　由于深度图投影阶段，这些结果显示对点密度具有高度的鲁棒性。随机噪声对我们的算法影响不大，因为SAF表示法只捕捉主要的几何特征。堵塞是最难处理的缺陷。该算法在较低的水平上克服了遮挡，但是受到很大的阻碍。总的来说，我们看到LORAX对大量的随机噪声，密度变化和遮挡不太​​敏感，而且其鲁棒性只在极端水平下才会恶化。 KP + FPFH算法（虚线）由于在全局点云的许多部分缺乏“关键点”诱导场景特征而在干净的局部点云上进行测试时返回了较低的二进制成功率。这些结果增加了对这项研究方向的信心。

#4.总结
　　本文提出了一种创新的点云配准算法LORAX。该算法以室外定位为目标，处理两个匹配点云之间的点数差异存在多个挑战，并涉及大量的点数。提出了两种原始方法：1）使用超点集（由随机点云的点集合组成）作为匹配的基本单位，而不是关键点; 2）使用深度神经网络自编码器编码局部3D几何结构。我们已经表明，这些想法的组合在具有挑战性的数据集上产生有希望的匹配结果。该方法是通用的，它可以处理任何数据，而不管传感器或场景的类型如何。而且，虽然它包含了一个有效的训练阶段，但在线阶段可以高效并行执行，使其适用于实时应用。

　　在未来的工作中;我们打算将这种方法适用于具有小场景重叠的类似大小的点云。另一个有趣的方向是设计这种算法的多尺度超级版本。最后，使用具有高度和颜色信息输入的卷积自动编码器可以产生出色的超点功能，可用于各种点云分析任务。

#感谢
　　这项研究得到了以色列工业和贸易部的Technion和Magnet Omek联盟的部分支持。 作者要感谢Elbit Systems Ltd为这项研究提供数据。





#摘要
 1.[Registration of 3D Point Clouds and Meshes: A Survey from Rigid to Nonrigid.](https://www.researchgate.net/publication/236675142_Registration_of_3D_Point_Clouds_and_Meshes_A_Survey_from_Rigid_to_Nonrigid)《3D点云与网格匹配：一个由刚性到非刚性的调查》
   >摘要 
3D表面匹配变换多个3D数据集到同一坐标系中以便对准这些集合中的重叠部分。最近的调查覆盖的是刚性或非刚性匹配，但很少有团队能够从不同方面讨论全过程。我们的研究有两个目的：（i）对两种类型的匹配进行了全面调查，专注于三维点云和网格，（ii）从数据拟合的角度更好地了解匹配。匹配是与数据拟合密切相关的，它包括三个核心：模式选择，相关性和约束性，优化。研究这些组分：（i）提供了一个比较不同新奇技术的比较基础，（ⅱ）揭示了刚性和非刚性在representations问题匹配的相似性 ;（iii）显示非刚性匹配过度拟合产生的原因，同时增加我们对内在技术的兴趣。我们进一步总结匹配的一些实际问题，其中包括初始化和评估，并讨论一些我们自己的意见，见解和可预见的发展趋势研究 



2.[Brent Schwarz. Mapping the world in 3d. Nat. Photonics,4(7):429–430, 2010.[3] Hugh D](https://www.nature.com/nphoton/journal/v4/n7/abs/nphoton.2010.148.html)《激光雷达：用3D来映射世界》
>摘要
>具有64个半导体激光器的旋转传感器头的高分辨率激光雷达系统可以以前所未有的细节高效生成3D环境地图。

3.[Hugh Durrant-Whyte and Tim Bailey. Simultaneous localization and mapping: part i. IEEE robotics&amp; automation magazine, 13(2):99–110, 2006.](http://ieeexplore.ieee.org/document/1638022/)《定位同时建图：部分一》
>摘要
>本文介绍了同时定位和建图（SLAM）问题和解决SLAM问题的基本方法，并总结了该方法的关键实现和演示。 虽然仍然有许多实际问题需要克服，特别是在更复杂的户外环境中，一般的SLAM方法现在已经成为机器人的一个很好的理解和建立的部分。 本教程的另一部分概述了解决SLAM中剩余的一些问题的最近的工作，包括计算，功能表示和数据关联。


4 [Jakob Engel, Thomas Schöps, and Daniel Cremers.Lsd-slam: Large-scale direct monocular slam. In European Conference on Computer Vision, pages 834–849. Springer, 2014](http://xueshu.baidu.com/s?wd=paperuri:%288d7f210f73ebaf20d4e03873a1dbe06d%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://link.springer.com/chapter/10.1007/978-3-319-10605-2_54&ie=utf-8&sc_us=9457780068583818158)《Lsd-slam：大规模直接单目slam》
>摘要
>我们提出了一种直接（无特征）的单眼SLAM算法，与当前直接方法的最新技术相比，它允许构建大规模，一致的环境地图。 除了基于直接图像对齐的高度精确的姿态估计之外，3D环境被实时重建为具有关联半密集深度图的关键帧的姿态图。 这些是通过对大量像素小基线立体比较进行滤波而获得的。 明确的尺度漂移感知公式允许该方法在具有挑战性的序列上操作，包括场景尺度的大的变化。 主要的推动力是两个关键的新颖之处：（1）一种新颖的直接跟踪方法，在sim（3）上运行，从而明确地检测尺度漂移，（2）优雅的概率解决方案，包括噪声深度值的影响跟踪。 由此产生的直接单目SLAM系统在CPU上实时运行。

5 [Norbert Haala and Martin Kada. An update on automatic 3d building reconstruction. ISPRS Journal of Photogrammetry and Remote Sensing, 65(6):570–580, 2010](http://www.sciencedirect.com/science/article/pii/S0924271610000894)《三维建筑物自动重建的最新进展 》
>摘要
>近二十年前，开发3D城市模型的工具就开始了。 从一开始，全自动重建系统就设想满足高效数据采集的需要。 然而，对城市自动建模的研究仍然是一个非常活跃的领域。 本文将回顾一些当前的方法，以全面阐述重建方法的现状和各自的原则。 最初，自动城市建模只针对多面体建筑物，主要反映了各自的屋顶形状和建筑物的脚印。 为此目的，使用空中图像或激光扫描。 除了这些发展之外，本文还将回顾目前从地面数据收集中生成更加详细的立面几何的方法。

6 [Jesse Levinson, Jake Askeland, Jan Becker, Jennifer Dolson, David Held, Soeren Kammel, J Zico Kolter,Dirk Langer, Oliver Pink, Vaughan Pratt, et al. Towards fully autonomous driving: Systems and algorithms. In Intelligent Vehicles Symposium (IV), 2011 IEEE, pages 163–168. IEEE, 2011.](http://ieeexplore.ieee.org/document/5940562/)《走向全自主驾驶：系统与算法 》
>摘要
>为了在交通不可预知的城市中实现车辆的自主运行，几个实时系统必须互操作，包括环境感知，本地化，规划和控制。此外，具有适当传感器，计算硬件，网络和软件基础设施的强大车辆平台是至关重要的。我们以前发表了“2007年DARPA城市挑战赛”中斯坦福大学Junior的概述。这场比赛是一场封闭式的比赛，虽然历史悠久，在这个领域取得了很大的进展，但并不能完全代表现实世界中存在的情况。在本文中，我们总结了我们最近的研究，旨在在更现实的情况下实现安全和强大的自主操作。首先，三个无监督算法自动校准我们的64光束旋转激光雷达，精度优于繁琐的手工测量。然后，我们生成环境的高分辨率地图，随后用于以厘米为单位的在线定位。现在，改进的感知和识别算法使Junior能够将障碍物追踪和分类为骑自行车者，行人和车辆;红绿灯也被检测到。一个新的计划系统使用这个输入数据来生成每秒数千个候选轨迹，动态地选择最佳路径。改进的控制器不断选择油门，刹车和转向驱动，从而最大限度地提高舒适性并最大限度地减少轨迹误差。所有这些算法都可以在阳光或雨中以及白天或黑夜中工作。通过这些系统的共同运作，Junior已经在各种现实条件下成功记录了数百英里的自主操作。

7.[US DoD. Global positioning system standard positioning service performance standard. Assistant secretary of defense for command, control, communications, and intelligence, 2001 ](http://xueshu.baidu.com/s?wd=paperuri:%285efb3bee0d5669b9cccf514725980b8f%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://trid.trb.org/view/894431&ie=utf-8&sc_us=6302811145085811136)《全球定位系统标准定位服务性能标准 》
>摘要
>美国全球定位系统（GPS）标准定位服务（SPS）由天文定位，导航和定时（PNT）信号组成，为全球和平的民用，商业和科学用途免费提供直接用户费用。 此SPS性能标准（SPS PS）根据广播信号参数和GPS星座设计来规定SPS性能的等级。 美国政府致力于达到并超越本SPS规定的最低服务水平，这一承诺已经编入美国法律（10U.S.C.2281（b））。

8.[Ben Bellekens, Vincent Spruyt, Rafael Berkvens,Rudi Penne, and Maarten Weyn. A benchmark survey of rigid 3d point cloud registration algorithms](https://www.researchgate.net/publication/280040097_A_Benchmark_Survey_of_Rigid_3D_Point_Cloud_Registration_Algorithms)《刚性三维点云配准算法的基准研究 》
>摘要
>先进的用户界面传感器能够使用特定的光学技术（如飞行时间，结构化光线或立体视觉）三维观察环境。由于现代传感器能够融合环境的深度和颜色信息的成功，出现了不同领域的新焦点。本调查研究了不同的最先进的配准算法，它们能够确定两个相应的三维点云之间的运动。本次调查从数学的角度出发，通过解释两种确定性的方法，即主成分分析（PCA）和奇异值分解（SVD），以及迭代最近点（ICP）及其变体等迭代方法。我们将不同算法的性能与基于真实世界数据集的精度和鲁棒性进行比较。这次调查的主要贡献包括基于现实世界数据集的性能基准，该数据集包括Microsoft Kinect相机的3D点云，以及不同匹配方法的数学概述，这些概念通常用于同时定位并绘制和3D扫描。我们的基准测试结果表明，ICP点对表面法是最精确的算法。除了精度，鲁棒性的结果，我们可以得出结论：在SVD方法之后应用ICP点对点方法的组合给出最小误差。

9 . [Radu Bogdan Rusu, Nico Blodow, and Michael Beetz.Fast point feature histograms (fpfh) for 3d registration.In Robotics and Automation, 2009. ICRA’09. IEEE International Conference on, pages 3212–3217. IEEE,2009](http://ieeexplore.ieee.org/document/5152473/?arnumber=5152473&tag=1)《快速点特征直方图（fpfh）三维配准 》
>摘要
>在我们最近的工作[\[1\] ](http://ieeexplore.ieee.org/document/4795593/)[\[2\]](http://ieeexplore.ieee.org/document/4650967/)中，我们提出了点特征直方图（PFH）作为鲁棒的多维特征，描述三维点云数据集点p周围的局部几何。在本文中，我们修改了它们的数学表达式，并针对重叠点云视图的3D配准问题对其稳健性和复杂性进行了严格的分析。更具体地说，我们提出几个优化，通过缓存先前计算的值或修改其理论公式来大幅减少计算时间。后者导致了一种新型的局部特征，称为快速点特征直方图（FPFH），其保留了PFH的大部分区分能力。此外，我们提出了一个在线计算实时应用的FPFH特征的算法。为了验证我们的结果，我们证明了它们的3D注册效率，并提出了一个新的基于样本共识的方法，将两个数据集带入本地非线性优化器的收敛区域：SAC-IA（SAmple Consensus Initial Alignment）。



10 .  [Radu Bogdan Rusu, Nico Blodow, Zoltan Csaba Marton, and Michael Beetz. Aligning point cloud views using persistent feature histograms. In 2008 IEEE/RSJ  International Conference on Intelligent Robots and Systems, pages 3384–3391. IEEE, 2008 ](https://www.researchgate.net/publication/221066028_Aligning_Point_Cloud_Views_using_Persistent_Feature_Histograms)
《使用持久特征直方图调整点云视图 》
>摘要
>在本文中，我们研究了将点云数据视图对齐成一个一致的全局模型的持久点特征直方图的使用情况。 给定噪声点云的集合，我们的算法估计了一组强大的16D特征，它们描述了每个点在本地的几何形状。 通过分析不同尺度上的特征的持续性，我们提取了一个最佳的表征给定点云的最优集合。 所产生的持久性特征被用于初始对准算法中以估计近似登记输入数据集的刚性变换。 该算法通过将数据集转换为其收敛盆地为迭代配准算法（如ICP（迭代最近点））提供了良好的起点。 我们表明，我们的方法是不变的构成和采样密度，并能应付来自室内和室外激光扫描的嘈杂数据。



11 .[Federico Tombari, Samuele Salti, and Luigi Di Stefano.Unique signatures of histograms for local surface description. In European conference on computer vision, pages 356–369. Springer, 2010 ](https://link.springer.com/chapter/10.1007/978-3-642-15558-1_26)《局部曲面描述的直方图唯一签名 》
>摘要
>本文涉及局部三维描述符的表面匹配。 首先，我们将现有的方法分为两类：签名和直方图。 然后，通过讨论和实验，指出了局部参照系唯一性和可重复性的关键问题。 基于这些观察，我们制定了一个新的表面表示的综合提案，其中包括一个新的独特和可重复的本地参考框架，以及一个新的三维描述符。 后者位于签名和直方图之间的交集处，以便在描述性和鲁棒性之间取得更好的平衡。 对公开可用数据集的实验以及使用Spacetime Stereo获得的范围扫描提供了对我们提议的全面验证。

12 .  [Paul Scovanner, Saad Ali, and Mubarak Shah. A 3-dimensional sift descriptor and its application to action recognition. In Proceedings of the 15th ACM international conference on Multimedia, pages 357–360. ACM, 2007. ](http://xueshu.baidu.com/s?wd=paperuri:%28d27927f10820b09c2b7fbb1d5fb7944c%29&filter=sc_long_sign&sc_ks_para=q=A%203-dimensional%20sift%20descriptor%20and%20its%20application%20to%20action%20recognition&tn=SE_baiduxueshu_c1gjeupa&ie=utf-8&sc_us=5087117483461663566)《一种三维SIFT描述子及其在动作识别中的应用》
>摘要
>在本文中，我们引入一个3维SIFT用于视频或3D图像（如MRI数据）的描述符。 我们还展示了这种新的描述符如何能够更好地表现视频数据在动作识别应用中的3D特性。 本文将展示3D SIFT如何以优雅和高效的方式超越以前使用的描述方法。 我们用一包词的方式来表示视频，并提出一种方法来发现时空词之间的关系，以便更好地描述视频数据。



13 .  [Bastian Steder, Radu Bogdan Rusu, Kurt Konolige,and Wolfram Burgard. Narf: 3d range image features for object recognition. In Workshop on Deﬁning and Solving Realistic Perception Problems in Personal Robotics at the IEEE/RSJ Int. Conf. on Intelligent Robots and Systems (IROS), volume 44, 2010](https://www.researchgate.net/publication/260320178_NARF_3D_Range_Image_Features_for_Object_Recognition)《物体识别的三维距离图像特征》
>摘要
>我们提出了一个新的方法：对于兴趣点检测和特征描述符计算三维范围数据，我们把它称为NARF（正常对齐径向特征）。 该方法明确地使用了对象边界信息，并试图提取表面稳定但附近有实质性变化的区域的特征。

14 . [Alex Krizhevsky, Ilya Sutskever, and Geoffrey E Hinton. Imagenet classiﬁcation with deep convolutional neural networks. In Advances in neural information processing systems, pages 1097–1105, 2012](http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks)《基于深度卷积神经网络的图像分类》[『中文翻译』](http://blog.csdn.net/motianchi/article/details/50851074)
>摘要
>我们训练了一个大型深度卷积神经网络来将ImageNet LSVRC-2010数据集中的120万张高清图片分到1000个不同的类别中。在测试数据中，我们将Top-1错误（分配的第一个类错误）和Top-5错误（分配的前五个类全错）分别降到了37.5%和17.0%，这比之前的技术水平要好得多。这个神经网络拥有6千万的参数和65万个神经元，共有五个卷积层，其中一些卷积层后面跟着最大池化层，还有利用softmax函数进行1000类分类的最后三个全连接层。为了让训练速度更快，我们使用不饱和【？non-saturating】神经元，并利用高效的GPU实现卷积操作。为了减少全连接层的过拟合，我们采用了一种最近研发出来的正则化方法——“DROPOUT”，它被证明十分有效。我们也在比赛中加入了这一模型的一个变体，第二名的26.2%相比，我们通过将TOP-5错误降到了15.3%而获胜。

15.[Yann LeCun, Yoshua Bengio, and Geoffrey Hinton.Deep learning. Nature, 521(7553):436–444, 2015](https://www.nature.com/nature/journal/v521/n7553/full/nature14539.html)《深度学习》
>摘要
>深度学习允许由多个处理层组成的计算模型来学习具有多个抽象级别的数据表示。 这些方法极大地改进了语音识别，视觉对象识别，对象检测和诸如药物发现和基因组学等许多其他领域的最新技术。 深度学习通过使用反向传播算法来发现大型数据集中复杂的结构，以指示机器应如何改变其内部参数，以用于根据前一层的表示计算每层中的表示。 深卷积网络在处理图像，视频，语音和音频方面取得了突破性进展，而经常性网络则对文本和语音等连续数据指明了方向。

16.[Geoffrey E Hinton and Ruslan R Salakhutdinov. Reducing the dimensionality of data with neural networks. Science, 313(5786):504–507, 2006.](http://www.cs.toronto.edu/~hinton/science.pdf)《用神经网络降低数据的维数》[中文介绍](https://segmentfault.com/a/1190000007665145)
>摘要
>通过训练具有小中心层的多层神经网络来重建高维输入向量，可将高维数据转换为低维码。 梯度下降可用于微调这种“自动编码器”网络中的权重，但只有在初始权重接近良好的解决方案时，才能正常工作。 我们描述了一个初始化权重的有效方法，允许深度自动编码器网络学习低于主要组件分析的低维代码作为降低数据维度的工具。

17 .  [Hangbin Wu and Hongchao Fan. Registration of airborne lidar point clouds by matching the linear plane features of building roof facets. Remote Sensing,8(6):447, 2016.](https://www.researchgate.net/publication/303531275_Registration_of_Airborne_LiDAR_Point_Clouds_by_Matching_the_Linear_Plane_Features_of_Building_Roof_Facets)《基于线性平面有限元匹配的机载激光雷达点云配准》
>摘要
>通过寻找和匹配相应的线性面特征，提出了一种新的机载LiDAR点云配准方法。线性平面特征是城市地区的一种常见特征，便于从点云中获取特征参数。利用这些线性特征参数，采用三维刚体协调变换模型，对不同轨迹的点云进行配准。该方法由三个步骤组成。在第一步中，应用OpenStreetMap辅助方法来选择简单结构的屋顶对作为匹配的相应屋顶面。在第二步中，计算所选屋顶小面的法向矢量并将其输入到一个超定的观测系统中，以估计配准参数。在第三步中，使用这些参数进行匹配。选择一个具有两个轨迹点云的案例数据集来验证所提出的方法。为了评估匹配后点云的准确性，手动选择了40个检查点;评估结果表明，一般精度为0.96米，约为点云分辨率的1.6倍。此外，选择两个重叠区域来测量两个轨迹之间的表面差异。根据分析结果，平均表面距离约为0.045-0.129米。



[18] [Paul J Besl and Neil D McKay. Method for registration of 3-d shapes. In Robotics-DL tentative, pages 586–606. International Society for Optics and Photonics, 1992.](http://ieeexplore.ieee.org/document/121791/?arnumber=121791&tag=1)《三维形状的配准方法 》

>摘要
>作者描述了一种通用的，与表示无关的方法，用于准确计算包括自由曲线和曲面在内的三维形状的高效配准。该方法处理完整的六个自由度，并基于迭代最近点（ICP）算法，该算法仅需要一个过程来找到几何实体上到给定点的最近点。 ICP算法总是单调地收敛到均方距离度量的最近局部最小值，并且在最初的几次迭代期间收敛速度是快速的。因此，对于具有一定“形状复杂度”的特定类别的对象，给定适当的初始旋转和平移集合，可以通过测试每个初始配准来在全部六个自由度上全局最小化均方距离度量。这种方法的一个重要应用是在形状检查之前将来自未固定的刚性物体的感测数据与理想的几何模型进行配准。实验结果显示了配准算法在点集，曲线和曲面上的能力





[19] [Szymon Rusinkiewicz and Marc Levoy. Efﬁcient variants of the icp algorithm. In 3-D Digital Imaging and Modeling, 2001. Proceedings. Third International Conference on, pages 145–152. IEEE, 2001.](http://ieeexplore.ieee.org/abstract/document/924423/)《ICP算法的有效变种 》
>摘要
>当相对姿态的初始估计已知时，ICP（迭代最近点）算法被广泛用于三维模型的几何对准。 已经提出了ICP的许多变体，从点的选择和匹配到最小化策略影响算法的所有阶段。 我们列举和分类许多这些变体，并评估它们对达到正确对齐的速度的影响。 为了改善具有小特征的近平面网格（如刻写曲面）的收敛性，引入了一个基于法向空间均匀采样的新的变体。 最后，我们提出了针对高速优化的ICP变体的组合。 我们演示一个能够在几十毫秒内对齐两个距离图像的实现，假设一个好的初始猜测。 这种能力可用于实时三维模型采集和基于模型的跟踪。


[20] [Ameesh Makadia, Alexander Patterson, and Kostas Daniilidis.Fully automatic registration of 3d point clouds. In 2006 IEEE Computer Society Conference on Computer Vision and Pattern Recognition (CVPR’06), volume 1, pages 1297–1304. IEEE, 2006.](http://xueshu.baidu.com/s?wd=paperuri:%28474b882d8a591cde1db98d7ff1f766b3%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://ieeexplore.ieee.org/xpls/abs_all.jsp?arnumber=1640899&ie=utf-8&sc_us=16791238355415665744)《三维点云的全自动配准》
>摘要
>我们提出了一种三维点云匹配的新技术，这种技术做了很少的假设：我们避免任何手动粗略对齐或使用地标，位移可以任意大，两点集合可以有很少的重叠。 粗对准是通过估计来自两个扩展高斯图像的3D旋转来实现的，即使当引起它们的数据集具有部分重叠时也是如此。该技术基于傅立叶域中两个EGI的相关性，并利用球面和旋转谐波变换。对于具有低重叠的配对而言，不能进行关键的验证步骤，可以通过对齐由EGI生成的星座图像来获得旋转对准。 旋转对齐的组通过使用体积函数的傅里叶变换的相关性来匹配。 在最后一步中，通过只用很少的迭代运行迭代最接近的点来获得精确的对齐。


[21] [GD Scott and DM Kilgour. The density of random close packing of spheres. Journal of Physics D: Applied Physics, 2(6):863, 1969.](https://www.researchgate.net/publication/230991730_The_Density_of_Random_Close_Packing_of_Spheres)《球体随机密堆积的密度》
>摘要
>随机包装硬球的模型表现出简单液体性质的一些特征，例如， 包装密度和径向分布。 球体的最大填充密度的值可以由模型确定，如果注意确保边界表面的随机堆积和如果对边界处的体积误差进行校正的话。 随机松散密度和随机密集填充密度的实验报告是用有机玻璃，尼龙和钢球在空气中的八分之一，还有浸在油中的钢球。 已经用多达80 000个钢球并借助机械振动器进行了一系列随机密堆积密度的测量。 计算机对结果的分析允许一步，双参数外推到无限量。 如此获得的随机密堆积密度的数字是06366±00005，这表示精度比先前的结果提高了一个数量级。


 [22] [Yann LeCun et al. Generalization and network design strategies. Connectionism in perspective, pages 143–155, 1989.](http://xueshu.baidu.com/s?wd=paperuri:%28479376c9d4a3d372841ed9b2acbb0f8e%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://citeseerx.ist.psu.edu/viewdoc/download;jsessionid=FFC68D55B6C24229FF3D33D0891ED180?doi=10.1.1.476.479&rep=rep1&type=pdf&ie=utf-8&sc_us=8907340944497699214)《通用化与网络设计策略 》
>摘要
>连接系统的一个有趣的特性是他们从例子中学习的能力。尽管该领域最近的工作集中于减少学习时间，但是学习机的最重要的特征是它的泛化性能。人们通常认为，除非对任务有一些了解，否则就不能实现对现实世界问题的良好泛化性能。反向传播网络通过对网络的体系结构和权重施加约束来提供指定这种知识的方式。一般来说，这样的参数可以看作是参数空间的特定变换。建立一个图像识别的约束网络似乎是一个可行的任务。我们描述了一个小的手写数字识别问题，表明即使问题是线性可分的，单层网络也表现出较差的广义性能。在层次结构中使用移位不变特征检测器时，多层约束网络在此任务上表现得非常好。这些结果证实了最小化网络中的自由参数数量的观点增强了概括性。

[23] [Terence D Sanger. Optimal unsupervised learning in a single-layer linear feedforward neural network. Neural networks, 2(6):459–473, 1989.](https://www.researchgate.net/publication/222464584_Optimal_Unsupervised_Learning_in_a_Single-Layer_Linear_Feedforward_Neural_Network)《在单层线性前馈神经网络最优的无监督学习 》
>摘要
>讨论了单层线性前向神经网络中无监督学习的一种新方法。提出了一种基于保留输出单元中最大信息的最优性原则。介绍了一种基于Hebbian学习规则的无监督学习算法，实现了理想的最优性。该算法找到了输入相关矩阵的特征向量，证明了其以概率1收敛。描述了仅使用局部“突触”修改规则训练神经网络的实现。结果表明，该算法与统计学（因子分析和主成分分析）和神经网络（自监督反向传播，或“编码器”问题）中的算法密切相关。因此，它提供了对经典统计技术方面的某些神经网络行为的解释。提出了使用线性网络来解决图像编码和纹理分割问题的例子。此外，它表明，该算法可以用来找到“视觉接受领域”在性质上类似于在灵长类视网膜和视觉皮层中发现的。


[24] [Martin A Fischler and Robert C Bolles.Random sample consensus: a paradigm for model ﬁtting with applications to image analysis and automated cartography. Communications of the ACM, 24(6):381–395,1981.](https://dl.acm.org/citation.cfm?id=358692)《随机样本一致性：模型与应用程序进行图像分析和自动绘图的范例》
>摘要
>介绍了一个新的范例，即随机样本一致性（RANSAC），用于拟合实验数据模型。 RANSAC能够解释/平滑包含大量重大错误的数据，因此非常适合自动图像分析应用，其中解释基于易错特征检测器提供的数据。 本文的一个主要部分描述了RANSAC在位置确定问题（LDP）中的应用：给定描绘具有已知位置的一组地标的图像，确定获得图像的空间点。 为了响应RANSAC的要求，需要获得解决方案所需的最小标记数量的新结果，并给出算法以计算封闭形式的这些最小标志性解决方案。这些结果为在困难的观察和分析条件下能够解决LDP的自动化系统提供了基础。 还介绍了实现细节和计算示例。



[25] [Kun Zhou, Qiming Hou, Rui Wang, and Baining Guo.Real-time kd-tree construction on graphics hardware.ACM Transactions on Graphics (TOG), 27(5):126,2008.](http://xueshu.baidu.com/s?wd=paperuri:%282db80d9a80bd71417232d7c26c888f97%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://dl.acm.org/citation.cfm?id=1409079&ie=utf-8&sc_us=1406914190655001141)《基于图形硬件的实时KD树构造 》
>摘要
>我们提出了一个在GPU上构建kd-tree的算法。该算法通过在kd树构造的各个阶段利用GPU的流式架构来实现实时性能。与以前的并行kd-tree算法不同，我们的方法完全以BFS（广度优先搜索）顺序构建树节点。我们还针对上级树的大型节点制定了一个特殊策略，以进一步利用GPU的细粒度并行性。对于这些节点，我们并行化所有几何基元的计算，而不是每个层次上的节点。最后，为了保持kd树质量，我们引入了新的方案来快速评估节点分裂成本。

>据我们所知，我们是GPU上的第一个实时kd-tree算法。我们的算法构建的kd树与离线CPU算法构建的kd树质量相当。就速度而言，我们的算法明显快于优化的单核CPU算法，并且与多核CPU算法相比具有竞争性。我们的算法提供了处理GPU上动态场景的一般方法。我们证明了我们的算法在涉及动态场景的应用中的潜力，包括GPU射线追踪，交互式光子映射和点云建模。


[26] [Donghwa Lee, Hyongjin Kim, and Hyun Myung.Gpu-based real-time rgb-d 3d slam. In Ubiquitous Robots and Ambient Intelligence (URAI), 2012 9th International Conference on, pages 46–48. IEEE, 2012.](http://xueshu.baidu.com/s?wd=paperuri:%283ce333febdb3c15020256f7c740c1c7b%29&filter=sc_long_sign&tn=SE_xueshusource_2kduw22v&sc_vurl=http://ieeexplore.ieee.org/document/6462927/&ie=utf-8&sc_us=3781435091169552354)《基于GPU的实时深度3D SLAM》
>摘要
>本文提出了一种基于GPU（图形处理单元）的实时RGB-D（红 - 绿 - 蓝）深度的三维SLAM（同时定位和映射）系统。 RGB-D数据包含二维图像和每像素深度信息。 首先，通过具有图像特征的3D-RANSAC（三维随机采样共识）算法获得6自由度视觉测距。 而投影ICP（迭代最近点）算法给出了具有深度信息的精确的测距估计结果。 为了加速提取特征和ICP计算，执行基于GPU的并行计算。 在检测到闭环后，基于图的SLAM算法优化了传感器的轨迹和三维图。




[27] [Deyuan Qiu, Stefan May, and Andreas Nüchter.Gpu-accelerated nearest neighbor search for 3d registration.In International Conference on Computer Vision Systems, pages 194–203. Springer, 2009.](https://www.researchgate.net/publication/221410064_GPU-Accelerated_Nearest_Neighbor_Search_for_3D_Registration)《GPU加速最近邻三维搜索 》
>摘要
>最近邻搜索（NnS）被许多计算机视觉算法所采用。 计算复杂度很大，对实时性能构成挑战。 基本的问题是快速处理大量的数据，这通常是通过高度复杂的搜索方法和并行处理来解决的。 我们展示了像迭代最近点算法（ICP）那样的基于NNS的视觉算法可以实现实时能力，同时保持紧凑的尺寸和适中的能量消耗，因为它在机器人和许多其他领域中是需要的。 该方法利用图形处理单元（GPGPU）上的通用计算概念，并与CPU上的并行处理进行比较。 我们将这种方法应用于三维扫描注册问题，与顺序CPU实现相比，加速因子为88。


[28] [François Pomerleau, Ming Liu, Francis Colas, and Roland Siegwart. Challenging data sets for point cloud registration algorithms. The International Journal of Robotics Research, 31(14):1705–1711, 2012.](http://xueshu.baidu.com/s?wd=paperuri:%28d1cecbd6c9b21c9caf0355aebfa848db%29&filter=sc_long_sign&sc_ks_para=q=Challenging%20data%20sets%20for%20point%20cloud%20registration%20algorithms&tn=SE_baiduxueshu_c1gjeupa&ie=utf-8&sc_us=6333769492514580415)《用于点云配准算法的具有挑战性的数据集 》
>摘要
>最近文献中匹配方案的数量已经开始出现。例如，迭代的最近点可以被认为是许多基于激光的定位和测绘系统的骨干。虽然它们被广泛使用，但在公平的基础上比较匹配解决方案是一个常见的挑战。主要限制是克服当前数据集中缺乏准确的基本事实，这些数据集通常涵盖仅在一个小范围的组织级别的环境。在计算机视觉领域，斯坦福三维扫描库通过提供高质量的扫描对象和精确定位，推动了点云配准算法和对象建模领域。我们的目标是为机器人和计算机视觉社区提供类似的高质量工作材料，但是以风景而不是物体。我们提出在现代机器人容易遇到的覆盖环境多样性的地点采集八个点云序列，从公寓内部到林地区域。数据集的核心由三维激光点云组成，每个姿态都提供支持数据（重力，磁北和GPS）。为了确保扫描仪的全球定位精度在毫米范围内，不受环境条件的影响，我们做了特别的努力。这将允许在映射具有挑战性的环境（例如在现实世界中发现的环境）时改进匹配算法的开发


[29] [Yanxin Ma, Yulan Guo, Jian Zhao, Min Lu, Jun Zhang, and Jianwei Wan. Fast and accurate registration of structured point clouds with small overlaps.](http://ieeexplore.ieee.org/document/7789576/)《小重叠结构点云的快速准确配准 》
>摘要
>为了进行大旋转，小重叠的结构化点云的配准，本文提出了一种基于密集点的方向角和投影信息的算法。 该算法充分利用了结构化环境的几何信息。 它由两部分组成：旋转估计和翻译估计。 对于旋转估计，为点云定义方向角，然后通过比较角度分布之间的差异来获得旋转矩阵。 对于平移估计，将点云投影到三个正交平面上，然后对投影图像执行相关运算以计算平移矢量。 已经对几个数据集进行了实验。 实验结果表明，该算法在精度和效率两方面都优于现有技术。

[30] Elbit Systems Ltd. 
>Elbit系统有限公司 是一家总部位于以色列的国际防务电子公司











[^footnote].
  [^footnote]: 这里是 **脚注** 的 *内容*.

