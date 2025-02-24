缓冲区管理 引入缓冲区的目的? - 缓和CPU与I/O设备之间速度不匹配的⽭盾【主要⽬的】
- 提⾼CPU与I/O设备之间的并⾏性
- 减少对CPU的中断频率,放宽了CPU对中断响应时间的限制 缓冲区实现方法? - 采⽤硬件缓冲器
- 采⽤缓冲区(位于内存区域)
缓冲区主要考虑的问题 - 缓冲区是⼀种临界资源,使⽤时要考虑同步与互斥的问题 需要使用缓冲技术的I/O操作 - 使⽤⿏标
- 多任务操作系统下的磁带驱动器(假设没有设备预分配) - 包含⽤户⽂件的磁盘驱动器 - 使⽤存储器映射I/O,直接和总线相连的图形卡 缓冲技术分类及计算
- T时间:数据从磁盘--->缓冲区
- M时间:数据从缓冲区---->⽤户 - C时间:CPU对⼀块数据处理的时间 单缓冲 Max(C,	T)	+	M
T > C时,处理⼀块数据平均需要(T+M) T < C时,处理⼀块数据平均需要(C+M)
双缓冲 Max(T,	C+M)
T > C+M时,平均处理⼀个数据块需要时间T T < C+M时,平均处理⼀个数据块需要时间(C+M)
循环缓冲

- 将多个⼤⼩相等的缓冲区连接成⼀个循环队列。

- 下图中橙⾊表示已充满数据的缓冲区,绿⾊表示空缓冲区。

当需要向缓冲区中冲⼊数据时,只要找到in指针指向的空缓 冲区,向其中冲⼊数据,然后再把in指针指向下⼀个空缓冲 区。

•
当需要取出满缓冲区的内容时,找到out指针执⾏的满缓冲, 读完数据后,将指针指向下⼀个满缓冲区。

•
缓冲池【存放在主存中】【最好的方法】

- 缓冲池由系统中共⽤的缓冲区组成 缓冲区按使⽤状况可以分为
- 空缓冲队列 - 输⼊队列:存储的是从设备发送给内存的数据
- 输出队列:存储的是从内存发送给设备的数据
•

根据⼀个缓冲区在实际运算中扮演的功能不同分为四种⼯作 缓冲区
- ⽤于收容输⼊数据的⼯作缓冲区(hin)
- ⽤于提取输⼊数据的⼯作缓冲区(sin) - ⽤于收容输出数据的⼯作缓冲区(hout) - ⽤于提取输出数据的⼯作缓冲区(sout)
•

设备分配
- 设备分配指根据⽤户的I/O请求分配所需的设备 - 计算机系统为每台设备确定⼀个设备的绝对号以便区分和识别设备 设备分配的策略 分配的原则 - 设备固有属性决定了设备的使⽤⽅式(充分发挥设备的使⽤效率,尽可能让设备忙碌)
- 设备独⽴性可以提⾼设备分配的灵活性和设备的利⽤率(设备独⽴性是指⽤户使⽤设备的透明性,即⽤户程序与实际使⽤的物理设备⽆关)
- 设备安全性可以保证分配设备时不会导致永久阻塞(要避免造成进程死锁)
设备分配的方式 - 静态分配---->对独占设备的分配---->在作业执行前分配---->不会出现死锁---->设备的使用率低
- 动态分配--->在进程执行中分配,通过系统调用发送请求,根据具体策略分配设备---->分配算法不好时会出现死锁---->设备利用率高 设备分配算法 先请求先分配,优先级高者分配。。。。(类比调度算法)
设备分配的安全性 定义 - 设备分配安全性是指设备分配中应防⽌发⽣进程死锁 安全分配方式 - 为进程分配⼀个设备后就将该进程阻塞,本次I/O完成后才将进程唤醒
- 安全分配⽅式在⼀个时间段内每个进程只能使⽤⼀个设备。 - 优点:破坏了请求等待条件,不会死锁。 - 缺点:对于⼀个进程来说,CPU和I/O设备只能串⾏⼯作,系统资源利⽤率低。

不安全分配方式 - 进程发出I/O请求后,系统为其分配I/O设备,进程可继续执⾏,之后还可以发出新的I/O请求,只有某个I/O请求得不到满⾜时才将进程阻塞
- 不安全分配⽅式⼀个进程可以同时使⽤多个设备 - 优点:进程的计算任务和I/O任务可以并⾏处理,使进程推进。 - 缺点:有可能发⽣死锁。

设备分配依据的数据结构 设备,控制器,通道之间的关系→
设备控制表(DCT) - 系统为每个设备配置⼀张DCT
- ⽤于记录设备情况 控制器控制表(COCT) - 每个设备控制器都会对应⼀张COCT
- 操作系统会根据COCT的信息对控制器进⾏操作管理 通道控制表(CHCT) - 每个通道都会对应⼀张CHCT
- 操作系统会根据CHCT的信息对通道进⾏操作管理 系统设备表(SDT) - 记录了系统中全部设备的情况,每个设备对应⼀个表⽬。

逻辑设备名到物理设备名的映射 目的 - 为了提⾼设备分配的灵活性和利⽤率---->分别实现I/O重定向---->所有引⼊了设备独⽴性 定义 - 设备独⽴性:指应⽤程序独⽴于具体使⽤的物理设备/用户在编程序时使用的设备与实际设备无关
- 逻辑设备表LUT:将逻辑设备名映射为物理设备名 - LUT表项包括:逻辑设备名,物理设备名,设备驱动程序⼊⼝地址 分类 - 两种⽅式设置逻辑设备表
- 1、整个系统只设置⼀张LUT---->适用于单用户系统
- 2、每个⽤户设置⼀张LUT---->用户登陆时,系统为⽤户建⽴⼀个进程,同时建⽴⼀张LUT
好处 - 方便用户编程
- 使程序运行不受具体机器环境的限制
- 便于程序移植 SPOOLing技术(假脱机技术)
脱机技术和假脱机技术对⽐
脱机技术 - 引⼊⽬的:缓解设备与CPU的速度⽭盾,实现预输⼊,缓输出
- 组成:外围机+更⾼速的设备--磁带
- 输⼊时:在外围控制机的控制下,慢速的输⼊设备先将数据输⼊到更快速的磁带上,之后主机就可以快速的从磁带上读⼊数据,从⽽缓解了速度⽭盾
- 输出时:先将数据输出到快速的磁带上,之后通过外围控制机控制磁带将数据依次的输出到慢速的设备上。

假脱机技术 - ⼜叫SPOOLing技术,⽤软件的⽅式模拟假脱机技术,不需要外围机,是⼀项将独占设备改造成共享设备的软件技术 SPOOLing系统的运⾏过程 输入进程和输出进程 预输⼊程序管理 - ⽤于模拟脱机技术中的外围控制机
- 需要和⽤户进程并发执⾏才可以模拟脱机技术,所以要实现SPOOLing技术需要多道程序技术的⽀持。

输入井和输出井 井输⼊程序管理 - ⽤于模拟脱机技术中的磁带 输入缓冲区和输出缓冲区 缓输出程序管理 - 输⼊缓冲区⽤于暂存从输⼊设备输⼊的数据,之后再转存到输⼊井中
- 输出缓冲区⽤于暂存从输出井传送的数据,之后再传送到输出设备上 SPOOLing系统的优特点 1、加快了作业执⾏的速度,提⾼了IO速度,将对低速IO设备执⾏的IO操作演变为对磁盘缓冲区中数据的存取 2、使独占设备变为共享设备,且没有为任何进程分配设备 3、提⾼了独占设备的利⽤率,缓和了CPU和低速IO设备之间的速度不匹配的⽭盾 4、实现了虚拟设备功能,对每个进程⽽⾔,都认为⾃⼰独占了⼀个设备 5、以空间换时间,需要磁盘空间(输⼊输出井)和内存空间(输⼊输出缓冲区)
实例:共享打印机的实现
- 打印机是独占设备,只允许各个进程串⾏使⽤设备,⼀段时间内只能满⾜⼀个进程的请求。

实现过程
- 当多个⽤户提出输出打印的请求时,系统会答应它们的请求,但是并不会真正把打印机分配给它们,⽽是有假脱机管理进程为每个进程做两件事
- (1) 在磁盘输出井中为进程申请⼀个空闲磁盘块,之后假脱机管理进程会将进程要打印的数据送⼊刚申请的空闲磁盘块中【打印结果⾸先送⼊到磁盘固定区域】 - (2) 为⽤户进程申请⼀张空⽩的打印请求表,并将⽤户的打印请求填⼊表中(⽤来说明⽤户的打印数据存放位置等信息),再将该表挂到假脱机⽂件队列上
- 当打印机空闲时,输出进程会从⽂件队列的队头取出⼀张打印请求表,并根据表中的要求打印数据从输出井传送到输出缓冲区,再输出到打印机打印
- 虽然系统中只有⼀台打印机,但每个进程提出打印请求时,系统都会为在输出井中为其分配⼀个存储区(相当于⼀个逻辑设备) - 使每个⽤户进程都觉得⾃⼰在独占⼀台打印机,从⽽实现对打印机的共享 磁盘的基本概念 示例图 - ⼀个盘⽚的结构,盘⽚中的每⼀层分为多个磁道,每个磁道分多个扇区,每个扇区是 512 字节
- 那多个具有相同编号的磁道形成⼀个圆柱,称之为磁盘的柱⾯
•
磁盘、磁道、扇区,簇 - 磁盘的表⾯是由⼀些磁性物质组成,可以⽤这些磁性物理记录⼆进制数据
- 磁盘表⾯被划分⻓的⼀个个磁道
- ⼀个磁道⼜被划分为⼀个个扇区,每个扇区就是⼀个"磁盘块" - 每个扇区的数据量相同(如1KB)
- 簇:⼀组扇区的意思 盘面、柱面 - 磁盘是由多个盘⽚摞起来的,每个盘⽚有两个盘⾯。每个盘⾯都对应⼀个磁头。所有的磁头都连在同⼀个磁臂上。

- 磁臂可以沿着盘⾯作径向运动,从⽽带动磁头到达不同的磁道来对不同扇区的读写操作。

- 所有盘⾯中的相对位置相同的磁道组成了柱⾯。例如上图中所有盘⾯的最外侧磁道可以组成⼀个柱⾯。

在磁盘中读/写数据 - 在磁盘中读写数据,需要借助磁头
- step1:将磁头移动到想要读/写的扇区所在的磁道 - step2:磁盘会转动,让⽬标扇区从磁头下⾯划过,才能完成对扇区的读/写操作 磁盘的物理地址 - 使⽤(柱⾯号,盘⾯号,扇区号)来定位任意⼀个磁盘块
- ⽂件数据存放在外存中的⼏号块,这⾥的块号就可以转换为(柱⾯号,盘⾯号,扇区号)的地址形式。

- 根据物理地址读取⼀个"块"
① 根据柱⾯号移动磁臂,让磁头指向指定柱⾯。 ② 激活指定盘⾯对应的磁头。 ③ 磁盘旋转的过程,指定的扇区会从磁头下⾯划过。这样就完成了对指定扇区的读写。

磁盘的分类 根据磁头是否可以活动划分 - 活动头磁盘
- 固定头磁盘 根据盘⾯是否可以更换划分 - 可换磁盘
- 固定盘磁盘 磁盘的管理 格式化的过程 第一步 低级/物理格式化 - 在磁盘可以存储数据之前,将它分为扇区
- 以便磁盘控制器能够进⾏读写操作 第二步 分区 - 将磁盘分为由一个或多个柱面组成的分区(如C区,D区)
- 每个分区的起始扇区和大小都记在磁盘主引导记录的分区表中 第三步 高级/逻辑格式化 - 创建文件系统的根目录
- 对保存空闲磁盘块信息的数据结构进行初始化 其他相关概念 引导块 - 计算机启动时需要运行一个初始化程序(自举程序),该程序存放在ROM中
- 启动/系统磁盘:磁盘具有启动分区的磁盘 坏块 - 对坏块的处理实质上就是用某种机制使系统不去使用坏块 固态硬盘SSD
固态硬盘的特性 - 一个SSD由一个或多个闪存芯片和闪存翻译层组成
- SSD由半导体存储器构成,没有移动的部件,因而随机访问时间比机械磁盘要快很多 - SSD是基于闪存的存储技术 - SSD随机读写性能明显高于磁盘,优势主要体现在随机存取上
- SSD随机写比较慢,但不比常规硬盘差 磨损均衡 - 为了弥补SSD的寿命缺陷,引入了磨损均衡
- 动态磨损均衡
- 静态磨损均衡(更先进)
磁盘调度算法
- 磁盘调度算法的⽬的:为了提⾼磁盘的访问性能,⼀般是通过优化磁盘的访问请求顺序来做到的 - 寻道的时间是磁盘访问最耗时的部分,如果请求顺序优化的得当,可以节省⼀些不必要的寻道时间,从⽽提⾼磁盘的访问性能 - 绝⼤数OS为改善磁盘访问时间,以簇(⼀组块)为单位进⾏空间划分
⼀次磁盘读/写操作需要的时间 寻找时间(寻道时间)Ts
【一般最长】

- 在读/写数据前,需要将磁头移动到指定磁道所花费的时间 - 寻道时间分两步:
(1) 启动磁头臂消耗的时间s (2) 移动磁头消耗的时间:假设磁头匀速移动,每跨越⼀个磁道消耗时间为m,共跨越n条磁道
- T! = s + m×n 延迟时间TR - 通过旋转磁盘,使磁头定位到⽬标扇区所需要的时间 T" = (
1 2
+ × (
1 r
+ = 12
•
- 1/r就是转⼀圈所需的时间。找到⽬标扇区平均需要转半圈,因此再乘以1/2 传输时间TR - 从磁盘读出或向磁盘中写⼊数据所经历的时间
- 假设磁盘转速为r,此次读/写的字节数为b,每个磁道上的字节数为N
T" = (
b N
+ × (
1 r
+ = b
•
- 每个磁道可存N字节数据,因此b字节数据需要b/N个磁道才能存储。⽽读/写⼀个磁道所需的时间刚好是转⼀圈的时间1/r。

总的平均时间 - Ta = Ts + 1/2r + b/(rN)
- ⽆法通过操作系统优化延迟时间和传输时间。所以只能优化寻找时间。

磁盘调度算法 假设有⼀个请求序列,每个数字代表磁道的位置:98,183,37,122,14,124,65,67,初始磁头当前的位置是在第 53 磁道 先来先服务算法(*FCFS*) 最短寻找时间优先(*SSTF*) 扫描/电梯算法(SCAN) *C-SCAN*算法 思想 先到来的请求,先被服务 优先选择从当前磁头位置所需寻道时间最短的请求 磁头在⼀个⽅向上移动,访问所有未完成的请求 直到磁头到达该⽅向上的最后的磁道,才调换⽅向
- 只有磁头朝某个特定⽅向移动时,才处理磁道访问请求
- ⽽返回时直接快速移动⾄最靠边缘的磁道,也就是复位磁头
- 这个过程是很快的,并且返回中途不处理任何请求 - 特点:磁道只响应⼀个⽅向上的请求 优点 - 公平
- 请求访问的磁道⽐较集中的话算法性能还算可以
- 是贪⼼算法的思想,只是选择眼前最优,但是总体未必最优
- ⽐FCFS效果好
- 性能较好,寻道时间较短
- 不会产⽣饥饿现象
- 对于各个位置磁道响应频率很平均 缺点 - 这种算法,⽐较简单粗暴
- 如果⼤量进程竞争使⽤磁盘,请求访问的磁道可能会很分散
- 算法在性能上就会显得很差,因为寻道时间过⻓
- 存在饥饿现象 - 产⽣饥饿的原因是磁头在⼀⼩块区域来回移动
- 中间部分的磁道会⽐较占便宜 - 中间部分相⽐其他部分响应的频率会⽐较多
- 也就是说每个磁道的响应频率存在差异
- 相⽐于SCAN算法,平均寻道时间更⻓。

处理顺序 - 98,183,37,122,14,124,65,67 - 65,67,37,14,98,122,124,183 - 假设扫描调度算先朝磁道号减少的⽅向移动
- 磁头先响应左边的请求
- 直到到达最左端后,才开始反向移动,响应右边的请求
- 37,14,0,65,67,98,122,124,183
- 假设循环扫描调度算先朝磁道增加的⽅向移动
- 磁头先响应了右边的请求
- 直到碰到了最右端的磁道 199,就⽴即回到磁盘的开始处
- 但这个返回的途中是不响应任何请求的
- 直到到达最开始的磁道后,才继续顺序响应右边的请求
- 65,67,98,122,124,183,199,0,14,37 示例图 设备控制器(I/O接⼝)
1.设备控制器主要功能
- 接受和识别CPU发来的命令
- 数据交换(设备和控制器的数据交换+控制器和主存数据交换)
- 标识和报告设备的状态,以供CPU处理 - 地址识别;数据缓冲;差错控制 - 设备控制器不属于操作系统范畴,它是属于硬件 2.设备控制器的组成 1.设备控制器与CPU的接口 - ⽤于实现CPU与控制器之间的通信
- CPU通过控制线发出命令
- 通过地址线指明要操作的设备 - 通过数据线来输⼊输出数据 2.设备控制器与设备的接口 - ⽤于实现控制器和设备之间的通信 3.I/O逻辑 - I/O逻辑⽤于负责接收和识别CPU的各种命名(如地址译码)
- 根据CPU的命令对相应的设备发送命令 - ⽤于实现设备控制功能 组成图 3.设备控制器有三种寄存器(设备控制器中可被CPU直接访问的寄存器,也叫I/O端⼝)
数据寄存器 CPU 向 I/O 设备写⼊需要传输的数据 命令/控制寄存器 - ⽐如要打印的内容是「Hello」,CPU 就要先发送⼀个 H 字符给到对应的 I/O 设备
- CPU 发送⼀个命令,告诉 I/O 设备,要进⾏输⼊/输出操作,于是就会交给 I/O 设备去⼯作
- 任务完成后,会把状态寄存器⾥⾯的状态标记为完成 状态寄存器 - ⽬的是告诉 CPU 现在已经在⼯作或⼯作已经完成
- 如果已经在⼯作状态,CPU 再发送数据或者命令过来,都是没有⽤的
- 直到前⾯的⼯作已经完成,状态寄存标记成已完成,CPU 才能发送下⼀个字符和命令 4.CPU与I/O端⼝的通信⽅式 端口 I/O(独立编制) - 每个控制寄存器被分配⼀个 I/O 端⼝
- 可以通过特殊的汇编指令操作这些寄存器 - ⽐如 in/out 类似的指令 内存映射 I/O(统一编制) - 将所有控制寄存器映射到内存空间中
- 这样就可以像读写内存⼀样读写数据缓冲区 设备控制⽅式(I/O控制⽅式)
- 外围设备和内存之间的控制⽅式 - ⽤于控制设备和内存或CPU之间的数据传送 1.程序直接控制⽅式
- ⼀种轮待询等的⽅法,让 CPU ⼀直查寄存器的状态,直到状态标记为完成 2.中断驱动程序
- 有⼀个硬件的中断控制器,当设备完成任务后触发中断到中断控制器,中断控制器就通知 CPU,例如键盘
⼀个中断产⽣了,CPU 需要停下当前⼿⾥的事情来处理中断
- 软中断,例如代码调⽤ INT 指令触发
- 硬件中断,就是硬件通过中断控制器触发的
•

- 中断的⽅式对于频繁读写数据的磁盘,并不友好,这样 CPU 容易经常被打断,会占⽤ CPU ⼤量的时间,使⽤DMA解决 3.DMA⽅式(Direct Memory Access)
概念 - 可以使得设备在 CPU 不参与的情况下,能够⾃⾏完成把设备 I/O 数据放⼊到内存
- 要实现 DMA 功能要有 DMA 控制器硬件的⽀持 工作方式 - CPU 需对 DMA 控制器下发指令,告诉它想读取多少数据,读完的数据放在内存的某个地⽅就可以了;
- 接下来,DMA 控制器会向磁盘控制器发出指令,通知它从磁盘读数据到其内部的缓冲区中,接着磁盘控制器将缓冲区的数据传输到内存;
- 当磁盘控制器把数据传输到内存的操作完成后,磁盘控制器在总线上发出⼀个确认成功的信号到 DMA 控制器; - DMA 控制器收到信号后,DMA 控制器发中断通知 CPU 指令完成,CPU 就可以直接⽤内存⾥⾯现成的数据了; 工作流程 1. 初始化DMA控制器并启动磁盘 2. 从磁盘传输⼀块数据到内存缓冲区 3. DMA控制器发送中断请求 4. 执行DMA结束中断服务程序 特性 - 整个过程仅仅在开始和结束时需要CPU⼲预
- 每个DMA控制器对应⼀台设备与内存传递数据
- DMA⽅式主要⽤于块设备,磁盘是典型的块设备 示例图 4.通道控制⽅式 通道方式过程 - 设置通道后,CPU只需向通道发送⼀条I/O指令,通道在收到该指令后,便从内存中取出本次要执⾏的通道程序,然后执⾏该通道程序
- 仅当通道完成规定的I/O任务后,才向CPU发出中断信号 特性 - CPU、通道、I/O设备可并⾏⼯作,资源利⽤率很⾼
- 实现复杂,需要专⻔的硬件⽀持
- ⼀个通道可以控制多台设备与内存的数据交换 分类 字节多路通道:通常包含许多⾮分配型字通道,数量可以达到⼏⼗到⼏百个,每个通道连接⼀台IO设备,并控制该设备的IO操作 字节多路通道常⽤于连接⼤量的低速或中速I/O设备 示例图 四种⽅式的⽐较 I/O管理要完成哪些功能?

状态跟踪 要能实时掌握外部设备的状态 设备存取 要实现对设备的存取操作 设备分配 在多⽤户环境下,负责设备的分配与回收 设备控制 包括设备的驱动,完成和故障的中断处理 I/O软件层次结构 四种结构 用户层 I/O软件 - ⽤户通过统⼀的接⼝发送命令
- 如发送read命令 设备独立性软件 - 对⽤户的命令进⾏解析
- 如解析read命令 设备驱动程序 - 负责执⾏OS发出的I/O命令,针对不同的硬件将命令解析为指令
- 若更换物理设备,只需要修改设备驱动程序,不需要修改应⽤程序
- 如将解析好的read命令转换为指令
- 如计算磁盘的柱⾯号,磁头号,扇区号 中断处理程序 - 中断正在运⾏的进程,转⽽执⾏⽤户命令
- 如中断当前进程,执⾏相关指令 层次结构的特点
- 每层都是利⽤其下层提供的服务,完成输⼊/输出功能中的某些⼦功能
- 屏蔽⼦功能实现的细节,向⾼层提供服务
- 仅最底层才涉及硬件的具体特性 应⽤程序I/O接⼝分类 字符设备接口 - 字符设备是指数据的存取和传输都是以字节为单位的设备
- 字符设备都属于独占设备 块设备接口 - 块设备是指数据的存取和传输都是以数据块为单位的设备,如磁盘
- 磁盘设备的I/O常使⽤DMA⽅式 网络设备接口 - 许多OS提供的⽹络I/O接⼝为⽹络套接字接⼝
阻塞/非阻塞 I/O - ⼤多数OS提供的 I/O接⼝都是采⽤阻塞 I/O
设备的分类 按信息交换的单位分类 块设备
- 传输速度较⾼+可寻址+可随机读/写任⼀块
- 属于有结构设备;如磁盘,硬盘,USB
字符设备

- 传输速率低+不可寻址+时常采⽤中断I/O⽅式
- 属于⽆结构类型;如交互式终端机,打印机,⿏标 按传输速度分类 低速设备
- 键盘,⿏标 中速设备

- 激光打印机 高速设备

- 磁盘机,光盘机 按设备特性分类 独占设备 - ⼀个时段只能分配给⼀个进程
- 所有字符设备都是独占设备 - 速度慢,利⽤率低
- 如输⼊机、打印机、磁带机等 共享设备 - ⼀段时间内允许多个进程同时访问的设备
- 共享设备必须是可寻址和可随机访问的设备
- 软硬盘、磁盘、光盘等块设备都是共享设备 虚拟设备 - 通过虚拟软件技术将独占设备改造成共享设备
- 如通过 SPOOLing技术将⼀台打印机虚拟成多台打印机 - 其实质上还是独占设备 按访问顺序 可以顺序访问的
- 磁带 可以随机访问的(直接访问)
- 光盘,磁盘,U盘 两者都可以的 - 光盘,磁盘,磁盘 第五章 I/O管理 2022年8月22日 星期一 10:24