# The A-Buffer, an Antialiased Hidden Surface Method

A-Buffer是一种基于隐式表面（hidden surface）的抗锯齿（anti-aliased）算法，比Z-Buffer效果更好，但消耗更大

### 隐式表面

在计算机图形学中，隐式表面有很广泛的应用。隐式表面是指，不使用参数来明确描述曲面上一个点，而是使用一个隐函数来表示（比如SDF）

这是一个位于原点的半径为r的球
$$
f(x,y,z,r)=||\mathbf{p}||-r=0
$$
隐式表面常用于射线相交测试、3D建模（物体间可以AND、OR操作，也支持混合变形）

隐式表面的法线就是梯度（梯度就是函数值变化最快的方向，与法线的定义一致）
$$
\nabla f(x,y,z)=(\frac{\partial f}{\partial x}+\frac{\partial f}{\partial y}+\frac{\partial f}{\partial z})
$$

### A-Buffer

A-Buffer有两种数据结构：pixel-structs和fragments



