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
- Analysis / 分析:
## Q2
- Prediction / 预测:
Total Time = 12 ticks，CPU利用率 = 5/12 ≈ 41.67%
- Reasoning / 理由:
PID0有4条CPU指令，连续执行4tick完成；之后运行PID1，PID1是1条IO指令。
PID1：RUN:io(1tick) → IO阻塞5tick → IO完成RUN:io_done(1tick)，PID1一共7tick。
总时间 = 4 + 7 = 12 tick。
CPU有效工作tick：4+2=5 tick，CPU利用率5/12。
- Verified result / 验证结果:
- Analysis / 分析:
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
- Analysis / 分析:
## Q4
- Prediction / 预测:
Total Time = 12 ticks，CPU利用率 = 5/12 ≈ 41.67%
- Reasoning / 理由:
参数`-S SWITCH_ON_END`，只有进程全部结束才切换，不会在IO阻塞时切换。
PID0（1条IO指令）从头到尾跑完7tick（1+5+1），结束后才运行PID1的4条CPU指令（4tick）。
总时间=7+4=11 tick。CPU工作tick=2+4=6，利用率6/11≈54.55%。
- Verified result / 验证结果:
- Analysis / 分析:
## Q5
- Prediction / 预测:
Total Time = 8 ticks，CPU利用率 = 6/8 =75%
- Reasoning / 理由:
`-S SWITCH_ON_IO`，IO发生就切换。
PID0执行1tick发起IO，阻塞5tick，CPU切换给PID1跑4条CPU。
IO完成后切回PID0执行io_done(1tick)。总时间8tick。
CPU总共工作6tick，利用率75%。
- Verified result / 验证结果:
- Analysis / 分析:
## Q6
- Prediction / 预测:
Total Time = 13 ticks，CPU利用率 = 7/13 ≈53.85%
- Reasoning / 理由:
4个进程：PID0/PID1/PID2/PID3，各1条IO指令。SWITCH_ON_IO，IO结束后IO_RUN_LATER。
一个进程发起IO后CPU切换下一个进程，4个进程轮流发起IO，IO串行执行。
每个进程消耗2个CPU tick，IO阻塞5tick。
总时间：1（第一个进程发起IO）+5（IO等待）+4（剩下3个进程发起IO）+5+1 =13tick。
CPU总工作tick=7。
- Verified result / 验证结果:
- Analysis / 分析:
## Q7
- Prediction / 预测:
Total Time = 8 ticks，CPU利用率 = 7/8 =87.5%
- Reasoning / 理由:
`-I IO_RUN_IMMEDIATE`，IO一旦完成立刻继续运行该进程，不排队。
4个进程，SWITCH_ON_IO，IO完成立即运行。IO可以并行重叠，大量节约时间。
总耗时8tick，CPU工作7tick，利用率87.5%。
- Verified result / 验证结果:
- Analysis / 分析:
## Q8
- Prediction / 预测:
Prediction：结果随随机种子变化，不同seed总时间、CPU利用率不一样。
- Reasoning / 理由:
`-l 3:0`代表30%概率CPU指令，70%IO指令，每次运行随机生成指令序列。每次运行的进程行为不一样，无法给出固定总tick。
- Verified result / 验证结果:
- Analysis / 分析: