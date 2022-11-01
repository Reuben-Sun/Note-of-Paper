# Parallax-corrected Cubemap

### IBL

Distant IBL（如无穷远的Cubemap）在处理无穷远的光照时效果不错，着色平滑

Loacl IBL（如逐物体、逐空间的Cubemap）就出现了很多问题

**若一个Mesh横跨两个区域，会出现渲染错误、交界线**

![seams](../Image/seams.png)

**视差问题：场景物体的反射光不在正确的位置**

这个很好理解，我们用这种办法计算反射光时，并没有用到场景物体的位置，只是单纯用法线、镜头矩阵去采样Cubemap，尤其是镜头移动时，问题会更明显

### 接缝问题

为了解决接缝问题，作者给的方案是**混合**

- 将场景划分为多个区域，不同区域有一个Cubemap
- **根据位置**，将周围多个Cubemap进行混合，生成一个临时Cubemap

![MixCubemap](../Image/MixCubemap.png)

作者将自己这套方案称为Point of interest（POI），在他的方案中，是根据相机/Player所在的位置进行混合的

#### Gather loacl cubemaps

而收集周围Cubemap，作者给出了一个方案：

- 给每一个Cubemap设置一个influence volume（这个volume是互相重叠的）
- 遍历所有Cubemap，若POI在其volume外，则不使用
- 使用八叉树来加速遍历
- 为了性能，限制了参与混合的Cubemap的最大数量

#### 混合算法

如何将多个Cubemap混合成一个Cubemap呢？我们需要先知道**权重**

作者的方案有些依赖美术，美术在制作Cubemap时，需要设定volume的边界

- POI在边界内，只使用这一个Cubemap，即权重为1
- 在边界处，该Cubemap权重为0
- 在不违法前两条的情况下，若大Volume内有小Volume，那么小Volume的权重应该更大

![Blending](../Image/Blending.png)

对每一个mipmap层级都进行混合

```c++
for(int i = 0; i < NumPrimitive; i++){
	if(In inner range) continue;
    if(In outer range){
        list.Add(Primitives[i].GetInfluenceWeights(POI));
    }
}
list.Sort();
for(int i = 0; i < list.Length(); i++){
	SumIW += list[i];
    InvSumIW += (1.0f - list[i]);
}
for(int i = 0; i < list.Length(); i++){
	BlendFactor[i] = (1.0f - (list[i] / SumIW)) / (list.Length()-1);
    BlendFactor[i] *= ((1.0f - list[i]) / InvSumIW);
    SumBlenderFactor += BlendFactor[i];
}
```

### 视差问题

效果如下

![视差](../Image/视差.png)

#### 传统方案

这里是视差矫正的传统方案（不是作者的）

![视差采样](../Image/视差采样.png)

- 根据相机位置和着色点，得到观察光线**V**和反射光线**R**
- **R**会与物体相交于P点
- 用Cubemap所在的C点于P点的向量进行采样

```c++
//AABB包围盒
float3 SampleCubemap(...){
    float3 V = PosWS - CameraWS;
    float3 R = reflect(V, N);
    //求交点P
    float3 FirstIntersect = (BoxMax - PosWS) / R;
    float3 SecondIntersect = (BoxMin - PosWS) / R;
    float3 FurthestPlane = max(FirstIntersect, SecondIntersect);
    float Distance = min(min(FurthestPlane.x, FurthestPlane.y), FurthestPlane.z);
    float3 P = PosWS + R + Distance;
    //采样
    float3 R1 = P - PosC;
    return texCUBE(envMap, R1);
}
```

时至今日，有了更多更便宜的求交算法，配合硬件加速，效率非常高

#### 作者的方案

由于作者的Cubemap就和传统的不一样（指根据POI混合出来），于是要使用不同的视差矫正方案

![作者的视差矫正](../Image/作者的视差矫正.png)

作者的方案放在现在其实也不怎么好了，原理极其简单暴力，就是将相机根据mirror镜像，在新的相机位置绘制一张Texture2D。很明显，这太overdraw了

而且随着RTX显卡和光追的发展，作者的这套方案可能还没“传统方案”性能好













