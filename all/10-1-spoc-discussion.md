# IO设备(lec 23) spoc 思考题

## 个人思考题
### IO特点 
 1. 字符设备的特点是什么？
 1. 块设备的特点是什么？
 1. 网络设备的特点是什么？
 1. 阻塞I/O、非阻塞I/O和异步I/O这三种I/O方式有什么区别？

### I/O结构
 - I/O访问的手段有哪些？
 - 请描述I/O请求到完成的整个执行过程
 - CPU与设备通信的手段有哪些？

> 显式的IO指令，如x86的in, out； 或者是memory读写方式，即把device的寄存器，内存等映射到物理内存中 

### IO数据传输
 - IO数据传输有哪几种？
 - 轮询方式的特点是什么？
 - 中断方式的特点是什么？
 - DMA方式的特点是什么？

### 磁盘调度
 - 请简要阐述磁盘的工作过程
 - 请用一表达式（包括寻道时间，旋转延迟，传输时间）描述磁盘I/O传输时间
 - 请说明磁盘调度算法的评价指标
 - FIFO磁盘调度算法的特点是什么?
 - 最短服务时间优先(SSTF)磁盘调度算法的特点是什么?
 - 扫描(SCAN)磁盘调度算法的特点是什么?
 - 循环扫描(C-SCAN)磁盘调度算法的特点是什么?
 - C-LOOK磁盘调度算法的特点是什么?
 - N步扫描(N-step-SCAN)磁盘调度算法的特点是什么?
 - 双队列扫描(FSCAN)磁盘调度算法的特点是什么?

### 磁盘缓存
 - 磁盘缓存的作用是什么？
 - 请描述单缓存(Single Buffer Cache)的工作原理
 - 请描述双缓存(Double Buffer Cache)的工作原理
 - 请描述访问频率置换算法(Frequency-based Replacement)的基本原理

## 小组思考题
 - (spoc)完成磁盘访问与磁盘寻道算法的作业，具体帮助和要求信息请看[disksim指导信息
 ](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab8/disksim-homework.md)和[disksim参考代码](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab8/disksim-homework.py)

---

*1.* 问题1

采用FIFO

磁盘请求序列及对应的访问时间：

[0] 旋转延迟165 传输时间30 寻道时间0 共195

[6] 传输时间30 寻道时间0 由于必须从5~6中间开始读，而一开始磁头在6处，所以需要旋转345，共计375

[30] 传输时间30 寻道时间80 旋转延迟345-80 共计375

[7,30,8] 采用FIFO，读7，传输30，寻道0，旋转15； 读30，传输30，寻道80，旋转300-80； 读8，传输30，寻道80，旋转，当转到外磁道时已经到达9~10中间，要重新旋转310； 总共为795

[10,11,12,13,24,1] 读10和11，寻道0，旋转105，连续传输60； 读12和13，转到第二磁道，寻道40，但是已经到达12~13中间，需要旋转320，连续读取60； 读24，寻道40，传输30，旋转300-40=260； 读1，寻道80，传输30，此持磁头在3~4之间，要重新旋转280   总共1305

*2.* 问题2

采用SSTF

[10,11,12,13,24,1] 先读取最外侧磁道上10,11,1。连续读取10,11，旋转105，传输60，读取1，旋转30，传输30，则计225； 然后读取12,13. 寻道40，连续传输60，旋转260，计360； 最后读取24，传输30，寻道40，旋转300-40=260，计330. 共计915

*3.* 问题3

采用SCAN和C-SCAN
[10,11,12,13,24,1] 在该测例中，采用SCAN和C-SCAN的访问时间都是915，和SSTF相同，因为测例是固定的，在访问过程中没有新的访问请求加入




