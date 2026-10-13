# Chapter 7: CPU Scheduling — 结果概述

本作业使用 `scheduler.py` 模拟器（官方 OSTEP Homework：https://github.com/remzi-arpacidusseau/ostep-homework/tree/master/cpu-sched），在统一工作负载下比较 **FIFO、SJF、RR** 三种 CPU 调度策略的**平均响应时间**与**平均周转时间**，并考察作业长度、量子长度对指标的影响。

## 核心结论

- **周转时间**：SJF（最短作业优先）最优，因为它先跑完短作业；FIFO 在"长作业先到"时会出现车队效应而变差；RR 因频繁抢占，周转时间往往最差。
- **响应时间**：RR（轮转）最优，时间片越小响应越好；FIFO/SJF 的响应时间随排在队前的作业变长而线性变差。
- **根本权衡**：优化周转（不公平地先跑短作业）以牺牲响应为代价；优化响应（公平轮转）以牺牲周转为代价。

## 结果摘要

| 题目 | 场景 | 平均响应 | 平均周转 |
|---|---|---|---|
| Q1 | FIFO / SJF，3×200 | 200.00 | 400.00 |
| Q2 | FIFO / SJF，100,200,300 | 133.33 | 333.33 |
| Q3 | RR(q=1)，100,200,300 | 1.00 | 465.67 |
| Q6 | SJF，10,20,30 | 13.33 | 33.33 |
| Q6 | SJF，1000,2000,3000 | 1333.33 | 3333.33 |
| Q7 | RR(q=10)，100,200,300 | 10.00 | 456.67 |
| Q7 | RR(q=100)，100,200,300 | 100.00 | 366.67 |

> 详细推导、预测与解释见 `analysis.md`；全部原始模拟器输出见 `data/`。

## 文件结构

```
hw/chapter7/
├── README.md
├── analysis.md          # Q1–Q7 书面答案（预测 + 验证 + 解释）
└── data/                # 每道题的模拟器原始输出
    ├── q1_fifo.txt
    ├── q1_sjf.txt
    ├── q2_fifo.txt
    ├── q2_sjf.txt
    ├── q3_rr_q1.txt
    ├── q6_sjf_10_20_30.txt
    ├── q6_sjf_1000_2000_3000.txt
    ├── q7_rr_q10.txt
    └── q7_rr_q100.txt
```

## 如何复现

在 Codespace 根目录 `/workspaces/ostep-codespace-lab` 下运行，例如：

```bash
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -p FIFO -l 100,200,300 -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -p SJF  -l 100,200,300 -c
python3 ./ext/ostep-homework/cpu-sched/scheduler.py -p RR   -l 100,200,300 -q 1 -c
```

`-c` 会输出完整执行轨迹与各作业/平均的响应、周转、等待时间。
