输入命令`cat /proc/cpuinfo` 查看physical id有几个，上述结果显示只有0，所以只有一个物理cpu；查看processor有几个，上述结果显示有0和1两个，所以有两个逻辑cpu。

### (一)概念
- ① 物理CPU
实际Server中插槽上的CPU个数
物理cpu数量，可以数不重复的 physical id 有几个
- ② 逻辑CPU 
 `/proc/cpuinfo` 用来存储cpu硬件信息的
信息内容分别列出了processor 0 –processor n 的规格。这里需要注意，n是逻辑cpu数
一般情况，我们认为一颗cpu可以有多核，加上intel的超线程技术(HT), 可以在逻辑上再分一倍数量的cpu core出来
逻辑CPU数量=物理cpu数量 x cpu cores 这个规格值 x 2(如果支持并开启ht)    
备注一下：Linux下top查看的CPU也是逻辑CPU个数
- ③ CPU核数
一块CPU上面能处理数据的芯片组的数量、比如现在的i5 760,是双核心四线程的CPU、而 i5 2250 是四核心四线程的CPU
一般来说，物理CPU个数×每颗核数就应该等于逻辑CPU的个数，如果不相等的话，则表示服务器的CPU支持超线程技术 
### ㈡ 查看CPU信息
当我们 `cat /proc/cpuinfo` 时、
具有相同core id的CPU是同一个core的超线程
具有相同physical id的CPU是同一个CPU封装的线程或核心
### ㈢ 下面举例说明
- ① 查看物理CPU的个数
    `cat /proc/cpuinfo |grep "physical id"|sort |uniq|wc -l`  
    2  
- ② 查看逻辑CPU的个数
    `cat /proc/cpuinfo |grep "processor"|wc -l`  
    24  
- ③ 查看CPU是几核
    `cat /proc/cpuinfo |grep "cores"|uniq`  
    6   
我这里应该是2个Cpu,每个Cpu有6个core,应该是Intel的U,支持超线程,所以显示24 