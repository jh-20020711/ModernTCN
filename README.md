数据集：https://pan.baidu.com/share/init?surl=r3KhGd0Q9PJIUZdfEYoymg&pwd=i9iy
在long-term-forecasting中的ModernTCN.py有中文注释
问题：无法对时间维度进行卷积
原因：代码运行过程ModernTCN初始化 → 下采样数据集→Stage列表创建 → 每个Stage初始化 → 每个Stage内的多个Block初始化
要对时间维度做卷积，首先就要知道时间维度N，而时间维度N的获取方法是通过下采样数据集，对数据做滑动窗口处理。在这一步中作者采用了stage方法（用于分阶段对数据不同大小的滑动窗口处理，以及分阶段对卷积核大小做计算）。因此在获取N之前要先创建stage，而创建stage就需要初始化Block（即卷积层），需要定义卷积层的输入输出通道，而此时还没有获取N，因此无法将卷积层的输入通道设置为N（因此无法对时间维度做卷积）。
不同stage的时间维度N是不一样的，只有特征D和变量M的维度是先验信息，因此只能对特征和变量做卷积。
