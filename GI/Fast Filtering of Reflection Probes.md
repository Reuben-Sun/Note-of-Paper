# Fast Filtering of Reflection Probes

在实时渲染中，对Cubemap物理地滤波，性能很差。作者提出了一种双pass各向同性滤波的算法，能大幅提升性能

### SAT

*CROWF.: Summed-area tables for texture mapping. InProceed-ings of ACM SIGGRAPH(1984)*

很多二维图像的滤波（Filter）方法并不能直接拿来滤波Cubemap，原因有：

- 要处理Cubemap的接缝
- Cubemap不径向对称

