# 2DVTI_INVERSION
主要用到的外部库是seismic unix中的su、par、cwp库


sub中包含的文件是做运动学射线追踪和动力学射线追踪的程序

common中的包含的文件iofile是文件读入输出的程序和wx是所有程序中有可能用到的简单函数

src是整个反演程序中所用到的关键的函数模块化的文件，以下介绍比较重要的几个程序作用
bsplines.c主要是将模型β样条化涉及的程序，将模型β样条不仅可以减少所要反演的参数，还对立体层析用到的偏导数求导起到关键作用
frechet.c是用来求kernel的函数，用散射积分法计算最速下降方向的函数以及用共轭梯度法求牛顿方向的函数
regu.c是对模型正则化，使得反演的结果更加稳定
forward_modeling.c是用来实现射线正演求计算结果的函数
stereo_subroutines.c是对输入数据进行筛选和得到初始模型的函数
update_model.c是更新模型的函数使得目标泛函下降的函数。

整个反演流程是
1、在参数卡中输入观测系统的信息，输入数据的文件名，以及反演中所用到的人为定义的参数。
2、给输出结果申请空间并初始化β样条的基函数。
3、初始化模型空间中速度和各向异性参数的部分。
4、在读入数据前根据初始的速度和各向异性参数对数据进行筛选。
5、读入筛选后的数据并根据初始速度和各向异性参数来初始化反射点位置和出射角度。
6、随后进入反演迭代步骤，每轮先射线正演计算梯度以及Hessian，加入正则化后利用最速下降梯度和Hessian求得牛顿方向并更新模型。
7、输出每轮迭代的结果。
