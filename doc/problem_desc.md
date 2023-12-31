# 厦门航空航班恢复智能规划数据集

此处数据集来自天池大赛公开，链接为 https://tianchi.aliyun.com/dataset/1078

## 描述

该数据集来自智慧航空AI大赛：https://tianchi.aliyun.com/competition/entrance/231609/information

## 数据列表

| 数据名称                      | 上传日期   | 大小        | 下载             |
| ----------------------------- | ---------- | ----------- | ---------------- |
| XMAE_data.xlsx                | 2018-07-13 | 539.32KB    | 请参考上述原链接 |
| XMAEvaluation.jar             | 2018-07-13 | 11.58MB     | 请参考上述原链接 |
| XMAEvaluation_source_code.zip | 2018-07-13 | 2.71GB      | 请参考上述原链接 |
| jar使用说明.txt               | 2018-07-13 | 192.00Bytes | 请参考上述原链接 |

其中的jar使用说明的内容如下：

```bash
使用方式:  
java -jar XMAEvaluation.jar 比赛数据文件路径 结果文件路径
使用例子:  
java -jar XMAEvaluation.jar data/厦航大赛数据20170814.xlsx data/baseline_result_2.csv
```



---



# 厦门航空航班恢复课题

此处复制自天池大赛赛题说明，链接如下：

https://tianchi.aliyun.com/competition/entrance/231609/introduction

## 1. 课题背景

- 目前厦门航空有限公司主要有七大运行基地：厦门、福州、杭州、泉州、北京、天津、长沙。夏秋季东南沿海的台风，冬春季北方的雾霾以及华南沿海的平流雾，飞机故障等，都有可能导致相关机场出现大面积航班延误的情况。为了使航班的计划合理并有序运行，签派员需要对航班进行调整。

- 厦航目前使用辅助人工决策的航班调整系统处理航班计划，但在台风等极端天气对多基地运行造成影响的情况下，仍需要6小时左右才能将航班计划梳理完毕，随着公司运力的逐渐递增以及运行环境的日益复杂化，后续航班大面积延误情况下，需要更长的时间梳理航班计划，这样不能满足对运行的快速响应，为了缩短航班计划恢复时间，提出了航班自动恢复系统的建设需求。该课题希望实现航班自动恢复系统的核心算法。当遇到恶劣天气或飞机故障时，该算法能够在满足各种实际约束条件的前提下，自动对航班进行快速调整，快速恢复航班计划并使公司运营成本达到最低。

## 2. 问题描述

- 已知厦门航空公司未来几天的全部航班计划，以及航班运行的各种约束条件，假设当前由于恶劣天气或飞机故障，导致某些航班不能正常运行，需要对相关航班进行调整（参考“3. 调整方法”），使得受影响的航班旅客尽快疏散，未受影响的航班运行受影响最小，而且恢复的综合成本（通过目标函数值来模拟整成本，参考“6. 目标函数”）尽可能低。

## 3. 调整方法

航班调整的方法有：

- 3.1  取消航班。
- 3.2  换飞机：改变航班绑定的飞机。可以换成同一机型的其他飞机，或者换成不同机型的飞机（目标函数中加惩罚，参考“6. 目标函数”）。
- 3.3  调整时间：修改航班起飞时间（提前或延后）。航班时刻提前与延误规则详见“8. 航班调整规则”
- 3.4  联程拉直：参考“5.5. 联程航班”和“表6 航班表”的说明。联程航班拉直规则详见“8. 航班调整规则”
- 3.5  调机：某飞机不搭乘旅客空飞到其他机场。调机规则详见“8. 航班调整规则”。
- 3.6  旅客签转：将取消航班的旅客、联程航班拉直损失的旅客、换飞机或机型变化损失的旅客、中转失败的旅客、超售的旅客，签转到后面航班空余的座位中，旅客签转规则详见“8. 航班调整规则”。

注意：换飞机、机型变化、联程拉直和调机时，都可以同时调整时间。

## 4. 结题要求

实现航班恢复算法（编程语言不限，建议使用Java 、C#、C++、或Python），该算法根据输入的数据（见“7. 数据说明”），采用“3. 调整方法”中描述的方法进行调整，生成新的航班计划，该计划满足下面的评测标准：

- 4.1.  计算正确无误，满足约束条件（参考“5. 约束条件”）。
- 4.2.  目标函数值小（参考“6. 目标函数”）。
- 4.3.  计算速度快，在提交结果的技术文档中（见“9. 提交结果”），需要写明机器配置和平均计算完成时间（配置描述如：普通PC服务器，2颗10核CPU，30G内存）。注：计算时间只作参考，主要看目标函数值。

## 5. 约束条件

表1 约束条件  

| **约束条件**  | **性质** | **参考表格** |
| ------------- | -------- | ------------ |
| 航站衔接      | 硬约束   | 表6          |
| 航线-飞机限制 | 硬约束   | 表7          |
| 机场关闭      | 硬约束   | 表8          |
| 飞机时间      | 硬约束   | 表10         |
| 联程航班      | 软约束   | 表6、表10    |
| 台风场景      | 硬约束   | 表9          |
| 边界调整      | 硬约束   | 表7          |
| 旅客中转时间  | 软约束   | 表12         |

约束条件中，如果性质是“硬约束”的，那么不得违反。如果性质是“软约束”的，尽量遵守，不遵守会在目标函数中进行惩罚。

- 5.1.  航站衔接：同一飞机上一航班落地机场和下一航班起飞机场必须一致。
- 5.2.  航线-飞机限制：某航线不能由某飞机来飞。
- 5.3.  机场关闭：有些机场会在特定时段关闭，关闭期间不允许飞机起飞和降落。
- 5.4.  飞机过站时间：飞机在任何机场降落后，至少停留50分钟才能起飞。但由于原始数据（表6 航班表）中可能存在一个飞机的两个连续航班间隔小于50分钟的情况，那么进行航班调整后，如果这两个航班仍使用同一架飞机（两个航班都不换飞机，或同时换成另一架飞机）并连续，那么间隔时间只要不小于原间隔时间即可。
- 5.5.  联程航班：表6航班表中，日期和航班号都相同的两条记录是联程航班。联程航班的调整限制参考“8. 航班调整规则”的说明。
- 5.6.  台风场景：台风期间，受影响的机场会对经过的飞机起飞、降落或停机产生限制。
- 5.7.  边界调整：每架飞机最早（不是某一天最早，是所给数据中最早）起飞的机场不能改变。所有飞机最晚（不是某一天最晚，是所给数据中最晚）降落的机场必须满足基地平衡，即停在机场的各类飞机数量与原计划停在机场的各类飞机数量一样，最简单的实现方式就是不调整每架飞机原计划最晚执行的航班。
- 5.8.  旅客中转时间：旅客从某一航班中转至另一航班需要满足最小中转时间。

## 6. 目标函数

- 目标函数值 = p1*调机空飞航班数 + p2*取消航班数 + p3*机型发生变化的航班数 + p4*换飞机数量 + p5*联程拉直航班的个数 + p6*航班总延误时间（小时） + p7*航班总提前时间（小时）+ p8*取消旅客人数 +p9*延迟旅客人数 +p10*签转延误旅客人数。
- 在计算目标函数值时，需要根据航班的重要系数进行调整（见“表6 航班表”最后一列，“重要系数”字段），例如重要系数=2.5的航班，统计它的取消、机型变化、联程拉直、延误或者提前时，按2.5倍统计。调机航班不需要考虑重要系数，默认值为1.0。
- 统计取消航班时，不考虑联程拉直航班取消的航班。统计联程拉直航班时，以拉直后航班的起飞时刻为准，统计其延误时间或者提前时间时，其重要系数参考联程航班的第一个航班。
- 统计旅客时，不考虑提前旅客，只考虑取消旅客和延误旅客，延误旅客又分原有的延误旅客和签转的延误旅客两种，需要分别统计。

参数设置为：

（1）p1：调机参数
```
p1 = 5000
```
（2）p2：取消航班参数
```
p2 = 1200
```
（3）p3：机型变化参数

机型总共有四种，机型两两之间发生变化，参数会相应地变化，p3 = 基础分`*`系数，基础分和系数详见下表： 

  *表2 机型变化参数表*

|      | 机型变更（纵向变更为横向） |      |      |      |
| ---- | -------------------------- | ---- | ---- | ---- |
| 系数 | 1                          | 2    | 3    | 4    |
| 1    | 0                          | 0    | 2    | 4    |
| 2    | 0.5                        | 0    | 2    | 4    |
| 3    | 1.5                        | 1.5  | 0    | 2    |
| 4    | 1.5                        | 1.5  | 2    | 0    |
|      | 机型变更基础分：500        |      |      |      |

（4）p4：换飞机参数

表3 换飞机参数表

|                            | 换飞机参数 |
| -------------------------- | ---------- |
| 航班计划起飞时刻≤6日16:00  | 15         |
| 航班计划起飞时刻＞6日16:00 | 5          |

注：选手最终方案与原航班计划比较，当每个航班号对应的飞机号发生变更，则产生上述惩罚分值。

（5）p5：联程拉直参数

```
p5 = 750
```

（6）p6：航班延误时间参数

```
p6 =100
```

（7）p7：航班提前时间参数


- ​     p7 = 150

（8）p8：旅客行程取消参数


- ​     p8 = 4

（9）p9：旅客行程延误参数

旅客行程延误参数采用阶梯方式，根据不同延误时间段选取不同延误分值，详见下表：

​     表4 旅客行程延误参数

| 航班延误时间（小时） | 航班延误旅客（每人） |
| -------------------- | -------------------- |
| （0,2]               | 1                    |
| (2,4]                | 1.5                  |
| (4,8]                | 2                    |
| (8,36]               | 3                    |
| (36,+∞)              | 直接取消             |

（9）p10：旅客签转延误参数

​     旅客签转延误参数根据签转延误的间隔时间不同，选择不同惩罚分值，详见下表：

​     表5 旅客签转参数

| 签转旅客延误时间（小时） | 签转旅客罚分（区间线性增长） | 签转旅客区间罚分最大值 |
| ------------------------ | ---------------------------- | ---------------------- |
| [0,6）                   | 1/30*延误时间                | 0.2                    |
| [6,12)                   | 1/24*延误时间                | 0.5                    |
| [12,24)                  | 1/24*延误时间                | 1                      |
| [24,36)                  | 1/18*延误时间                | 2                      |
| [36,48]                  | 1/16*延误时间                | 3                      |
| (48,+∞)                  | 直接取消                     | +∞                     |



**目标函数具体计算方式详见下面例子：**

例如，原来有100个航班（其中1个航班的重要系数=2.5，其他航班的重要系数=1.0），重新排班后，调整情况如下：

- 1)    1个航班调机
- 2)   4个航班取消（其中有1个是因为联程拉直被取消，另外3个是非联程航班，其中包含1个重要系数=2.5的航班），其中一班航班78人改签到一班未受影响的航班（改签延误6.5h），另外12名乘客取消。其余3班航班共337名乘客取消。
- 3)   2个航班计划起飞时刻≤6日16:00的航班由原来的机型3换成了机型2，另有2个航班计划起飞时刻＞6日16:00的航班进行了同机型更换。 
- 4)   1对联程航班（即2个航班）被拉直
- 5)   2个航班延误（其中1个航班乘客151人延误1.2小时，1个重要系数为2.5的乘客数为180人的航班延误3小时）    
- 6)   3个航班提前（其中1个航班提前2小时（乘客140人），2个航班提前1小时（乘客共210人））

那么，

```
目标函数值 = 5000*1*1.0 + 1000*(2*1.0+1*2.5)+ 500*2*2*1.0 + (15*2*1.0+5*2*1.0) + 750*(1.0+1.0) + 100*(1*1.2*1.0 + 1*3 *2.5)+ 150*(1*2 * 1.0 + 2*1*1.0) + 4*(12+337) +（1*151+1.5*180）+0.5*78
```

## 7. 数据说明     

### 表6 航班表

| 航班ID | 日期      | 国际/国内 | 航班号 | 起飞机场 | 降落机场 | 起飞时间        | 降落时间        | 飞机ID | 机型 | 旅客数 | 联程旅客数 | 座位数 | 重要系数 |
| ------ | --------- | --------- | ------ | -------- | -------- | --------------- | --------------- | ------ | ---- | ------ | ---------- | ------ | -------- |
| 1      | 2016/6/15 | 国内      | 1      | 北京     | 武汉     | 2016/6/15 14:47 | 2016/6/15 16:33 | 1      | 7    | 85     | 0          | 213    | 1.0      |
| 2      | 2016/6/19 | 国际      | 2      | 武汉     | 巴黎     | 2016/6/15 12:27 | 2016/6/15 21:13 | 2      | 8    | 110    | 0          | 163    | 2.5      |
| 3      | 2016/6/20 | 国内      | 3      | 上海     | 武汉     | 2016/6/20 10:12 | 2016/6/20 11:58 | 3      | 7    | 92     | 37         | 163    | 1.0      |
| 4      | 2016/6/20 | 国内      | 3      | 武汉     | 北京     | 2016/6/20 14:32 | 2016/6/20 16:58 | 3      | 7    | 84     | 37         | 155    | 1.5      |
| …      | …         |           |        |          |          |                 |                 |        |      |        |            |        |          |

表6中每一行数据都有唯一的航班ID来标识航班，且航班中日期和航班号相同的两个航班就是联程航班，例如第3、4两行，表示有129 = 92 + 37名旅客从上海到武汉，武汉下机92名旅客，剩下37人（购买上海-武汉-北京联程机票的乘客）加上新登机84人继续从武汉飞往北京。

本次赛题至多考虑两段联程（不考虑3段及以上联程）。

### 表7 航线-飞机限制表

| **起飞机场** | **降落机场** | **飞机ID** |
| ------------ | ------------ | ---------- |
| 拉萨         | 长沙         | 1          |
| 北京         | 香港         | 2          |
| 深圳         | 厦门         | 3          |

表7中列出不允许的航线 + 飞机组合。例如第1行表示从拉萨到长沙的航线不能由飞机1来飞。

### 表8 机场关闭限制表

| **机场** | **关闭时间** | **开放时间** | **生效日期** | **失效日期** |
| -------- | ------------ | ------------ | ------------ | ------------ |
| 北京     | 23:00        | 6:00         | 2016/6/15    | 2016/8/15    |
| 上海     | 2:00         | 10:00        | 2016/8/12    | 2016/8/15    |

表8中列出机场关闭的时间。例如第1行表示北京机场每天23:00到次日6:00关闭（2016/6/15日零点生效，2016/8/15日零点失效），第2行表示上海机场每天2:00到10:00关闭（2016/8/12日零点生效，2016/8/15日零点失效）。

### 表9 台风场景表

| **开始时间**    | **结束时间**    | **影响类型** | **机场** | **航班ID** | **飞机** | **停机数** |
| --------------- | --------------- | ------------ | -------- | ---------- | -------- | ---------- |
| 2016/6/16 16:00 | 2016/6/17 17:00 | 起飞         | 厦门     |            | 1        |            |
| 2016/6/16 14:00 | 2016/6/17 17:00 | 起飞         | 泉州     |            |          |            |
| 2016/6/16 16:00 | 2016/6/17 17:00 | 降落         | 厦门     |            |          |            |
| 2016/6/16 16:00 | 2016/6/17 17:00 | 停机         | 厦门     |            |          | 2          |

表9列出影响飞机运行的台风场景。影响类型有3种：起飞、降落和停机。例如第1行某时段厦门机场禁止飞机1起飞，第2行某时段泉州机场禁止任何飞机起飞，第3行表示厦门机场某时段禁止任何飞机降落，第4行表示厦门机场某时段内停留的飞机数量不能超过2架（例如停在机库过夜）。如果停机数的值为空，则表示机场此时间段内不能停留任何飞机 。

停机说明：经常受台风影响的机场一般具有少量台风停机位，或者作为重要枢纽的机场停机位容量有限，只能满足一定数量的飞机停靠在机场内，但超出停机位容纳范围的飞机必须在机场停机限制前飞离机场。同时，如果航班数据中某架飞机的最早执行航班一开始起飞就处于停机时间段内，通过航班延后调整后，并不违背停机约束。因为该架飞机执行最早航班之前的位置未知，有可能是由外场飞入该场的飞机，也可能是停靠在维修仓库中。

### 表10 飞行时间表

| **机型** | **起飞机场** | **降落机场** | **飞行时间（分钟）** |
| -------- | ------------ | ------------ | -------------------- |
| 1        | 北京         | 上海         | 60                   |
| 1        | 杭州         | 上海         | 75                   |
| 3        | 北京         | 上海         | 80                   |
| …        | …            | …            | …                    |

表10列出不同机型的飞机在两个机场之间的飞行时间。例如第1行表示，机型为1的飞机从北京飞到上海的时间为60分钟。飞行时间表主要用于调机和联程拉直时调整时间，具体调整规则参考“8 航班调整部分规则”。

### 表11  机场表

| **机场** | **国内/国际** |
| -------- | ------------- |
| **23**   | **1**         |
| **45**   | **0**         |
| **56**   | **1**         |
| **……**   | **……**        |

### 表11列出了机场的属性，

1表示国内机场，0表示国际航班，用于航班提前、联程航班拉直和调机。

### 表12 旅客中转时间限制表

| **进港航班ID** | **出港航班ID** | **最短转机时限（分钟）** | **中转旅客数量** |
| -------------- | -------------- | ------------------------ | ---------------- |
| 1              | 2              | 60                       | 18               |
| 5              | 6              | 75                       | 16               |
| …              | …              | …                        | …                |

表12列出了旅客中转航班的最短转机时间以及中转旅客数量，例如第一行表示，18名旅客乘航班1降落后，需要乘坐航班2转机，最短的转机时间为60分钟。

## 8. 航班调整规则

### 8.1.  航班时刻提前

​    航班提前会导致乘客体验比较差，因此未受台风影响的航班不允许提前。航班时刻提前的限定条件是：

​    1)  仅针对在受台风影响的禁止起飞时间段，从受影响机场起飞的航班。例如厦门机场受台风影响，14:00后禁止落地，16:00后禁止起飞。那么原计划16:20在厦门机场起飞的航班，可以提前至16:00之前起飞。

​    2)  仅针对国内航班。

​    3) 最大提前时间6小时。

### 8.2.  航班时刻延误

​     航班可以延误，但必须满足航班最大延误时间的限制，国内航班不得大于24小时，国际航班延误不得大于36小时。

### 8.3.  联程航班调整

​     联程航班调整的方式有四种：联程拉直、全部执行、全部取消、部分取消。联程航班调整的限定条件是：

​    1）如果两段都不取消，那么两段必须使用同一架飞机，并且继续联程。参考约束条件“5.4. 飞机过站时间”，即两段时间间隔不得小于50分钟或原来间隔时间。例如航班号 = 3的航班，调整后两段都仍然使用飞机id = 3的飞机，或者两段都换成飞机id = 8的飞机，那么后一段的起飞时间减去前一段的降落时间，不得小于50分钟或原来间隔时间。

​    2）仅当两段都是国内航班（参考“表6航班表”中数据列“国际/国内”），且中间机场受影响时，才可拉直联程航班。例如联程航班A->B->C（其中A、B、C表示机场），B机场受台风影响，A->B的降落时间在B机场的禁止降落时间内，或B->C的起飞时间在B机场的禁止起飞时间内。

​    3）拉直联程航班的时间调整与结果展示：例如联程航班A->B->C，其中A、B、C表示机场，当拉直后，取消B->C的航班，将A->B的航班调整成A->C，其中A->C的飞行时间参考“表10 飞行时间表”。如果表10查不到A->C的飞行时间，则取A->B和B->C的飞行时间之和。

### 8.4.  调机

​    调机的限定条件是：

​    1) 调机必须使用在“表6 航班表”中出现的飞机。

​    2) 只有国内航班（起飞机场和降落机场都是国内机场）允许调机。

​    3) 调机时间调整：例如飞机从机场A调机空飞到机场B，飞行时间参考“表10 飞行时间表”。如果表10中查不到A->B的飞行时间，则不允许调机。

​    4) 调机的航班，需要新增航班id来表示，从9001开始依次加1，分别是9001，9002，….

​    5) 调机航班不能签转旅客。

### 8.5.  单位时间容量限制

​    台风过后，受影响的机场逐渐恢复秩序，但恢复机场的容量是有限的。特限定起飞或者降落受影响的机场在5月6日16:00前一小时，5月7日17:00后两小时，每5分钟内仅允许2个航班起飞和降落,以避免提前航班起飞时刻集中及恢复时航班集中到港。

### 8.6.  恢复窗口限制

​    由于5月5日的所有航班都不受台风影响，调整后不利于航空公司运营，特限定航班计划起飞时刻位于5月6日06:00-5月8日24:00之间才能调整。其中联程航班中只要有一个航班位于非恢复窗口内，且另一个航班位于恢复窗口内，则不能取消，可以调整。

### 8.7.  换飞机

​    换飞机执行航班，会产生相应的惩罚（表3）。如果同时更换了机型，则也会产生相应的惩罚（表2）。延误和提前的计算都是以航班起飞时刻为准。

### 8.8.  机型变化

​    当航班发生机型变更时，由于各机型的座位数不同，需考虑因大机型变更为小机型带来的旅客损失。假设执行航班的飞机机型由机型1（乘客数160）变更为机型2（座位数127）时，将会损失33名旅客。

### 8.9.  旅客签转

​    当航班取消、中转失败等操作都会带来旅客的流失，为了减少经济损失和提高用户体验，旅客可以签转到其他航班。旅客分为普通旅客、中转旅客（中转出去）和联程旅客三种，其中普通旅客由当地旅客和其他航班中转进来的旅客组成，中转旅客代表当前航班结束后需要中转到其他航班的旅客。表6航班表中旅客数为普通旅客数和中转旅客数之和。同时赛题限定中转旅客只考虑中转一次的情况，即一个航班中中转进来的旅客和中转出去的旅客是独立的。旅客签转的限定条件是：

​    1) 只有航班取消、中转失败、航班拉直、换飞机、机型变化、超售导致流失的旅客才能签转，且只有普通旅客才能签转。

​    2) 签转的旅客不能提前。

​    3) 签转旅客到某航班，必须满足该航班的座位数限制。

​    4) 在恢复窗口限制范围之外超售的旅客不能签转，只能取消。

​    5) 签转旅客只能签转到与原航班起飞机场和降落机场一致的普通航班或者联程拉直后的航班。同时联程旅客不能签转。

​    6) 航班中的中转旅客（中转出去）不能签转。

​    7) 接受签转旅客的航班不能签转旅客到其他航班，例如航班A签转部分旅客到航班B，则航班B不能签转任何旅客到其他任何航班。

## 9. 提交结果

### 9.1.  程序输出结果

csv文件（逗号分割），文件名为选手昵称+下划线+目标函数值（小数点后3位）+下划线+计算时间（分，取整数），例如选手“aaa”的成绩是90000.187，计算时间12分钟，那么文件名就是“aaa_90000.187_12.csv”。文件格式如下（提交时统一规定不要包含表头）。

表13 结果数据

| 航班ID | 起飞机场 | 降落机场 | 起飞时间        | 降落时间        | 飞机ID | 是否取消 | 是否拉直 | 是否调机 | 是否签转 | 签转旅客情况 |
| ------ | -------- | -------- | --------------- | --------------- | ------ | -------- | -------- | -------- | -------- | ------------ |
| 1      | 北京     | 武汉     | 2016/6/15 14:47 | 2016/6/15 16:33 | 1      | 1        | 0        | 0        | 0        |              |
| 2      | 武汉     | 上海     | 2016/6/15 19:27 | 2016/6/15 21:13 | 2      | 0        | 0        | 0        | 0        |              |
| 3      | 武汉     | 上海     | 2016/6/15 10:12 | 2016/6/15 11:58 | 3      | 0        | 1        | 0        | 1        | 6:11         |
| 4      | 重庆     | 上海     | 2016/6/15 14:32 | 2016/6/15 16:58 | 3      | 1        | 1        | 0        | 0        |              |
| 9001   | 北京     | 沈阳     | 2016/6/16 21:40 | 2016/6/16 23:50 | 4      | 0        | 0        | 1        | 0        |              |
| 5      | 武汉     | 上海     | 2016/6/15 10:00 | 2016/6/15 12:00 | 1      | 1        | 0        | 0        | 1        | 2:31&3:12    |
| 6      | 武汉     | 重庆     | 2016/6/16 10:40 | 2016/6/16 13:50 | 2      | 0        | 0        | 0        | 0        |              |

注：


- （1）是否取消、是否拉直、是否调机、是否签转这4列，“1”表示是，“0”表示否。
- （2）结果数据中未作调整的字段原样输出，调整的字段按调整后结果输出。
- （3）计算目标函数值，统计取消航班数时，不统计联程拉直的航班，比如上表中航班ID = 4的航班，虽然标记了取消，但是在统计取消航班时不统计它，只在联程拉直时统计。
- （4）取消的航班涉及到旅客签转，必须在“签转旅客情况”一列罗列签转情况，格式参考表13中航班5呈现的方式（航班ID:旅客数&航班ID:旅客数&…），“2:31&3:12”表示签转31名旅客到航班2上，签转12名旅客到航班3上。同时如果航班由于中转失败、航班拉直、换飞机、机型变化、超售导致旅客的流失，旅客需要进行签转，则需要在对应的航班后标记签转情况。例如表13中航班3由于拉直导致11名旅客签转到航班6上。

### 9.2. 源代码 。

### 9.3. 技术文档

（包括使用说明和测试文档）。

每次提交时至少要有程序输出结果，最后一次提交时需要提交源代码和技术文档。

