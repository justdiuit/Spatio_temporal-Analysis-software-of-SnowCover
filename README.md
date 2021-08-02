# Spatio_temporal-Analysis-software-of-SnowCover
The system processes and analyzes snow parameters of remote sensing images, including data extraction and pretreatment, annual average snow coverage calculation, snow cover area, variation trend of snow parameters, snow cover days, test of first snow day and snow melt day, and section statistics.

 
 
目录
第1章	软件概况	3
1.1	运行硬件环境	3
1.2	运行软件环境	3
1.3	编程语言及其版本	3
1.4	源程序行数	3
1.5	系统功能概述	3
第2章	界面概述	4
2.1	界面总体布局	4
2.2	软件子界面	4
第3章	工具箱	6
3.1	数据读取	6
3.2	文件夹生成	7
3.3	工具	7
3.3.1	掩膜文件生成	7
3.3.2	应用掩膜文件批处理	8
3.3.3	重采样操作	8
3.3.4	生成经纬度格网	8
3.3.5	格式转换	8
第4章	时序分析	9
4.1	平均值	9
4.2	趋势分析	9
4.3	积雪覆盖日数	10
4.4	初雪日	11
4.5	融雪日	11
第5章	统计	12
5.1	分区段统计	12
5.2	积雪覆盖面积	12
5.3	积雪覆盖度	13


第1章	软件概况
1.1	运行硬件环境
台式机、笔记本均可。内存大于1G，硬盘空间大于50G。
1.2	运行软件环境
操作系统：Windows XP/7/10；
应用软件：MATLAB R2020a及以上版本

1.3	编程语言及其版本
编程语言：MATLAB
编译环境：MATLAB R2020b
1.4	源程序行数
代码行数：3530
1.5	系统功能概述
积雪时空分布的研究对于国民生产、生活影响重大。对积雪分布情况的监测，在预防融雪洪水灾害、保障农牧业生产以及合理利用季节性积雪水资源等领域具有重要意义。
本系统针对遥感影像积雪参数进行处理与分析，包括数据提取与预处理，年均积雪覆盖度运算、积雪覆盖面积、积雪参数变化趋势、积雪覆盖日数、初雪日与融雪日检验、分区段统计等功能。



第2章	界面概述
2.1	界面总体布局
软件整体界面分为左侧和右侧，左侧包括影像展示、文件输入路径、文件输出路径、状态栏。右侧包括3个模块：“工具箱”、“时序分析”、“统计”。
右侧模块的输入、输出路径及结果示意图展示，均默认选取左侧工具栏预设路径。
如下图所示：
 
图2.1软件操作界面
2.2	软件子界面
如下图所示，软件包含三个子界面：
	工具箱
	文件读取与预处理
	文件夹生成
	工具：掩膜制作与应用、重采样批处理与空间参考、经纬度格网生成、格式转换
	时序分析
	平均值（用于积雪覆盖度、雪水当量等的空间分布）
	趋势分析：Slope趋势分析、MK趋势检验
	积雪覆盖日数
	初雪日、融雪日
	统计
	分区段统计
	积雪覆盖面积、积雪覆盖度
	结果显示与变化下的趋势线拟合
    
 
图2.2	子界面‘工具箱’‘时序分析’‘统计’

第3章	工具箱
3.1	数据读取
数据读取目的：规范影像命名规则与存储方式，为后续规范操作的基础。经过该操作，影像的命名为yyyymmdd.mat格式，通过其命名中的时间信息，进一步进行后续操作。
此处输入文件夹深度为1级，即在左侧的输入路径中，路径内文件夹下无2级子文件夹。本操作输出结果也为1级文件夹文件。
 
图3.1 数据读取部分操作页面
数据读取文件格式：支持H5\HDF\TIF\mat文件读取。
其中对于H5和HDF格式的影像读取，需要在数据名中明确数据集的名称。比如需要读取某h5文件的SWE[mm]数据集（如图3.2所示），则在数据名中填入/SWE[mm]。对于TIF和mat文件无需此项操作。
 
图3.2 文件读取示例
本系统需要明确该时间序列影像文件名的命名规则中，指示时间的字段。如文件名为F13_SSMI_SWE_20080124_DAILY_025KM_V1.2.h5，可以发现其中指示时间的字段为20080124，在该文件名字符串中，使用切割符号‘_’进行切割后，该指示时间的字段在切割后的第4位，所以切割符号后的框内填入‘_’，日期字段位置填入4。
如果该日期的命名为日积年格式，即比如2020032为日积年，需要将其转换为yyyymmdd格式，即20200201。如果有日积年转换需求，则选中日积年转换前的框。
对于读取的影像数值，设置有效值范围，在有效值范围之外的区域均设为nan，如图3.3所示。
 
图3.3 文件读取示例
3.2	文件夹生成
在时序分析中，常需要对影像的年尺度、月尺度、季节尺度进行分析。此处的工具即按照时序分析需求，将各影像划分入相应文件夹，方便后续时序操作处理，如下图3.4所示。
水文年：输入日尺度文件（1级文件夹），输出以年份命名的年尺度文件夹（2级文件夹），此处输出的为水文年，定义9月1日为该年第一天。
月尺度【年-月】：输入日尺度文件（1级文件夹），输出年-月-日文件夹（3级文件夹）。
月尺度【月-年】：输入日尺度文件（1级文件夹），输出年-日-月文件夹（3级文件夹）。
季节尺度：输入月尺度【年-月】文件夹，输出季节尺度文件夹：四季-年-日（3级文件夹）。
 
图3.4 文件夹生成读取示例
3.3	工具
3.3.1	掩膜文件生成
本操作并未对文件夹进行操作，仅生成单独文件。
输入：掩膜文件路径（.shp文件），经度格网和纬度格网（两类输入格网需大小一致）。
输出：在左侧栏的输出路径下输出该掩膜文件，该掩膜文件是由1和nan值生成的.mat文件。
左侧栏的绘图框会出现该掩膜文件的示意图：
 
图3.5 掩膜工具生成示例
注：该操作的核心是inpolygon函数，只能在提供的经纬度格网下实现掩膜文件的生成。如果需要将区域缩放至裁剪区域，需要使用maskByShape函数，此时影像的空间参考也会发生变化。

3.3.2	应用掩膜文件批处理
输入掩膜文件（.mat文件，数值仅由0和nan构成），对文件夹（1级文件夹，在左侧‘输入路径’中设置该文件夹的输入路径）内文件均使用该文件（注意，掩膜文件与文件夹内文件大小需一致）。

3.3.3	重采样操作
点击“重采样”按钮，对输入文件夹内的文件（1级文件夹，在左侧‘输入路径’中设置该文件夹的输入路径），均进行重采样操作。
重采样级数范围为0-1，例：原文件为200*300大小的影像，经过0.1级重采样后，生成20*30影像。重采样方法为均值滤波。
【重采样方法有：均值（mean）、中位数（median）、众数（mode）。本软件默认为均值方法，如需设置其他重采样方法，修改代码upscaleGrid函数最后一个参数即可】
点击“空间参考”按钮，则会生成重采样后的空间参考，重采样后影像的空间参考会发生变化，此处需要在“空间参考R路径”下，输入未重采样前的空间参考（.mat文件）（对于TIFF影像，可使用geotiffread函数来获取原始空间参考），在输入路径输入未重采样前的一景影像的.mat文件（任意影像即可，只是用于获取未重采样时的影像大小）

3.3.4	生成经纬度格网
此处为常用的经纬度格网生成工具，获取GeoTiff影像的空间信息。
在‘文件路径’文本框中，输入该TIFF影像的路径，即可在输出路径下的文件夹内，获取该TIFF影像的经度格网和纬度格网。

3.3.5	格式转换
本功能适用于mat格式转tif格式，针对“输入路径”文件夹内mat文件进行处理，有两种转换方式：
（1）	使用空间参考进行格式转换，该空间想参考格式为mat格式
（2）	指定空间范围，即确定该mat的经纬度，则输出该经纬度信息的TIFF影像
注意，本功能导出的默认投影为等经纬度投影.


第4章	时序分析
4.1	平均值
输入：左侧输入路径文件夹内文件，可调节文件夹深度，分为1级文件夹、2级文件夹、3级文件夹。
以三级文件夹为例，如某文件夹路径为年-月-日，则运行该功能，导出结果为某年某月的均值（即月尺度）。
 
图4.1 平均值工具使用示例
提供两种均值求解方式，Mean_M1是针对像元出现次数进行平均，常用于初雪日、融雪日等参数的均值求解方法，Mean_M2针对影像数进行平均，对于当前文件夹内每个像元除以相同的数，常用于平均积雪覆盖度、平均积雪日数的运算。
4.2	趋势分析
本文提供两种趋势分析方法：
	Slope：采用线性法，逐栅格对积雪日数/覆盖度和年份进行回归分析，来表征积雪日数/覆盖度的空间变化趋势，线性趋势斜率用最小二乘法来计算。
本系统可以设置其置信度、后缀名和文件夹深度（1、2级文件夹）。其中不满足置信度的部分会被掩膜。
如下图4.2所示，其空间分布显示于左上角，数值分布显示于右下角。
 
图4.2 趋势分析slope工具使用示例
	MK非参数检验：对多种不同概率分布时间序列进行分析。
本系统可以设置后缀名和文件夹深度（1、2级文件夹）。结果有4类，值为10表示显著减小，20表示不显著减少，30表示不显著增加，40表示显著增加。
 
图4.3 趋势分析MK工具使用示例
4.3	积雪覆盖日数
积雪覆盖日数（SCD）的统计，以水文年年（9月1日-翌年8月31日）进行逐像元的统计。原数据的值为正值，则认为当日有积雪，统计日数加1。
本功能可以设置文件夹深度（1级文件夹、2级文件夹）。
如下图4.4所示，其空间分布显示于左上角，数值分布显示于右下角。
 
图4.4 积雪覆盖日数工具使用示例
4.4	初雪日
FSD：积雪覆盖开始日期，将FSD定义为首次连续n日均被积雪覆盖的第一个日期，本功能可以在滑动窗口设置n值（取值范围：2-14，建议取5）。
本功能支持的输入文件夹为年文件夹。
点击FSD按钮，结果如下图4.5所示，其空间分布显示于左上角，数值分布显示于右下角。
 
图4.5 初雪日工具使用示例
4.5	融雪日
ESD：积雪覆盖结束日期，定义为最后一次连续n日均被积雪覆盖的最后一个日期，本功能可以在滑动窗口设置n值（取值范围：2-14，建议取5）。
本功能支持的输入文件夹为年文件夹。
点击ESD按钮，结果如下图4.6所示，其空间分布显示于左上角，数值分布显示于右下角。
 
图4.6 融雪日工具使用示例
第5章	统计
5.1	分区段统计
本操作只针对单个mat文件积雪操作，不是针对文件夹进行操作。
输入：在左侧“输入路径”输入.mat文件，该文件为待统计影像。
在“分区段依据影像”路径中，输入分类依据影像。在“分段”文本框中，输入分段区间。
例：使用DEM影像，对SWE影像进行分区段统计，要求对于[2300，6300]高程，每250m统计SWE的均值和像元数量。
则在“输入路径”输入SWE影像路径，在“分区段依据影像”路径中，输入DEM影像路径。在“分段”文本框中，输入2300,500,6300，结果如下图5.1所示。
 
图5.1 分段统计工具使用示例
如图5.1所示，在右侧中部的表格中，显示各区段的均值和像元个数。表头为分度依据，第一行为各区段划分下，像元均值；第二行为各区段划分下，像元个数；
在右侧下部的图中，显示各区段均值和像元个数。其中各区段均值用-o形式，像元个数用bar形式，各区段均值的拟合趋势用红色虚线形式，该拟合线斜率值在右下角。
5.2	积雪覆盖面积
积雪覆盖面积（SCA）的统计，为逐像元的统计。原数据的值为正值，则认为当日有积雪，统计像元数加1；原数据的值为 0值或者数据缺失，则认为当日没有积雪，统计像元数不变。
本功能仅为统计像元数，实际积雪覆盖面积为像元数*像元面积，需要用户自行根据实际情况得到实际积雪覆盖面积。
输入：年-月格式文件夹(文件夹深度为2)。该文件夹内所有文件为yyyymmdd.mat格式，结果如下图5.2所示。
 
图5.2 积雪覆盖日数工具使用示例
如图5.2所示，在右侧中部的表格中，显示各年月的积雪覆盖面积值。表头为月份，x轴表示月份（1-12月），y轴表示年份；
在右侧下部的图中，显示每年年均积雪覆盖面积。使用Plot绘制每年积雪面积，用红色虚线显示其变化趋势，该拟合线斜率值显示于右下角。
可以设定开始时间和结束时间，即某一段时间的斜率，比如计算2000-2020年积雪面积变化趋势，拉动“开始时间”和“结束时间”进度条，可以获取任意时间段内的积雪面积变化情况及其实时拟合线斜率。
5.3	积雪覆盖度
积雪覆盖度(FSC)的统计，为逐像元的统计。计算影像的FSC均值，即将所有FSC值相加后除以像元个数，此处可以在“研究区像元数”中设定实际像元个数，若不设置，则默认像元数为整景影像全部像元数。
输入：年-月格式文件夹(文件夹深度为2)。该文件夹内所有文件为yyyymmdd.mat格式，结果如下图5.3所示。
 
图5.3 积雪覆盖度工具使用示例
如图5.3所示，在右侧中部的表格中，显示各年月的积雪覆盖度值。表头为月份，x轴表示月份（1-12月），y轴表示年份；
在右侧下部的图中，显示每年年均积雪覆盖度。使用Plot绘制每年积雪度，用红色虚线显示其变化趋势，该拟合线斜率值显示于右下角。
可以设定开始时间和结束时间，即某一段时间的斜率，比如计算2000-2020年积雪覆盖度变化趋势，拉动“开始时间”和“结束时间”进度条，可以获取任意时间段内的积雪覆盖度变化情况及其实时拟合线斜率。

