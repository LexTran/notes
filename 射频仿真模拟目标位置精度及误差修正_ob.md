# 射频仿真模拟目标位置精度及误差修正
蒋庆平，刘晓宁，阎杰

---
## 背景
射频目标模拟器的作用
- 复现目标的空间属性
> 距离、距离变化率、角度、角度变化率
- 复现目标的射频信号特征
> 幅度、相位、频率、幅度起伏、角闪烁、极化

---
## 三元组工作原理
三元组：射频辐射单元按一定规律排成一个阵列，阵列中相邻的三个单元按等边三角形排列构成的子阵列称为三元组
三元组中三个单元辐射的合成信号表示模拟器模拟的目标信号，利用==射频开关矩阵==实现目标模拟信号在三元组间的转移，每个单元的射频信号分别通过==程控衰减器==和==移相器==改变相对幅度和相位

---
## 角闪烁方程推导
角闪烁方程是目标角运动模拟中的关键理论
天线互易原理：一般天线都具有可逆性，即同一副天线既可用作发射天线，也可用作接收天线，同一天线作为发射天线或接收天线的基本特性参数是相同的。
角闪烁方程即求雷达在三元组天线三个顶点上的电场矢量的合成矢量$\vec{E}=\vec{E_1}+\vec{E_2}+\vec{E_3}$的方位角$\phi$和俯仰角$\theta$
每个矢量$E_i$可作如下分解(直角坐标-球坐标转换):
$$\begin{cases}
x_i=E_i\cos \theta_i \cos\phi_i\\
y_i=E_i\cos \theta_i \sin\phi_i\\
z_i=E_i\sin\theta_i
\end{cases}\qquad (1)$$
其中$E_i$是矢量$\vec{E_i}$的幅度。合成矢量$\vec{E}$的二维角度坐标分量的正切函数值为
$$\begin{cases}
\tan \phi=\frac{y}{x}\\
\tan \theta=\frac{z}{(x^2+y^2)^{\frac{1}{2}}}
\end{cases}\qquad (2)$$
将三个分矢量分解式代入上述式计算
$$
\tan \phi=\frac{\sum_{i=1}^3E_i\cos \theta_i \sin\phi_i}{\sum_{i=1}^3E_i\cos \theta_i \cos\phi_i}\qquad (3-1)$$
$$=\frac{\sum_{i=1}^3E_i \sin\phi_i}{\sum_{i=1}^3E_i \cos\phi_i}=\frac{\sum_{i=1}^3E_i\tan\phi_i}{\sum_{i=1}^3E_i}
$$
$$\tan \theta=\sum_{i=1}^3E_i\sin\theta_i /\{\sum_{i=1}^3E_i^2\cos^2{\theta_i} + 2[E_1E_2\cos\theta_1\cos\theta_2\cos(\phi_1-\phi_2) + E_1E_3\cos\theta_1\cos\theta_3\cos(\phi_1-\phi_3) + E_2E_3\cos\theta_2\cos\theta_3\cos(\phi_2-\phi_3)] \}^\frac{1}{2}\qquad (3-2)$$
==(注此处推导存疑)==
由于$\phi$、$\theta$、$\phi_i$、$\theta_i$、$(\phi_i-\phi_j)_{i\neq j}$均为几十毫弧度内的小角度(约等于0)，则其余弦值均趋于1
故上述方程可简化为
$$\sum_{i=1}^3E_i\sin\theta_i /\{\sum_{i=1}^3E_i^2\cos^2{\theta_i} + 2[E_1E_2 + E_1E_3 + E_2E_3] \}^\frac{1}{2}$$
$$=\frac{\sum_{i=1}^3E_i\sin\theta_i}{[(E_1+E_2+E_3)^2]^\frac{1}{2}}=\frac{\sum_{i=1}^3E_i\sin\theta_i}{\sum_{i=1}^3E_i}$$
故角闪烁方程如下
$$\begin{cases}
\phi=\frac{\sum_{i=1}^3E_i\phi_i}{\sum_{i=1}^3E_i}\\
\theta=\frac{\sum_{i=1}^3E_i\theta_i}{\sum_{i=1}^3E_i}
\end{cases}\qquad (4)$$

---
## 天线阵仿真目标的位置
简单起见，先讨论二元组
	$设接受点P位于(x,r)处$，$且r>>d$
其中，$d$是两个单元间距离
有$\theta=\sin\theta=\frac{x}{r}$，则$r_1\approx r+\frac{d}{2}\sin\theta$，$r_2\approx r-\frac{d}{2}\sin\theta$
在$P$点处合成场可以用两个单元的场矢量合成，即$$E=E_1e^{-2jkr_1}+E_2e^{-jkr_2}$$
$$=(\frac{E_1+E_2}{2}+\frac{E_1-E_2}{2})e^{-jkr_1}+(\frac{E_1+E_2}{2}-\frac{E_1-E_2}{2})e^{-jkr_2}$$
代入上述$r_1$、$r_2$与$r$之间关系可得
$$E=(\frac{E_1+E_2}{2}+\frac{E_1-E_2}{2})e^{-jk(r+\frac{d}{2}\sin\theta)}+(\frac{E_1+E_2}{2}-\frac{E_1-E_2}{2})e^{-jk(r-\frac{d}{2}\sin\theta)}$$
$$=(\frac{E_1+E_2}{2}+\frac{E_1-E_2}{2})e^{-jkr} e^{-jk\frac{d}{2}\sin\theta}+(\frac{E_1+E_2}{2}-\frac{E_1-E_2}{2})e^{-jkr}e^{jk\frac{d}{2}\sin\theta}$$
$$=e^{-jkr}[(\frac{E_1+E_2}{2}+\frac{E_1-E_2}{2})e^{-jk\frac{d}{2}\sin\theta}+(\frac{E_1+E_2}{2}-\frac{E_1-E_2}{2})e^{jk\frac{d}{2}\sin\theta}]$$
由欧拉公式$e^{jx}=\cos x+j\sin x$，上式可写为
$$=e^{-jkr}[(\frac{E_1+E_2}{2}+\frac{E_1-E_2}{2})(\cos (\frac{kd}{2}\sin\theta)-j\sin(\frac{kd}{2}\sin\theta))$$
$$+(\frac{E_1+E_2}{2}-\frac{E_1-E_2}{2})(\cos (\frac{kd}{2}\sin\theta)+j\sin(\frac{kd}{2}\sin\theta))]$$
化简可得
$$E=e^{-jkr}[\frac{E_1+E_2}{2}\cos(\frac{kd}{2}\sin\theta)+\frac{E_1+E_2}{2}\cos(\frac{kd}{2}\sin\theta)$$
$$-j\frac{E_1-E_2}{2}\sin(\frac{kd}{2}\sin\theta)-j\frac{E_1-E_2}{2}\sin(\frac{kd}{2}\sin\theta)]$$
$$=e^{-jkr}[(E_1+E_2)\cos(\frac{kd}{2}\sin\theta)-j(E_1-E_2)\sin(\frac{kd}{2}\sin\theta)]$$
$$=e^{-jkr}[(E_1+E_2)\cos(\frac{kd}{2}\sin\theta)+j(E_2-E_1)\sin(\frac{kd}{2}\sin\theta)]$$
合成场的相位为$$\phi = -kr+\xi=-kr+\arctan[\frac{E_2-E_1}{E_2+E_1}\tan(\frac{kd}{2}\sin\theta)]\qquad (5)$$
在极坐标系下求梯度
$$\nabla\phi=-\vec{r}k+\vec{\theta}\frac{\partial\xi}{r\partial{\theta}}\qquad (6)$$
==注原文使用$\hat{r}$和$\hat{\theta}$符号，虽然未解释符号含义，但是根据梯度的定义，应该是极坐标系下的单位向量，故改为向量符号更易理解==
==另，此处梯度中第二项分母位置的$r$来源于极坐标下梯度推导==
$$\frac{\partial\xi}{\partial{\theta}}=\frac{\frac{E_2-E_1}{E_2+E_1}\sec^2(\frac{kd}{2}\sin\theta)\frac{kd}{2}\cos\theta}{1+[\frac{E_2-E_1}{E_2+E_1}\tan(\frac{kd}{2}\sin\theta)]^2}$$
当$P$点位于$Z$轴上，即$x=0$时，可知$x=r\sin\theta=0$，且$r\neq 0$，则可知$\theta=0$，代入后上式可化简为
$$\frac{\partial\xi}{\partial{\theta}}=\frac{E_2-E_1}{E_2+E_1}\frac{kd}{2}$$
故
$$\nabla \phi=-\vec{r}k+\vec{\theta}\frac{E_2-E_1}{E_2+E_1}\frac{kd}{2r}\qquad (7)$$
==TODO==
整理变换可得$$E_1(\frac{d}{2}+x_0)=E_2(\frac{d}{2}-x_0)\qquad (8)$$
若将$E_1$和$E_2$视作力，则上式表明两个单元到辐射中心$x_0$点力矩平衡，此结论可推广到三元组。
	$设三元组三个辐射信号幅度为E_1、E_2、E_3$，$各天线没有相位差$
则此时辐射中心位置$(x_0,y_0)$对于$E_1、E_2、E_3$的力矩在$x$、$y$方向平衡，即
$$\begin{cases}\sum_{i=1}^3R_i\vec{x}E_i=0\\
\sum_{i=1}^3R_i\vec{y}E_i=0
\end{cases}\qquad (9)$$
其中$R_i$为第$i$个单元到辐射中心位置的距离
可以反过来求得$$\begin{cases}x_0=\frac{(E_1-E_3)d}{2(E_1+E_2+E_3)}\\
y_0=\frac{\sqrt{3}(2E_2-E_1-E_3)d}{6(E_1+E_2+E_3)}
\end{cases}\qquad (10)$$

---
## 角闪烁方程的解
将$E_{in}$归一化，即$\sum_{i=1}^3E_i=1$，则上述角闪烁方程变为如下形式
$$\begin{cases}\phi=\sum_{i=1}^3E_i\phi_i\\
\theta=\sum_{i=1}^3E_i\theta_i\end{cases}\qquad (11)$$
现已知目标出现在目标模拟阵列中的角度位置为$(\phi,\theta)$，假设三元组的角度位置分别为$(\phi_1, \theta_1),(\phi_1+\frac{\phi_0}{2}, \theta_1+\theta_0),(\phi_1+\phi_0, \theta_1)$，其中$\phi_0$和$\theta_0$分别是相邻两列相邻两行的两个天线的方位角差值和俯仰角差值。
则与$E_1+E_2+E_3=1$联立可得
$$E_1\phi_1+E_2(\phi_1+\frac{\phi_0}{2})+E_3(\phi_1+\phi_0)=\phi$$
$$E_1\theta_1+E_2(\theta_1+\theta_0)+E_3\theta_3=\theta$$
解三元一次方程组(以$E_1$$E_2$$E_3$为三个变量)可推得
$$\begin{cases}
E_1=1-\frac{\theta-\theta_1}{2\theta_0}-\frac{\phi-\phi_1}{\phi_0}\\
E_2=\frac{\theta-\theta_1}{\theta_0}\\
E_3=\frac{\phi-\phi_1}{\phi_0}-\frac{\theta-\theta_1}{2\theta_0}
\end{cases}\qquad (12)$$

---
## 修正量求解
建立阵列坐标系(参考方辉煜-《对空导弹武器系统仿真》第七章雷达目标及环境仿真)
坐标原点处于三轴转台回转中心，三个单元的位置分别是$A(R_0,\theta_1,\phi_1),B(R_0,\theta_2,\phi_2),C(R_0,\theta_3,\phi_3)$，其中$R_0$是阵面上三元组天线口径面到转台回转中心的距离(注意需满足远场条件)。
将之转换成直角坐标
$$(R_0\cos\theta_i\cos\phi_i,R_0\cos\theta_i\sin\phi_i,R_0\sin\theta_i)$$
导引头天线上任意点$(x,y,z)$到三元组各阵元的距离记为$R_i$
则$$R_i=\sqrt{(x-R_0\cos\theta_i\cos\phi_i)^2+(y-R_0\cos\theta_i\sin\phi_i)^2+(z-R_0\sin\theta_i)^2}$$
$$\approx R_0-(x\cos\theta_i\cos\phi_i+y\cos\theta_i\sin\phi_i+z\sin\theta_i)$$
$$+\frac{x^2+y^2+z^2-(x\cos\theta_i\cos\phi_i+y\cos\theta_i\sin\phi_i+z\sin\theta_i)^2}{2R_0}\qquad (13)$$
为便于计算，将$x\cos\theta_i\cos\phi_i+y\cos\theta_i\sin\phi_i+z\sin\theta_i$记为$t_i$，将$x^2+y^2+z^2$记为$R$，则
$$R_i=R_0-t_i+\frac{R-t_i^2}{2R_0}\qquad (14)$$
根据电磁学基本原理，将三个阵元作为点源考虑并忽略互耦的影响，第$i$个阵元在天线上$(x,y,z)$处的场可写为
$$\vec{F_i}=A\frac{e^{-jkR_i}}{4\pi R_i}(\vec{u}\vec{E_i}\vec{n})\approx A\vec{E_i}\frac{e^{-jkR_0}}{4\pi R_0}e^{jk(t_i\frac{R-t_i^2}{-2R_0})}\qquad (15)$$
其中，$A$是常数，$\vec{E_i}$为源的激励场==注意此处原文中使用$\vec{E}$，但是根据文字描述以及公式来看应为$\vec{E_i}$==，$\vec{u_i}$是源点到场点的单位矢量$\vec{n}$是喇叭口面的法向单位矢量
导引头天线上$(x,y,z)$处接收到的场为
$$\vec{F(x,y,z)}=\vec{F_1}f_1Vs+\vec{F_2}f_2Vs+\vec{F_3}f_3Vs\qquad (16)$$
其中，$f_i=\sqrt{1-\vec{u_i}\cdot \vec{n}\times \vec{v}}$是导引头天线上$(x,y,z)$处对第$i$个阵元的阵元因子，$Vs$为阵元加权因子

其中，$\vec{v}$是导引头天线阵元在$(x,y,z)$处激励电场的单位矢量
==当导引头天线口径不是太大，$R_0$不是太小，所有$f_i$与中心单元阵元因子$f_{0i}$近似相等==
当天线垂直极化时，$$f_{0i}=\sqrt{1-\cos^2\theta_i\sin^2(\phi-\phi_i)}\qquad (17)$$
==此处注意原文中说将式(9)中$\vec{E}$换成点源幅度值$E_i$，应为笔误，实质是将式(15)中$\vec{E_i}$换成相应幅度值$E_i$==
将式(15)和式(17)同时代入式(16)化简可得$(x,y,z)$处场强
$$F(x,y,z)=E_1ph_1+E_2ph_2+E_3ph_3\qquad (18)$$
其中，$ph_i=Vsf_{0i}e^{jk(t_i\frac{R-t_i^2}{-2R_0})}$
导引头天线阵面上某一象限的合成场强为
$$F_k(\phi,\theta)=\int\int_{S_K}f(x,z)F(x,y,z)dxdz\qquad (19)$$
其中，$S_K$表示天线口径面在第$K$象限的区域
导引头天线的和方向图、归一化方位差方向图和归一化俯仰差方向图可通过如下公式计算
$$SUM(\phi,\theta)=F_1(\phi,\theta)+F_2(\phi,\theta)+F_3(\phi,\theta)+F_4(\phi,\theta)\qquad (20)$$
$$DAZ(\phi,\theta)=\frac{F_1(\phi,\theta)+F_4(\phi,\theta)-F_2(\phi,\theta)-F_3(\phi,\theta)}{SUM(\phi,\theta)}\qquad (21)$$
$$DEL(\phi,\theta)=\frac{F_1(\phi,\theta)+F_2(\phi,\theta)-F_3(\phi,\theta)-F_4(\phi,\theta)}{SUM(\phi,\theta)}\qquad (22)$$
令$DAZ(\phi,\theta)=0$，$DEL(\phi,\theta)=0$，用$|E_i|$代替$E_i$，再加上$E_i$的约束条件($E_1+E_2+E_3=1$)可推得
$$\begin{cases}
\lvert DAZ_1\lvert E_1\rvert +DAZ_2\lvert E_2\rvert +DAZ_3\lvert E_3\rvert \rvert=0\\
\lvert DEL_1\lvert E_1\rvert +DEL_2\lvert E_2\rvert +DEL_3\lvert E_3\rvert \rvert=0\\
\lvert \lvert E_1 \rvert + \lvert E2 \rvert + \lvert E_3 \rvert -1 \rvert=0
\end{cases}\qquad (23)$$

其中
$$DAZ_i=\frac{\int\int_{S_1}ph_i
+\int\int_{S_4}ph_i-\int\int_{S_2}ph_i-\int\int_{S_3}ph_i}{\lvert SUM(\phi,\theta)\rvert}\qquad (24)$$
$$DEL_i=\frac{\int\int_{S_1}ph_i+\int\int_{S_2}ph_i-\int\int_{S_3}ph_i-\int\int_{S_4}ph_i}{\lvert SUM(\phi,\theta)\rvert}\qquad (25)$$
给定$(\phi,\theta)$时，可以解出$(E_1,E_2,E_3)$，再代入式(11)求出$(\phi^{'},\theta^{'})$
可得到修正量$\Delta \phi=\phi^{'}-\phi$和$\Delta \theta=\theta^{'}-\theta$