# 航班恢复 airline_schedule_recovery

航班恢复问题算法 airline schedule recovery algorithm

## 问题背景

航班恢复问题的背景，可以参考天池大赛厦门航空的赛题。

https://tianchi.aliyun.com/competition/entrance/231609/introduction

也可以参考本地的文档[doc/problem_desc.md](./doc/problem_desc.md)

## 数据集

数据集1: 天池大赛的数据集，链接：
- https://tianchi.aliyun.com/competition/entrance/231609/information
- 本地数据集[data/tianchi](./data/tianchi)

## 算法说明

这里在该赛题的数据集和问题描述的背景下，采用了一种元启发式算法，通过局部搜索为主要策略的算法，实现了航班恢复。
