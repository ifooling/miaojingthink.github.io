---
layout: post
title: 对单个Geometry使用四参数转换
date: 2017-05-08 16:23:56
category: "arcgis"
---

### 四参数转换

```c#
/// <summary>  
///  对单个Geometry使用四参数转换。  
/// </summary>  
/// <returns></returns>  
private IGeometry GeometryConvert(IGeometry geometry)  
{  
    FourParameters fourParameters = _parameters as FourParameters;  
    IAffineTransformation2D3GEN affineTransformation = new AffineTransformation2D() as IAffineTransformation2D3GEN;  
    double[] expectedParameters = new double[6];  
    expectedParameters[0] = fourParameters.K;  
    expectedParameters[1] = -fourParameters.R;  
    expectedParameters[2] = fourParameters.Dx;  
    expectedParameters[3] = fourParameters.R;  
    expectedParameters[4] = fourParameters.K;  
    expectedParameters[5] = fourParameters.Dy;  
    affineTransformation.SetLinearCoefficients(esriTransformDirection.esriTransformForward, ref expectedParameters);  
    ITransform2D pTransform2D = geometry as ITransform2D;  
    pTransform2D.Transform(esriTransformDirection.esriTransformForward, affineTransformation as ITransformation);  
    return geometry;  
}  
```
