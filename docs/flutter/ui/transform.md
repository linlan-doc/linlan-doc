---
title: flutter中的变换和Matrix4
sidebar_position:  11
toc_max_heading_level: 4

keywords: ['flutter语言教程','flutter变换','flutter Transform','flutter Rotate']
---

<head>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.13.24/dist/katex.min.css" integrity="sha384-odtC+0UGzzFL/6PNoE8rX/SPcQDXBJ+uRepguP4QkPCm2LBxH3FA3y+fKSiJ+AmM" crossorigin="anonymous" />
</head>

import Image from '@theme/IdealImage';

> 本文是Flutter动画系列的第九篇，建议读者阅读前面的教程，做到无缝衔接。

 前面的教程里已经使用了平移和旋转变换，本文将系统的介绍 _flutter_ 里的变换和 _Matrix4_ 。

### 1. 变换

 在图形学里，变换指通过某种规则将一个图形变为另外一个图形。_flutter_ 变换的类名为[Transform](https://api.flutter.dev/flutter/widgets/Transform-class.html)，变换主要有平移变换（_translation_）、旋转变换（_rotation_）、缩放变换（_scaling_）和剪切变换（_shear_）。除了剪切变换，其他三种变换 _flutter_ 都提供了对应的构造函数。

    Transform.rotate({Key? key, required double angle, Offset? origin, AlignmentGeometry? alignment = Alignment.center, bool transformHitTests = true, FilterQuality? filterQuality, Widget? child})

    Transform.scale({Key? key, double? scale, double? scaleX, double? scaleY, Offset? origin, AlignmentGeometry? alignment = Alignment.center, bool transformHitTests = true, FilterQuality? filterQuality, Widget? child})

    Transform.translate({Key? key, required Offset offset, bool transformHitTests = true, FilterQuality? filterQuality, Widget? child})

### 2. 为什么使用矩阵来实现变换

 图形学中，一般使用向量（_Vector_）来表示一个点，例如三维空间中的一个点可以表示为$[x,y,z]$，_x_ 轴的平移变换可以表示为$[x+dx,y,z]$，_x_ 轴的缩放变换可以表示为$[k*x,y,z]$。

 使用矩阵实现变换的好处是，多种变换可以融合在一起，同时变换的叠加可以通过矩阵相乘实现。

$$

\begin{bmatrix}
a & b & c\\
d & e & f\\
h & i & j
\end{bmatrix}

\begin{bmatrix}
x \\
y \\
z
\end{bmatrix}
=
\begin{bmatrix}
a*x + b*y + c*z \\
d*x  + e*y + f*z \\
h*x + i*y + j*z
\end{bmatrix}

$$


#### 2.1 为什么使用4*4的矩阵

&emsp;使用3*3无法实现平移变换，所以新增了一列来实现平移变换，这称为齐坐标。沿着 _x_ 轴平移用矩阵表示如下。

$$

\begin{bmatrix}
1 & 0 & 0 & dx\\
0 & 1 & 0 & 0\\
0 & 0 & 1 & 0\\
0 & 0 & 0 & 1
\end{bmatrix}

\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}
=
\begin{bmatrix}
x+dx \\
y \\
z \\
1
\end{bmatrix}

$$


### 3. Matrix4

 _flutter_ 提供[Matrix4](https://api.flutter.dev/flutter/vector_math_64/Matrix4-class.html)类来构造变换矩阵，它的构造函数非常多，常用的构造函数如下，读者可以根据需要选用。

    Matrix4.zero() //全0矩阵
    Matrix4.identity() //对角线全是1，对等变换
    Matrix4.rotationX(double radians) //绕x轴旋转
    Matrix4.rotationY(double radians) //绕y轴旋转
    Matrix4.rotationZ(double radians) //绕z轴旋转
    Matrix4.diagonal3Values(double x, double y, double z)//主对角线元素值，用来缩放

* * *

1.  [Why do we use 4x4 matrices to transform things in 3D?](https://gamedev.stackexchange.com/questions/72044/why-do-we-use-4x4-matrices-to-transform-things-in-3d)

2.  [Transformation matrix](https://en.wikipedia.org/wiki/Transformation_matrix)

[署名-非商业性使用-禁止演绎 4.0 国际](https://creativecommons.org/licenses/by-nc-nd/4.0/deed.zh)
