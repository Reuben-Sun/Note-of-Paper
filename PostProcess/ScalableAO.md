# Scalable Ambient Obscurance

这是SSAO的改进

### 算法

该算法输入depth buffer，输出屏幕像素ao

#### 1. High-Precision z Prepass

我们将使用depth buffer重建世界坐标，因此对z的精度要求很高

作者做了

- 以双精度计算矩阵
- 令$z_f=- \infty$，这减少了浮点运算（why？）
- 左乘矩阵（why？）

> When using column-major matrices (the default in OpenGL), multiply vectors on the left ($\mathbf{v'}$ = $\mathbf{v'P}$) in the vertex shader. This saves about half a bit of precision

#### 2. Hierarchical z Pass

这个pass用于生成Hi-Z



#### 3. Distributed AO Sample Pass

#### 4. Bilateral Reconstruction Passes