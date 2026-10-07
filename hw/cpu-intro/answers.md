]633;E;for i in 1 2 3 4 5 6 7 8;ee5276bc-d7be-433c-b290-7dde9a199956]633;C## Q1
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q2
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q3
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q4
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q5
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q6
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q7
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q8
- Prediction / 预测:
- Reasoning / 理由:
- Verified result / 验证结果:
- Analysis / 分析:

## Q1
- Prediction / 预测:
Total Time = 10 ticks，CPU利用率 = 100%
- Reasoning / 理由:
两个进程都是纯CPU指令（5:100，无IO）。没有IO事件，不会触发进程切换。PID0先连续跑5个tick完成，然后切换到PID1连续跑5个tick。总共5+5=10 tick。全程CPU一直在执行任务，没有空闲，CPU利用率100%。
- Verified result / 验证结果:
Total Time = 10 ticks，CPU利用率 = 100.00%
- Analysis / 分析:
预测结果与模拟器运行结果完全一致。两个进程均为纯CPU指令，没有IO事件，不会触发进程切换。PID0执行完毕后才运行PID1，CPU全程都在工作，没有空闲，CPU利用率100%。
## Q2
- Prediction / 预测:
Total Time = 12 ticks，CPU利用率 = 5/12 ≈ 41.67%
- Reasoning / 理由:
PID0有4条CPU指令，连续执行4tick完成；之后运行PID1，PID1是1条IO指令。
PID1：RUN:io(1tick) → IO阻塞5tick → IO完成RUN:io_done(1tick)，PID1一共7tick。
总时间 = 4 + 7 = 12 tick。
CPU有效工作tick：4+2=5 tick，CPU利用率5/12。
- Verified result / 验证结果:
Total Time = 11 ticks，CPU利用率 = 54.55%
- Analysis / 分析:
预测结果与模拟器运行结果不一致。原预测总时间12ticks，实际为11ticks；原预测CPU利用率41.67%，实际54.55%。PID0执行4条CPU指令完成后，PID1发起IO，IO阻塞期间CPU空闲；IO完成后系统直接结束，因此总耗时比预估少，CPU利用率高于预测。
## Q3
- Prediction / 预测:
Total Time = 8 ticks，CPU利用率 = 5/8 = 62.5%
- Reasoning / 理由:
PID0是1条IO指令，PID1是4条CPU指令。默认SWITCH_ON_IO。
PID0执行1tick发起IO，进入阻塞；此时CPU切换到PID1并行跑CPU任务。
PID0的IO阻塞5tick期间，PID1刚好跑完4条CPU指令。
IO结束后，PID0再花1tick完成io_done。
总耗时 1+5+1 = 8 tick。
CPU工作tick：PID0(2)+PID1(4)=6 tick，利用率6/8=75%。
- Verified result / 验证结果:
Total Time = 7 ticks，CPU利用率 = 85.71%
- Analysis / 分析:
预测结果与模拟器运行结果不一致。原预测总时间8ticks，实际为7ticks；原预测CPU利用率62.5%，实际85.71%。开启SWITCH_ON_IO，PID0发起IO后立刻切换到PID1执行CPU任务，IO与CPU计算并行执行，极大提升CPU利用率，总耗时比预估更少。
## Q4
- Prediction / 预测:
Total Time = 12 ticks，CPU利用率 = 5/12 ≈ 41.67%
- Reasoning / 理由:
参数`-S SWITCH_ON_END`，只有进程全部结束才切换，不会在IO阻塞时切换。
PID0（1条IO指令）从头到尾跑完7tick（1+5+1），结束后才运行PID1的4条CPU指令（4tick）。
总时间=7+4=11 tick。CPU工作tick=2+4=6，利用率6/11≈54.55%。
- Verified result / 验证结果:
Total Time = 11 ticks，CPU利用率 = 54.55%
- Analysis / 分析:
预测结果与模拟器运行结果基本一致。开启SWITCH_ON_END，只有进程结束时才切换，PID0发起IO进入阻塞期间，CPU不会切换到PID1，CPU处于空闲状态；直到PID0的IO全部完成后，才切换到PID1执行CPU任务。相比Q3的SWITCH_ON_IO，总耗时更长，CPU利用率大幅下降。
## Q5
- Prediction / 预测:
Total Time = 8 ticks，CPU利用率 = 6/8 =75%
- Reasoning / 理由:
`-S SWITCH_ON_IO`，IO发生就切换。
PID0执行1tick发起IO，阻塞5tick，CPU切换给PID1跑4条CPU。
IO完成后切回PID0执行io_done(1tick)。总时间8tick。
CPU总共工作6tick，利用率75%。
- Verified result / 验证结果:
Total Time = 7 ticks，CPU利用率 = 85.71%
- Analysis / 分析:
预测结果与模拟器运行结果不一致。原预测总时间8ticks，实际为7ticks；原预测CPU利用率75%，实际85.71%。开启SWITCH_ON_IO，当PID0发起IO时立刻切换至PID1执行CPU任务，IO操作与CPU计算并行，有效利用CPU，缩短总运行时间并提高CPU利用率。
## Q6
- Prediction / 预测：
Total Time = 10 ticks，CPU利用率 = 8 / 10 = 80.00%
- Reasoning / 理由：
4个进程：PID0/PID1/PID2/PID3，每个进程包含1条CPU指令 + 1次IO。
SWITCH_ON_IO：进程发起IO时，CPU立刻切换到下一个就绪进程；IO_RUN_LATER：IO完成后不会抢占CPU，等待调度。
4个进程依次执行CPU指令并发起IO，4个IO可以并行执行；IO全部完成后，再依次执行收尾，总耗时10 ticks。
- Verified result / 验证结果：
Total Time = 10 ticks，CPU利用率 = 80.00%
- Analysis / 分析：
最开始我错误预测总时间为13 ticks，和模拟器运行结果不一致。原因是一开始误以为IO是串行执行，实际上4个IO可以并行，大量重叠等待时间，因此总时间更短，CPU利用率更高。
## Q7
- Prediction / 预测：
Total Time = 10 ticks，CPU利用率 = 8 / 10 = 80.00%
- Reasoning / 理由：
4个进程：PID0/PID1/PID2/PID3，每个进程包含1条CPU指令 + 1次IO。
SWITCH_ON_IO：进程发起IO时，CPU立刻切换到下一个就绪进程；IO_RUN_IMMEDIATE：IO完成后立刻抢占CPU。
4个进程几乎同时发起IO，4个IO并行执行并在同一时刻完成，没有出现抢占冲突，IO结束后依次执行收尾，总耗时10 ticks。
- Verified result / 验证结果：
Total Time = 10 ticks，CPU利用率 = 80.00%
- Analysis / 分析：
在本案例中4个IO同时完成，因此IO_RUN_IMMEDIATE和IO_RUN_LATER的最终运行结果完全一致。
若IO完成时间错开，IO_RUN_IMMEDIATE的抢占机制会改变时序，总运行时间可能发生变化。
## Q8
- Prediction / 预测：
结果随随机种子变化，不同seed总时间、CPU利用率不一样。
- Reasoning / 理由：
`-l 1:3` 代表30%概率生成CPU指令，70%概率生成IO指令，每次运行都会随机生成不同指令序列。
SWITCH_ON_IO：进程发起IO时，CPU切换到其他就绪进程。
由于指令序列随机，每次模拟器执行的行为不同，无法给出固定的ticks数值。
- Verified result / 验证结果：
本次运行：Total Time = 7 ticks，CPU利用率 = 2 / 7 = 28.57%，IO Busy = 5（71.43%）
- Analysis / 分析：
随机指令序列会改变进程CPU与IO的交替顺序，因此每次运行得到的总耗时、CPU利用率都会不同。本次示例中IO占比很高，CPU空闲时间较多，CPU利用率偏低。