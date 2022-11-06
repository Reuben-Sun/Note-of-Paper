# Illumination-driven Light Probe Placement

为了提升Light Probe的效果，我们会要求美术place probe manually

而这篇问题提出了一种自动排布Light Probe的方案：

- 先根据用户策略，在场景中摆放过量的Light Probe（LP）
- 烘焙LPs，并将其存储在一个四面体图中
- 在场景中摆放评估点Evaluation point（EP）
- 计算每个EP在几个方向下的Illumination，可以按照用户策略求均值或者单独存储
- 存储EP数据
- 遍历LPs
  - 删除当前这个LP，于是新的图中有n-1个LPs
  - 


作者在开放了源码，[Github](https://github.com/cgaueb/light_probe_placement)



















