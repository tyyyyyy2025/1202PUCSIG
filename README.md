# GISCUP2021

## 数据介绍

### train数据集

#### head部分

* order id
* ata(actual time of attival)=订单结束时间-乘客上车时间，单位为秒
* distance
* simple eta(estimated time of arrival)**出发时刻**平均通行时间加和，最后的结果至少应该比它好
* driver id
* slice id出发时刻时间片id(每5分钟一个，24h循环)

#### link部分

link的选取并没有很直接的意义

* link id
* link time**出发时刻**道路小段前10分钟的平均通行时间，基于历史统计，本身不具有预测作用
* link ratio实际轨迹覆盖该小段的占比，除了首尾一定都是1，所以应该只有在处理首尾时间时有意义
* link current status发单时刻该道路小段的路况状态。路况状态分为拥堵（3），缓行（2），畅通（1），未知（0）
* link arrival status到达该小段时的路况状态，这个特征在test集中不存在，那么怎样去预估/学习这个状态，靠路网的领域拓扑关系？

#### cross部分

* cross id不是一个数，link id1_link id2组成，且两个小段可能不相邻
* cross time挖掘得到，粒度粗

### nextlink.txt（路网拓扑数据）

格式：link id next link id1, next link id2, ...

翻译：一个道路小段和其下辖的道路小段，一个id既可能出现在前半部分，也可能出现在后半部分-->**有向**

### weather.csv（天气数据）

包含日期、天气信息、最高最低温度



## 解题思路

* 1.大方向：回归问题，数据有序列性-->LSTM建模

    https://blog.csdn.net/yangwohenmai1/article/details/84873763

* 2.考虑邻域的影响，需要用到路网拓扑推知路况变化的传递（空间和时间）
* 3.**模型的上限是特征**，思考可以进一步挖掘那些特征统计值
