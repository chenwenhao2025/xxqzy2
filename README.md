# 第二次作业：探索数据可视化
要求：
有好几个库都可以进行数据可视化。用 matplotlib 和 seaborn 对本课中涉及的 Pumpkin 数据集创建一
些数据可视化的图标。

# 1 数据基本信息

|列名|说明|缺失值数量|
|---|---|---|
|City Name|城市名称|0|
|Type|类型|1712|
|Package|包装|0|
|Variety|品种|5|
|Sub Variety|子品种|1461|
|Grade|等级|1757|
|Date|日期|0|
|Low Price|低价|0|
|High Price|高价|0|
|Mostly Low|大部分低|103|
|Mostly High|大部分高|103|
|Origin|原产地|3|
|Origin District|原产地地区|1626|
|Item Size|物品大小|279|
|Color|颜色|616|
|Environment|环境|1757|
|Unit of Sale|销售单位|1595|
|Quality|质量|1757|
|Condition|状态|1757|
|Appearance|外观|1757|
|Storage|存储|1757|
|Crop|裁剪|1757|
|Repack|重新包装|0|
|Trans Mode|转换模式|1757|


# 2.1 数据准备

使用Pandas读取数据集，查看数据的基本组织信息：
![屏幕截图 2025-07-02 141926](https://github.com/user-attachments/assets/c85af18b-372d-43bf-8d89-078509289af7)

  通过观察发现：数据存在缺失值，数据共有1757行，代表1757个样本，按城市分组，数据的列是特征变
  量。


# 2.2 数据检查

（1）缺失值检查

![屏幕截图 2025-07-02 141944](https://github.com/user-attachments/assets/34d046aa-265e-488a-a07f-1143b7190f02)

  从缺失值看出其中City Name、Package、Variety、Date、Low Price、High Price、Origin、Repack
  列数据基本无缺失，其它列数据缺失较多。

（2）一致性检查
![屏幕截图 2025-07-02 143007](https://github.com/user-attachments/assets/5b72d470-e89b-4497-a896-9a308f43a47a)

  南瓜的包装（称量单位）并不相同，后续可能需要统一单位。

（3）提取本次研究所需要的有价值的特征和标签
![屏幕截图 2025-07-02 143133](https://github.com/user-attachments/assets/2c4bdd6d-c20e-4c1f-935a-2878a494a8d2)

  假设目标是预测南瓜价格，那么价格将是因变量标签。而南瓜的价格可能与以下因素有关：月份、销售
  日期、销售地区、南瓜种类、包装（称量单位）等。

# 2.3 数据整理

（1）过滤缺失值
![屏幕截图 2025-07-02 143329](https://github.com/user-attachments/assets/f7b57d6c-7db1-41fe-b3fe-9d5aa77c0640)

先过滤含有空值的数据

![屏幕截图 2025-07-04 142723](https://github.com/user-attachments/assets/b625bafd-e2e7-4868-b397-f3cb62c464b4)

前面已经验证南瓜的包装方式（称量单位）不统一。经研究原始数据，发现其中包含“英寸”、“磅”和“蒲式
耳”三种称量类型，单位的不同，南瓜似乎很难保持一致的重量，那么它的价格预测也毫无意义。

因此，打算只筛选“蒲式耳”单位（只包含字符串“bushel”）的数据，其它的南瓜数据将过滤它们，虽然缺
失了很多数据，但它们对分析无用。

（2）数据提取与转化

1.月份：可以从Date列中提取
![屏幕截图 2025-07-04 143831](https://github.com/user-attachments/assets/3d481c54-463c-4e52-a511-5250dc8a221c)
![屏幕截图 2025-07-04 143841](https://github.com/user-attachments/assets/44018c19-9eaa-4d81-a0a4-a6e0cde7c5c4)

2.销售日期：可以通过Date列转化得到（统一转化为该年中的第几天）
![屏幕截图 2025-07-04 143939](https://github.com/user-attachments/assets/cd6443bd-f3d0-42b3-bd18-f1b325541b8b)
![屏幕截图 2025-07-04 143952](https://github.com/user-attachments/assets/22a4acce-dc52-4682-804a-9c52f3b27e7f)

3.南瓜价格：可以从已存在的特征（Low Price和High Price）计算平均值得到
![屏幕截图 2025-07-04 144054](https://github.com/user-attachments/assets/2377919f-3b9c-43d6-b59b-a3c1644fb2e8)
![屏幕截图 2025-07-04 144107](https://github.com/user-attachments/assets/e9868b43-7300-40c8-878b-da086c3f2ed8)


4.统一南瓜价格的每单位称量单位
![屏幕截图 2025-07-04 144722](https://github.com/user-attachments/assets/135c399c-8463-4d60-af50-d2aac2dc12dd)



（3）数据整理查看
![屏幕截图 2025-07-04 144847](https://github.com/user-attachments/assets/6f1d611e-0b47-4ad9-bae5-bf447a19a8a7)

最终，本次的特征变量有：Month、DayOfYear、City Name、Variety和Package；目标标签为：Price；
样本数为415。


# 2.4 相关性分析

（1）月份与价格
![image](https://github.com/user-attachments/assets/82737863-8bdc-4944-b6d6-ad57fd8e1f95)

从柱状图中可以直观的看出，南瓜的最高价格可能出现在9月或10月。


（2）城市与价格
![image](https://github.com/user-attachments/assets/f917a550-c9c1-41e0-91ae-f6463a2c15a1)

可以看到，大部分城市南瓜的价格处于中等水平，少部分城市南瓜的价格相差较大。


（3）品种与价格
![image](https://github.com/user-attachments/assets/912b473d-5e2e-4897-9fb2-f7efb7cd9e9f)

上图可知，不同品种的南瓜之间的价格还是存在不小差异。


（4）包装与价格
![image](https://github.com/user-attachments/assets/47d1bf50-f216-43a0-b7e9-a963bd6f9444)

不同大小与包装的南瓜价格相差较大，其中最高价格是最低价格的三倍。


（5）销售日期与价格
![image](https://github.com/user-attachments/assets/23ea8dcc-4640-4bf1-8c69-d745aec79a43)

这看起来似乎不同的价格群对应于不同的南瓜品种。为了证实这一点，使用不同的颜色标记不同的南瓜种
类。


![image](https://github.com/user-attachments/assets/8f271097-b154-4525-98b3-4603f5d80586)

根据上图，可以看到南瓜种类对南瓜价格的影响比销售日期更大。MINIATURE种类的南瓜价格整体较贵，
而PIE TYPE种类的南瓜价格整体较便宜。



# 3 简单线性回归实践
# 3.1 销售日期-南瓜价格
为了得到效果满意的线性回归模型，将使用Scikit-Learn库分别尝试从单一变量的简单线性回归到多项式
回归再到多元线性回归依次训练模型。

所谓简单线性回归，就是单变量的线性回归，如销售日期与南瓜价格的线性关系或南瓜种类与南瓜价格的
线性关系，简单线性回归的回归线是一条直线，它可以单方面的探索特征间的相关性。

例如，按月预测每体积单位的南瓜价格：将月份作为自变量X，将南瓜价格作为因变量Y，根据拟合的直
线，便可根据月份预测出该月的南瓜价格。

![屏幕截图 2025-07-04 152700](https://github.com/user-attachments/assets/fa1b942a-d6ec-4f37-a53d-cb71105dfb9d)

![屏幕截图 2025-07-04 152708](https://github.com/user-attachments/assets/0daa40b6-8bae-43c3-9c51-47aa008b071a)

拟合结果如下图：
![屏幕截图 2025-07-04 152715](https://github.com/user-attachments/assets/8f2fa837-fd69-42d2-bc50-6891a380fbe2)

从结果可以看出，均方根误差在10.66左右，相关系数约为0.03，这是相当低的。如果相关系数为0，则表
示模型不考虑输入数据，预测的效果最差。相反1意味着可以完美地预测所有预期的输出。

相关系数是表示样本值与预测值之间相关程度的系数，其取值区间为[0，1]，越靠近1相关性越高；1表示
完全相关，0为完全不相关；可以使用散点图来可视化此系数。聚集在回归线周围的数据点具有高度相关
性，分散在回归线周围的数据点具有低相关性。


# 3.2 南瓜种类-南瓜价格
接下来，使用不同的南瓜类型来检验南瓜类型与价格的相关性。希望能够使用相同的模型预测不同南瓜品
种的价格。但是，南瓜类型列与其他特征略有不同，因为它的值是字符串而非数值。

为了方便训练，首先需要将其转换为数值形式，对其进行编码。

pd.get_dummies()函数可以将原有列用4个（种类数）不同的列替换，每个列对应一个品种。每列将包含
相应种类的值，这意味着线性回归中将有四个系数，每个南瓜品种一个，负责该特定品种的起始价格。

![屏幕截图 2025-07-04 153135](https://github.com/user-attachments/assets/b4ab9548-497f-4a55-b39c-4836f9b73419)

![屏幕截图 2025-07-04 153143](https://github.com/user-attachments/assets/d5d8efa8-818f-4d12-a74f-9f6b005d05d9)

可以看到，均方根误差RMSE数值减半，数据间的相关性提升到了0.76，这与猜想的一样，南瓜价格与南瓜
品种的相关性高于销售日期。

再看一下销售日期和价格之间的关系。虽然变量之间有时存在线性关系，但并不是所有的关系都是呈线性
的，在这种情况下一元线性回归拟合出来的回归线欠拟合，不能很好的预测价格。因此可以采用多项式回
归。


# 4 多项式回归实践
多项式中的每个单项式叫做多项式的项，这些单项式中的最高项次数就是这个多项式的次数。单项式由单
个或多个变量和系数相乘组成，也有常数项；多项式回归模型创建的曲线可以更好地拟合非线性数据，但
需要注意避免过拟合。

多项式回归算法并没有新的特点，完全是使用线性回归的思路，关键在于为数据添加新的特征，而这些新
的特征是原有的特征的多项式组合，采用这样的方式就能解决非线性问题。多项式回归通过特征升维，可
以更好地拟合高维数据。

# 4.1 销售日期-南瓜价格
使用构建流程化工具Pipeline对南瓜价格关于销售日期（月份）的2阶多项式进行拟合
![屏幕截图 2025-07-04 154038](https://github.com/user-attachments/assets/03f9d58a-6c18-4c08-8e04-74984066ffd3)

![屏幕截图 2025-07-04 154051](https://github.com/user-attachments/assets/0e3ac72c-9f33-4308-972e-33f3820607d0)

拟合结果如下图：
![image](https://github.com/user-attachments/assets/754e0115-9a7a-4f41-881d-f68841c98ced)

使用多项式回归，获得的RMSE和相关系数仍然很低，效果不明显，需要考虑其他特征。


# 4.2 南瓜种类-南瓜价格

![屏幕截图 2025-07-04 154139](https://github.com/user-attachments/assets/18f66238-aaee-4958-900d-e7443bdfc5a6)


![屏幕截图 2025-07-04 154146](https://github.com/user-attachments/assets/5bd47b6d-ab56-4cea-860a-04a39695d486)

从模型评估结果可以看出，销售日期和南瓜种类与南瓜价格的多项式回归效果都不明显。那么，南瓜价格
会不会与多个因素共同相关

# 5 多元线性回归实践
多元，即特征向量不唯一、多个特征，多项式是基于输入特征生成特征，而多元既可以是输入特征生成的
特征，也可以只是输入的特征。

随着特征变量的增多，需要计算的ω（回归系数或斜率）也增加。这与简单线性回归的区别不大，只是拟合
出来的回归线是曲线。基于多种特征变量拟合出来的特征曲线（机器学习模型）更加贴合实际。

为了获得更准确的预测，需要考虑更多的分类特征。下面，考虑销售日期（月份）、销售地区、南瓜种类
等多个因素。

# 5.1 使用南瓜种类、销售日期、销售地区、称量单位四个一阶特征拟合南瓜价格

![屏幕截图 2025-07-04 154308](https://github.com/user-attachments/assets/28b6daec-9740-4c0a-b36e-bdb0d0eeb50a)

效果很好，均方根误差RMSE明显降低，相关性已经达到了0.93。

# 5.2 使用南瓜种类、月份、销售地区、称量单位四个高阶特征拟合南瓜价格

![屏幕截图 2025-07-04 154325](https://github.com/user-attachments/assets/a5511472-d4e1-49af-a6ca-f77be3503d29)

模型评估指标显示，得到了高达0.96相关性的最佳相关系数，RMSE仅为2.16。在本次案例中共创建了六个
回归模型，并将模型质量提高到了96%。
