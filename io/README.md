
##关于延迟显示

为了跟踪系统的延迟, 我们一般是通过 iostat 记录不同时间点的延迟. 之后画出延迟图,

x 轴为时间, y 轴为延迟.

但是, 这里隐藏了一个问题, 不管是一个系统还是一个进程, 每个时刻的 io
不可能是一个, 一个时间段(1min, 1s, 1ms)可能有多个 IO 操作同时在进行.
每个 IO 的延迟是不一样的, 如何刻画这中延迟, 显然通过一个二维图象是
不行的. 因此, 需要增加一个维度, 即 z 表示在每个时间点, 不同延迟的分别
情况. 比如， 在时间点 t2-t1 时间内, 当前有 n 个 IO 操作, 最大延迟是 100 ms,
最小延迟是 1 ms, 那么, 这 n 个 IO 操作主要分布在哪个延迟段内, 比如, 其中
1% 分布在 100ms, 80% 分别在 10ms, 这个比例通过 z 来显示, 比如通过颜色
的深度. 那么, 这样就可以非常清晰地刻画一个系统或一个进程的延迟特性.

为了达到以上目的, SVG 正好满足需求.

SVG 图解释

1. x 轴表示时间.
2. y 轴表示延迟时间.
3. z 轴表示某个延迟的数量. 通过颜色深度来表示, 颜色越深表示该延迟时间点的操作越多.

鼠标悬浮在图的任何一个点, 就会显示当前时间, 该点在当前时刻 IO 占的比例.


##关于延迟采集

目前主流的工具, 如果 iostat, strace 对于跟踪一个大量 IO 的系统, 要不是
粒度过于粗, 要不就是负载过高, 解决办法, 在 Linux 系统下, 目前可用的工具
只有 perf 和 systemtap. 当然等 eBPF 成熟, 应该是更好的选择.


##诊断延迟问题步骤

1. 收集延迟数据
2. 画出延迟 SVG 图, 找到延迟的关注点.
3. 对延迟数据进行分析, 找到对应的进程及操作
4. 找到对策, 解决延迟问题


##参考
http://www.brendangregg.com/HeatMaps/latency.html