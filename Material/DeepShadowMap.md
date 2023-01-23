# Deep Shadow Map

传统的Shadow Map在每一个像素上存储一个深度信息

而Deep Shadow Map存储的是该像素在所有可能的位置上的可见性信息

优点：

- DSM来自预计算，于是运行时查找效率更高
- 支持半透明物体、体积体的阴影
- 用于动态模糊（motion blur）时性能更好

