# 绪论

储存
- 2D图像在计算机中用f(x,y)表示图像，灰阶常取0-255
- 3D图像用f(i,j,k)，4D图像用f(i,j,k,t)
三要素
- 像素
- 灰度
- 坐标
医学图像分类
- CT
- MRI
- X-光
- 超声
- 正电子成像
- 三维超声

# 医学图像配准

医学影像模态多，能够反映的人体特征也不同，配准可以让不同模态的图像在同一空间下进行叠加反映特征。体现为不同图像在空间坐标上的一致或视觉感上的一致

也称为图像融合(image fusion)，图像叠加(image superimposition)，图像匹配(image matching)或图像混合(image merge)

配准过程中需要一张移动图像(Moving Image)和一张固定图像(Fixed Image)

## 配准的基本原理

![[医学图像配准框架.png]]
<center><font color=silver>图6.1 医学图像配准框架</font></center>
移动图像参照固定图像做形变，形变过程中有可能会丢失像素信息，所以需要进行插值恢复丢失的信息，然后计算两幅图像的相似度，根据相似度进行优化迭代求解

### 配准的分类

维度：
>- 2D-2D
>- 3D-3D
>- 2D-3D

配准特征：
> - 基于图像的
>	- 外在特征
>	- 内在特征
> - 不基于图像

按转换方法：
>- 刚体
>- 仿射
>- 投影
>- 曲线

按交互方法：
>- 交互式
>- 半自动化
>- 自动化

涉及模态：
>- 单模态
>- 多模态
>- 模态到模型

对象：
>- 同一个体不同时间
>- 不同个体之间
>- Atlas解剖图谱

转换域：
>- 本地
>- 全局

优化过程

### 坐标系转换

- 刚体
> 只允许平移和旋转
- 仿射
> 允许缩放和裁剪
- 曲线
> 允许直线到曲线的映射
- 投影
> 不需要保证线之间的平行约束

### 配准算法

==寻找坐标系转换的算法==
**刚体和仿射**
- 基于地标/特征点(Landmark)
Landmark类型：
内在的：解剖特征点； 外在的：成像前给病人身上贴上标识
计算Landmark之间的关系，找到他们的中心，或者通过旋转，使点之间距离(最小方差)最小
- 基于曲面
Head and Hat算法
迭代最近点算法
使用脊柱线的配准
- 基于信息论
减少联合熵(Joint Entropy)；增加互信息
- 基于边缘
- 基于Voxel强度(衡量灰度一致性)
计算联合灰度直方图(Joint Histogram)，移动图像找到最大直方图

**非刚体**
- 基函数
Fourier基函数；三角基函数能够很好地表现形变场的谱特征，从而反映出形变场；小波基函数
- 样条函数
在Landmarks之间进行样条差值
- 物理配准
	- 弹性
	- 流体
	- 光流场
弹性：针对局部、非线性(如大脑中肿瘤造成的组织结构的形变)，可以使用Navier线弹性PDE描述
流体：将图像建模为高粘性流体
机械模型：使用一个三元件模型模拟刚体、弹性体和流体结构的性质
==配准中的优化算法==
首先假设一个初始的配准，迭代过程中，使用当前估计变换来计算相似度测度
==可视化==
- 彩色重叠
- 并行显示
- 图像减法
- 象棋盘融合(Chessboard Fusion)
- 动态变换
==验证==
图像配准没有一个金标准，那么该如何评价配准效果呢？使用计算机对图像进行一系列变换，如平移、旋转、放缩，此时我们已知图像做过的变换，然后让配准算法进行配准观察配准结果
还可以利用物理虚假模型(Phantom)
临床上，视觉评价、解剖结构差异比对、手动制作金标准都是常用方法

## 刚体配准

实际上刚体配准是线性配准的一种，刚体配准也分为Rigid3D、Rigid2D，还有仿射配准、B样条插值都属于刚体配准

### 线性配准

1. 平移、放缩、旋转
2. 不允许局部形变

### 变换的数学描述

#### 2D变换

$$
\begin{equation}
\begin{aligned}
平移：x_1&=x_0+0\cdot y_0+t_x\\
y_1&=y_0+0\cdot x_0+t_y\\
旋转：x_1&=\cos\theta x_0+\sin\theta y_0+0\\
y_1&=-sin\theta x_0+\cos\theta y_0+0\\
放缩：x_1&=s_xx_0+0\cdot y_0+0\\
y_1&=s_yy_0+0\cdot x_0+0\\
剪切：x_1&=x_0+hy_0+0\\
y1&=y_0+0\cdot x_0 +0
\end{aligned}
\end{equation}
$$
使用如上表述方式可以在编写程序时使用矩阵进行计算，不需对各变换进行单独处理

#### 3D变换

$$
\begin{aligned}
\begin{pmatrix}
1\quad 0\quad 0\quad &x_{trans}\\
0\quad 1\quad 0\quad &y_{trans}\\
0\quad 0\quad 1\quad &z_{trans}\\
0\quad 0\quad 0\quad &1
\end{pmatrix}_{T}
&\times
\begin{pmatrix*}[c]
1\qquad 0\qquad 0\qquad 0\\
0\quad \cos\Phi\quad \sin\Phi\quad 0\\
0\quad {-\sin\Phi}\quad \cos\Phi\quad 0\\
0\qquad 0\qquad 0\qquad 1
\end{pmatrix*}_{P}\\
\times
\begin{pmatrix*}[c]
\cos\Theta\quad 0\quad \sin\Theta\quad 0\\
0\qquad 1\qquad 0\qquad 0\\
-\sin\Theta\quad 0\quad \cos\Theta\quad 0\\
0\qquad 0\qquad 0\qquad 1
\end{pmatrix*}_{R}
&\times
\begin{pmatrix*}[c]
\cos\Omega\quad \sin\Omega\quad 0\quad 0\\
-\sin\Omega\quad \cos\Omega\quad 0\quad 0\\
0\qquad 0\qquad 1\qquad 0\\
0\qquad 0\qquad 0\qquad 1
\end{pmatrix*}_{Y}
\end{aligned}
$$
