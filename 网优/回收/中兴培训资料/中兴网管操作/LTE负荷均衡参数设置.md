

## 负荷均衡参数表

| 参数字段名                                    | 参数名称                              | 1.8&2.6&2.1             |
| --------------------------------------------- | ------------------------------------- | ----------------------- |
| lbSwch                                        | 负荷均衡算法开关                      | 2                       |
| prbIntraNeighborLoadThrdUl                    | 上行Intra-LTE邻小区PRB过负荷门限(%)   | 80                      |
| prbIntraNeighborLoadThrdDl                    | 下行Intra-LTE邻小区PRB过负荷门限(%)   | 80                      |
| prbIntraNeighborLoadRelaThrdUl                | 上行Intra-LTE邻小区PRB相对负荷门限(%) | 8                       |
| prbIntraNeighborLoadRelaThrdDl                | 下行Intra-LTE邻小区PRB相对负荷门限(%) | 8                       |
| lbPeriod                                      | 负荷均衡执行周期(秒)                  | 20                      |
| loadTriggerMode                               | 负荷均衡的触发模式                    | 0                       |
| ngbrPRBFactor                                 | NGBR业务PRB比例因子                   | 12                      |
|                                               | 上行异系统邻小区过负荷门限            | 10                      |
|                                               | 下行异系统邻小区过负荷门限            | 10                      |
| prbLBExeThrdZUl                               | 上行同厂商PRB负荷均衡执行门限(%)      | 35                      |
| prbLBExeThrdZDl                               | 下行同厂商PRB负荷均衡执行门限(%)      | 35                      |
| prbOffloadThrdUL                              | 上行PRB卸载负荷门限(%)                | 80                      |
| prbOffloadThrdDL                              | 下行PRB卸载负荷门限(%)                | 80                      |
| allowHLNbrNum                                 | 允许负荷重于本小区的邻小区数目        | 12                      |
| intraLBFreqPriorSwch                          | Intra-LTE负荷均衡频点优先级策略开关   | 1                       |
| lbRATPriority                                 | 负荷均衡系统优先级指示                | 255;254;0;0;0;0;0       |
| lbIntraFreqPriority                           | Intra-LTE负荷均衡同频频点优先级       | 0                       |
| lbInterFreqPriority（系统内异频结构体的字段） | Intra-LTE负荷均衡异频频点优先级       | 0;255;255;0             |
| lbUESelSchemePrio                             | 负荷均衡用户选择策略优先级            | 251;252;253;250;255;254 |
| numMeasureUE                                  | 下发事件测量配置的用户数              | 10                      |
| thresholdOfRSRP                               | 事件判决的RSRP门限(dBm)（250/252）    | -100                    |
| numHOUE                                       | 降负荷用户数                          | 10                      |

## 负荷均衡参数明细

### 注意事项

> 1、扩容小区记录好高负荷小区对应的扩容小区，方便参数修改及指标观察；
> 2、高负荷站点CA解除；
> 3、高话务站点1.8G/2.1G功率保持基本一致，差值不超过1dB，并定期核查；
> 4、如对覆盖无硬性要求快速配置可考虑适当降低流量大频段小区功率（可导致MR覆盖率下降）；
> 5、重选参数包括重选优先级保持一致并定期核查；
> 6、异频切换参数、测量参数需定期核查，建议按照前期负荷均衡配置方案使用，对于效果不明显的可通过调整A1、A2门限、增加一套新的测量配置索引，1.8G和2.1G小区分别引用不同的索引，以均衡话务；
> 7、Qcell可通过小区分裂提供更多小区；
> 8、涉及高校等无流量收费且话务极高区域需与客户沟通是否使用限制RB数、AMBR限速功能；
> 9、RF调整涉及覆盖变化，调整后涉及多个维度评估，现场慎用；
> 10、扩容需考虑BP板负荷；

### 基于PRB连接态
| 编号 | 参数在LB功能的作用归类 | 管理对象名称 | Manage Object Name | 参数字名| 参数名称  | 设置建议值 | 备注 |
| :-: | :-:| :-: | :-: | :-: | :-:| :-: | :-: |
| 1| 源侧负荷触发相关| 小区负荷均衡配置表 | LoadMNGCell| prbLBExeThrdZUl| 上行同厂商PRB负荷均衡执行门限(%) | 5          | 设置越小，越容易触发 |
| 2    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbLBExeThrdZDl                | 下行同厂商PRB负荷均衡执行门限(%)      | 5          | 设置越小，越容易触发                                         |
| 3    | 对于MR选择并执行       | 小区负荷均衡配置表 | LoadMNGCell         | numHOUE                        | 降负荷用户数                          | 10         | 绝大多数情况下，10应该是足够的。                             |
| 4    | 选择参与测量的UE       | 小区负荷均衡配置表 | LoadMNGCell         | numMeasureUE                   | 下发事件测量配置的用户数              | 15         | 10~20均可，但是不要过大，加重系统CPU负荷                     |
| 5    | 总开关                 | 小区负荷均衡配置表 | LoadMNGCell         | lbSwch                         | 负荷均衡算法开关                      | 2          | 设置为1，表示盲切换；设置为2，表示基于测量。视成效设置。     |
| 6    | 测量的目标邻区选择     | 小区负荷均衡配置表 | LoadMNGCell         | intraLBFreqPriorSwch           | Intra-LTE负荷均衡频点优先级策略开关   | 1          | 不能为0，否则在测量方式下会将同频也纳入LB的目标范围          |
| 7    | 测量的目标邻区选择     | 小区负荷均衡配置表 | LoadMNGCell         | lbRATPriority                  | 负荷均衡系统优先级指示                | 默认       | 负荷均衡的目标系统优先级，根据实际情况设置。在本场景中定义的是系统内异频，那么可以将FDD  LTE和TD LTE系统优先级设置最高。 |
| 8    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | lbPeriod                       | 负荷均衡执行周期(秒)                  | 30         | 可以设置为20~30s                                             |
| 9    | 选择参与测量的UE       | 小区负荷均衡配置表 | LoadMNGCell         | PRBQueueWeight                 | PRB队列权重因子(%)                    | 70         | 默认为70%，设置越大，那么选择出来的UE更可能是占用RB资源多的UE |
| 10   | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | allowHLNbrNum                  | 允许负荷重于本小区的邻小区数目        | 1          | 设置越大，越容易触发                                         |
| 11   | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | loadTriggerMode                | 负荷均衡的触发模式                    | 0          | 0为PRB资源利用率方式                                         |
| 12   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | neighLoadJudgeMode             | 邻区/频点选择模式                     | 0          | 建议为独立判决，如果设置为联合判决，更难以筛选出负荷低的邻区 |
| 13   | 选择参与测量的UE       | 小区负荷均衡配置表 | LoadMNGCell         | prbBasedUESortOrder            | 基于PRB负荷均衡用户排序开关           | 1          |                                                              |
| 14   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | neighLoadJudgeMode             | 邻区/频点选择模式                     | 0          | 建议为独立判决，如果设置为联合判决，更难以筛选出负荷低的邻区 |
| 15   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbIntraNeighborLoadThrdUl     | 上行Intra-LTE邻小区PRB过负荷门限(%)   | 90         | 设置越大，越容易找到满足条件的邻区                           |
| 16   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbIntraNeighborLoadThrdDl     | 下行Intra-LTE邻小区PRB过负荷门限(%)   | 90         | 设置越大，越容易找到满足条件的邻区                           |
| 17   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbIntraNeighborLoadRelaThrdUl | 上行Intra-LTE邻小区PRB相对负荷门限(%) | 2          | 设置越小，越容易找到满足条件的邻区                           |
| 18   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbIntraNeighborLoadRelaThrdDl | 下行Intra-LTE邻小区PRB相对负荷门限(%) | 2          | 设置越小，越容易找到满足条件的邻区                           |
| 19   | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbOffloadThrdUL               | 上行PRB卸载负荷门限(%)                | 25         | 需要大于prbLBExeThrdZUl。如果现网X2链路配置不全面，那么该门限会有作用，设置小一点，会让系统更早地将那些没有负荷的邻区也作为LB的目标考虑范围 |
| 20   | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | prbOffloadThrdDL               | 下行PRB卸载负荷门限(%)                | 25         | 需要大于prbLBExeThrdZDl。如果现网X2链路配置不全面，那么该门限会有作用，设置小一点，会让系统更早地将那些没有负荷的邻区也作为LB的目标考虑范围 |
| 21   | 下发的测量配置         | 小区负荷均衡配置表 | LoadMNGCell         | lbPRBUEPosInfSwch              | 基于PRB负荷均衡用户位置信息开关       | 0          | 设置为1时候默认调用252号，设置为0，默认调用250号。但是实际上绝大多数情况下不用弄这么复杂，建议该开关设置为0，然后将250、252号测量配置门限设置为一样。 |
| 22   | 联动参数               | 负荷管理           | LoadManagement      | interNeighborLoadThrdUI        | 上行异系统邻小区过负荷门限            | 5          | 必须小于负荷均衡执行门限                                     |
| 23   | 联动参数               | 负荷管理           | LoadManagement      | interNeighborLoadThrdDI        | 上行异系统邻小区过负荷门限            | 5          | 必须小于负荷均衡执行门限                                     |
| 24   | 源侧负荷触发相关       | 负荷管理           | LoadManagement      | lbOverlapCoverageThrd          | 负荷均衡邻区关系关联度门限            | 60         | 必须大于邻接关系普通邻区关系关联系数小于CA站点邻区关系关联系数 |
| 25   | 源侧负荷触发相关       | E-UTRAN邻接关系    | EUtranRelation      | OverlapCoverage               | 邻区关系关联系数                      | 90         | 协作类型为CA的邻区配置   |

### 基于PRB驻留态

| 编号 | 参数在LB功能的作用归类 | 管理对象名称       | Manage Object  Name | 参数字段名      | 参数名称                         | 设置建议值 | 备注                  |
| ---- | ---------------------- | ------------------ | ------------------- | --------------- | -------------------------------- | ---------- | --------------------- |
| 1    | 总开关                 | 小区负荷均衡配置表 | LoadMNGCell         | interCLBSwitch  | 驻留负荷均衡算法开关             | 1          | 设置为1，表示通用策略 |
| 2    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | clbPRBExeThrdUl | 上行PRB驻留态负荷均衡执行门限(%) | 5          | 设置越低，越容易触发  |
| 3    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | clbPRBExeThrdDl | 下行PRB驻留态负荷均衡执行门限(%) | 5          | 设置越低，越容易触发  |

### 基于CU连接态

| 编号 | 参数在LB功能的作用归类 | 管理对象名称       | Manage Object  Name | 参数字段名                  | 参数名称                            | 设置建议值 | 备注                                                         |
| ---- | ---------------------- | ------------------ | ------------------- | --------------------------- | ----------------------------------- | ---------- | ------------------------------------------------------------ |
| 1    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | lbUeNumThrd                 | 负荷均衡的RRC连接用户数门限         | 50         | 设置越小，越容易触发                                         |
| 2    | 对于MR选择并执行       | 小区负荷均衡配置表 | LoadMNGCell         | numHOUE                     | 降负荷用户数                        | 10         | 绝大多数情况下，10应该是足够的。越大一次降用户数越多。但是太多加重负荷 |
| 3    | 选择参与测量的UE       | 小区负荷均衡配置表 | LoadMNGCell         | numMeasureUE                | 下发事件测量配置的用户数            | 15         | 10~20均可，但是不要过大，加重系统CPU负荷                     |
| 4    | 总开关                 | 小区负荷均衡配置表 | LoadMNGCell         | lbSwch                      | 负荷均衡算法开关                    | 2          | 设置为1，表示盲切换；设置为2，表示基于测量。视成效设置。     |
| 5    | 测量的目标邻区选择     | 小区负荷均衡配置表 | LoadMNGCell         | intraLBFreqPriorSwch        | Intra-LTE负荷均衡频点优先级策略开关 | 1          | 不能为0，否则在测量方式下会将同频也纳入LB的目标范围          |
| 6    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | lbPeriod                    | 负荷均衡执行周期(秒)                | 30         | 可以设置为20~30s，越短同时段次数越多                         |
| 7    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | allowHLNbrNum               | 允许负荷重于本小区的邻小区数目      | 1          | 设置越大，越容易触发                                         |
| 8    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | loadTriggerMode             | 负荷均衡的触发模式                  | 1          | 1为基于CU的负荷均衡，就是用户数                              |
| 11   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | cuIntraNeighborLoadThrd     | 同厂商用户数负荷均衡执行门限        | 5          | 设置越小越容易触发                                           |
| 12   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | cuIntraNeighborLoadRelaThrd | Intra-LTE邻小区用户数过负荷门限     | 90         | 设置越小，邻区越容易满足                                     |
| 13   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | cuOffloadThrd               | Intra-LTE邻小区用户数相对负荷门限   | 2          | 设置越小，越容易满足邻区要求                                 |
| 14   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | cuOffloadThrd               | 用户数卸载负荷门限                  | 25         | 取值越大，向无法获取的负荷信息的小区均衡的可能性越小         |
| 15   | 邻区负荷测量相关       | 小区负荷均衡配置表 | LoadMNGCell         | neighLoadJudgeMode          | 邻区/频点选择模式                   | 0          | 建议为独立判决，如果设置为联合判决，更难以筛选出负荷低的邻区 |
| 20   | 联动参数               | 负荷管理           | LoadManagement      | interNeighborLoadThrdUI     | 上行异系统邻小区过负荷门限          | 5          | 必须小于负荷均衡执行门限                                     |
| 21   | 联动参数               | 负荷管理           | LoadManagement      | interNeighborLoadThrdDI     | 上行异系统邻小区过负荷门限          | 5          | 必须小于负荷均衡执行门限                                     |
| 22   | 源侧负荷触发相关       | 负荷管理           | LoadManagement      | lbOverlapCoverageThrd       | 负荷均衡邻区关系关联度门限          | 60         | 必须大于邻接关系普通邻区关系关联系数小于CA站点邻区关系关联系数 |
| 23   | 源侧负荷触发相关       | E-UTRAN邻接关系    | EUtranRelation      | OverlapCoverage             | 邻区关系关联系数                    | 90         | 协作类型为CA的邻区配置                                       |

### 基于CU驻留态

| 编号 | 参数在LB功能的作用归类 | 管理对象名称       | Manage Object  Name | 参数字段名     | 参数名称                         | 设置建议值 | 备注                  |
| ---- | ---------------------- | ------------------ | ------------------- | -------------- | -------------------------------- | ---------- | --------------------- |
| 1    | 总开关                 | 小区负荷均衡配置表 | LoadMNGCell         | interCLBSwitch | 驻留负荷均衡算法开关             | 1          | 设置为1，表示通用策略 |
| 2    | 源侧负荷触发相关       | 小区负荷均衡配置表 | LoadMNGCell         | lbCUExeThrd    | 基于用户数驻留态负荷均衡执行门限 | 20         | 设置越低，越容易触发  |

### 重选（可选）

| 编号 | 管理对象名称    | Manage Object  Name | 参数字段名                  | 参数名称        | 设置建议值 | 备注                                                         |
| ---- | --------------- | ------------------- | --------------------------- | --------------- | ---------- | ------------------------------------------------------------ |
| 1    | E-UTRAN小区重选 | EUtranReselection   | 同/低优先级RSRP测量判决门限 | snonintrasearch | 48         | 设置越大越易触发重选                                         |
| 2    | E-UTRAN小区重选 | EUtranReselection   | 服务小区重选迟滞            | qhyst           | 2          | 根据重选公式，减小此值，服务小区R排序降低                    |
| 3    | E-UTRAN小区重选 | EUtranReselection   | 频间频率偏移值              | qOffsetFreq     | -6         | 在异频载频重选结构体中，期望驻留的频段进行设置。根据重选公式，减小此值，邻区小区R排序升高。根据情况减小 |

注意：如负荷均衡效果不佳再行调整