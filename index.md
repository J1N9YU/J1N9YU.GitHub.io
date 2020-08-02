# 更新日志

## 可视化
测试了TEve绘制点、线的功能。尝试了使用模拟的数据，但是由于坐标系不一致，效果不好，这里只展示一个点和一条线。

点和线没有在投影里显示出来，应该是我自己写的bug，最终是可以显示出来的。

![avatar](pic\200802_1.png)

另外，我正在把右下角的投影改成顶视图（修改ROOT源代码）

网页显示方面还没进行尝试。

## 数据格式
重新写了数据处理（轨迹重建）的部分，现在流程更加简洁，只有几行。计算结果的数据类型为numpy矩阵，可以通过ROOT的python模块导入eve中。
```python
from numpy import *
import ROOT

#塑闪数量
nPS = 12
#每个塑闪的SiPM数量
nPM = 2

#SiPM信号（暂且随机生成）
signal = random.random((nPS,nPM))

#PMT位置（应当从几何读入）
position = random.random((nPS,nPM,3))*100

#重建算法
ratio = signal/sum(signal,1,keepdims =True)
hitVertex = sum(position*expand_dims(ratio,2).repeat(3,2),1)

#拟合。center（三维点）和v（的一个列向量）决定了一条直线
center = average(hitVertex,0)
u,s,v=linalg.svd(hitVertex-center)

```

## 

上述代码中的```signal```变量就是我需要的数据输入，目前，它是$12\times 2$的矩阵，矩阵元即电压读数。












