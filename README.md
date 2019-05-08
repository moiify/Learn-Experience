- ### USmart函数的使用  usmart.c 三个函数 +一个串口接收函数

串口接收函数需要配合原子家自己的接收协议（回车那个） 

一个函数用来初始化中断用来实现串口数据处理 。 

另外两个函数一个是用来算程序运行时间的（根据所使用的MUC时钟来修改）

usmart_scan 两个参数 一个是串口数据 一个是串口接收状态 

具体修改在 usmart_config.中加头文件加按格式写好的函数名

list 打印所有函数

id  获取入口地址 

help  帮助信息

hex dec 十进制十六进制转换

runtime 1/0  开启或关闭 函数时间计时功能

带有函数参数的函数的调用  利用id获取函数参数地址   使用方法函数（地址，参数）




-  ### 高级定时器 1&8

8个通道

16位定时器  由一个可编程预分频器驱动（16位）

脉冲宽度捕获 。产生波形 （N多功能 PWM 死区时间互补PWM 和H桥和半H桥有关  上下桥臂不是同时输出 造成差异）

周期由两个分频器控制。 高级定时器和通用定时器互相独立。




-  ### USART和UART

universal asynchronous reciver and transmitter    通用异步收发器

universal synchronous asynchronous receiver and transmitter  通用同步异步收发器

USART 支持同步模式，因此USART 需要同步始终信号USART_CK（单片机可发送），通常情况下同步信号很少使用，因此单片机的UART和

USART使用方式差不多，都是用异步模式进行收发。

通过串口向单片机发送数据。 比如说发“abcde111" 串口以字符串发送数据，首先将字符串转化为二进制,一次性接收8位，相当于第一次

接收到”a“ -0000 1010 。是一个字符是一个字节，产生一次中断。 0x01 占一个字节 0x001占两个字节。 串口中断一次一个字节八位。

//取消ARM的半主机工作模式
#pragma import(__use_no_semihosting)
struct __FILE {
​    int handle;
};

FILE __stdout;
_sys_exit(int x)
{
​    x = x;
}

int fputc(int ch, FILE *f){
​    while((USART1->SR&0X40)==0);
​    USART1->DR = (u8) ch;
​    return ch;
}

关于使用HAL库开发F7中 使用 __HAL_UART_GET_FLAG（）函数卡死问题

参考链接：https://blog.csdn.net/shaynerain/article/details/73530119

注释了HAL库中 串口锁定功能代码，问题没解决。

 参考了网上这篇文章，大致是RXEN标志位未被清除，产生overrun错误，解决措施就是在判断标志位前，清除该清除的标志位。

通过标准的使用HAL库，卡死问题得到解决；

UART和USART 使用起来没区别， 如果设置同步，得加时钟信号，等待发送完成后继续发送（USART）。


-  ### 堆 FIFO  栈 FILO

心得：对于搞嵌入式的人来所，嵌入式系统的内存是宝贵的，内存是否高效率的使用往往意味着嵌入式设备是否高质量和高性能，

所以高效的使用内存对我们来说是很重要的。



-  ###  Git

.gitignore 用于忽略你不想提交到Git上的文件

.gitattribute 指定非文本文件的对比合并方式

pull是从remote repo拉取local branch的最新代码下到local repo；

fetch是从remote repo拉取other branches的最新代码下到local repo。





-  ### ESP8266

AT指令固件包  NodeMCU固件包

芯片是乐鑫的  模组是安信可的

ESP8266 也可以用SPI进行通信开发（未完待续）：

一共两个SPI：SPI和HSPI   其中SPI已经和内部FLASH连接 HSPI可以为我们所用（仅支持主机模式）
图:
震惊，给8266刷上nodemcu固件 竟然变成了8位MCU！ （抛弃AT，暂弃SDK！）

266如果要进行SPI与外部MCU进行通讯

https://bbs.espressif.com/viewtopic.php?t=2426

https://github.com/espressif/ESP8266_RTOS_SDK

https://www.cnblogs.com/yangfengwu/p/7524297.html

https://www.espressif.com/zh-hans/support/download/documents?keys=&field_type_tid%5B%5D=14

https://bbs.espressif.com/viewtopic.php?f=67&t=225

https://wenku.baidu.com/view/7160d1ff312b3169a551a452.html

CPOL控制空闲时的电平状态，CPHA控制数据在CLK第几个沿开始传输。

高位开始传输

-  ### FreeRTOS

利用STM32CubeMX 进行基本配置

首先配置始终，选择外部晶振输入，然后配置两个IO口并改标签。然后使能FreeRTOS。

切换第二个表卡，点击图标。Config parameters
是配置参数，列出了FREERTOS中可配置的参数，对应FreeTTRTOSConfig.h中的参数

Include parameters 选项卡的参数是用来配置裁剪系统的。

Tasks and Queues 用于添加任务和队列

Timers and Semaphores 是添加软件定时器和信号量的选项


-  ### JTAG JLINK SWD  下载调试

JLINK是利用STM32的JTAG口进行下载和调试的。

使用了JLINK V7版本（利用的是之前飞思卡尔 Zigbee开发板送的）

 用了四根线  VCC GND CLK SWDDIO 。发现下载不进去 提示尝试Reset。 因为仿真器没有这个脚。手按着开发板的复位键也不行。

MDK 设置Reset模式也没用 。网上说与一种system reset 模式和  vect reset 都没有找到。 仿真器的reset 脚接开发板的reset脚 。不就是控制开发板的reset吗

最后没有下载成功。 希望以后能用上高级版本的jink 进行测试。

-  ### STM8

Stm8cubemx 只能生成引脚分配图  不能生成代码

Stm8 AD 配置流程
- 使能或失能AD外设
- 初始化和配置寄存器，ADC分辨率，ADC转换模式
- 配置外部触发源
- 通过软件设置触发AD转化

typedef* structure

structure-> =

typedef structure

structure. =

-  ### C指针



int *p；p是一个指针变量 需要指向一个地址 （初始化） p就是一个需要被指向整形变量地址的变量。

int *p；
p=&a;
*p++;
print(a)//a=a+1;


在形参中 int *p 就是一个地址。 point( int *p){}

int* p;
p="abcd"; //可行 应为字符串传递值就是字符串的首地址

int a[10];
p=a; //数组a的首地址
char *arr[4] = {"hello", "world", "shannxi", "xian"};
//arr就是我定义的一个指针数组，它有四个元素，每个元素是一个char *类型的指针，这些指针存放着其对应字符串的首地址。
char (*pa)[4];
//pa是一个指针指向一个char数组，每个数组元素是一个char类型的变量

%2f 指打印数据宽度为2位（包括小数点），不满两位就左边补0； %.2f 指保留两位小数。

%02d 表示用输出整形不足两位用0来补，

函数指针  传入函数地址
int * p a[2](int x, int y)=
{
​    main,
​    slave
}

a[1](1,2)

-  ### RS485

RS485转换器SP3485。其中5脚和8脚是电源引脚；6脚和7脚就是RS485通信中的A和B两个引脚；1脚和4脚分别接到单片机的RXD和TXD引脚上，直接使用单片机UART进行数据接收和发送；2脚和3脚是方向引脚，其中2脚是低电平使能接收器，3脚是高电平使能输出驱动器，我们把这两个引脚连到一起，平时不发送数据的时候，保持这两个引脚是低电平，让MAX485处于接收状态，当需要发送数据的时候，把这个引脚拉高，发送数据，发送完毕后再拉低这个引脚就可以了。为了提高RS485的抗干扰能力，需要在靠近MAX485的A和B引脚之间并接一个电阻，这个电阻阻值从100欧到1K都是可以。

modbus 是一个应用层级的协议。 特点包括数据头的设备地址，功能码，n个数据位，CRC校验位。 另外主机发送请求后，从机会有对应的响应。

freemodbus的移植与使用





参考网址：

https://www.amobbs.com/thread-5491615-1-1.html  Freemudbus移植

https://github.com/armink/FreeModbus_Slave-Master-RTT-STM32  主从机模式 都有




-  ### 差分信号

任何两个信号都可以分解为共模信号和差模信号。共模信号是作用在差分放大器或仪表放大器两个输入端的相同信号，通常是由于线路传导和空间磁场干扰产生的，是不希望出现的信号，差模信号是两个输入端信号的相位相差180度。如果共模信号被放大很多，会影响到真正需要放大的差模信号。设两路的输入信号分别为： A,B，分别表示为：A=m+n；B=m-n，则输入信号A,B可以看成一个共模信号 m 和差模信号 n 的合成，其中m=(A+B)/2；n=(A-B)/2。
常用共模抑制比CMRR来衡量差分放大电路抑制共模信号的能力，它是放大器对差模信号的电压放大倍数与对共模信号的电压放大倍数之比，CMRR越大，放大器的性能越好。

在差分放大电路中，无论是温度变化，还是电源电压的波动等，都会引起两管（两特性相同的三端器件所组成的差分放大电路的输入端）集电极电流以及相应的集电极电压相同的变化，其效果相当于在两个输入端加入了共模信号。温度变化、电源电压的波动等引起的信号变化当然要加以抑制，所以要抑制共模信号。
常用共模抑制比作为一项衡量差分放大电路抑制共模信号的能力的技术指标，其定义为：放大电路差模信号的电压增益与共模信号的电压增益之比的绝对值。共模抑制比愈高，抑制共模信号的能力越强。

-  ### PCB
 #### 布局
DC/DC 变换器、开关元件和整流器应尽可能靠近变压器放置，整流二极管尽可能靠近调压元件和滤波电容器。以减小其线路长度。

　　电磁干扰（EMI）滤波器要尽可能靠近 EMI 源。尽可能缩短高频元器件之间的连接，设法减少他们的分布参数及和相互间的电磁干扰。易受干扰的元器件不能相互离的太近，输入和输出应尽量远离。

　　对于电位器、可调电感线圈、可变电容器、微动开关等可调元器件的布局应考虑整块扳子的结构要求，一些经常用到的开关，在结构允许的情况下，应放置到手容易接触到的地方。元器件的布局到均衡，疏密有度。

　　发热元件应该布置在 PCB 的边缘，以利散热。如果 PCB 为垂直安装，发热元件应 该布置在 PCB 的上方。热敏元件应远离发热元件。

　　在电源布局时，尽量让器件布局方便电源线布线走向。布局时需要考虑减小输入电源回路的面积。满足流通的情况下，避免输入电源线满板跑，回路圈起来的面积过大。电源线与地线的位置良好配合，可降低电磁干扰的影响。如果电源线和地线配合不当，会出现很多环路，并可能产生噪声。

　　高、低频电路由于频率不同，其干扰以及抑制干扰的方法也不相同。所以在元件布局时，应将数字电路、模拟电路以及电源电路按模块分开布局。将高频电路与低频电路有效隔离，或者分成小的子电路模块板，之间用接插件连接。

　　此外，布局中还应特别注意强、弱信号的器件分布及信号传输方向路径等问题。 为将干扰减轻到最小程度，模拟电路部分和数字电路部分分隔开之后，保持高、中、低速逻辑电路在 PCB 上也要用不同区域，PCB 板按频率和电流开关特性分区。

噪声元件与非噪声元件要距离远一些。热敏元件与发热元件距离远一些。低电平信号通道远离高电平信号通道和无滤波的电源线。将低电平的模拟电路和数字电路分开，避免模拟电路、数字电路和电源公共回线产生公共阻抗耦合。

高压部分需要与地保持间隔，并用丝印层标出。有些高压输出的地方不需要铺铜。

-  ### MQTT

 使用 Modbus 作为本地接口来管理设备，使用 MQTT 作为全局协议来扩展设备的范围，二者都起到了重要的作用.

 创建实例，创建设备，创建身份（每一台设备thing都需要绑定相应的身份principal），创建策略，得到账号密码。

 创建客户端，填入账号密码（不明）。

 S8WXST3StTeHQNTEVC4MciPoSTVEyjNji6iTcGyiR+0=
 S8WXST3StTeHQNTEVC4MciPoSTVEyjNji6iTcGyiR+0=

 -  ### SDIO

 SIDO 第一步设置SDIO_POWER 寄存器最低2位为1 上电


  -  ### FATFS

  ①修改数据类型 integer.h
  ②配置相关功能  ffconfg.h
  ③底层驱动函数编写 一般编写6个函数 diskio.c

  从我学习FATFS到现在已经2个月了，大概能操作TF卡后就开始往里写数据，还一直在改写写卡的逻辑，下面是我在写卡时总结的（可能并不适应与你）。

1、硬件：lpc2132 硬件SPI操作 22M主频
2、文件系统：FATFS R0.09
3、卡：2G、4G、16G（有的是大陆产的，有的是台湾产的。目前2G的卡有大陆和台湾产的，发现大陆的这个没台湾的好，写卡时电流不稳定，而台湾的那个很 稳）
4、数据：AD采样进来的音频数据，短的几秒钟，长得几个小时
5、方式：录完一个系统进入掉电模式，有录音需要时再起来录（每次单片机起来这里就会建立一个txt文件）
6、AD：pcm1870（谁用过这个东东呢，它老是丢数，我问ti在线支持了，他们没给我个好的解决方法，不知你们发现没有丢数这个现象）
7、采样率：8K


注意点：1、在TF卡中写时不要直接在根目录下写文件，最好先建立一个文件夹，在文件夹里写，这样工作电流就会小很多，而且电流也很稳定（主要原因）
​        2、一个文件夹里的txt文件数目不能太多，否则单片机从掉电模式起来到写卡时有点慢，会造成前面的数据丢失（我的2G的卡里，每个文件夹里的文件不能超过50个，超过后唤醒单片机到写卡时间过长，造成一些数据丢失）
​        3、小容量卡写电流小，大容量卡写电流大（2G:40mA，4G:50mA，16G:100mA，）
​        4、写小的文件（写几秒就要关闭的）用f_sync(&file);写大文件（就是连续写n个小时）用f_close(&file);

还在郁闷的问题：
1、第一次单片机从掉电模式起来的比较慢（大概2秒），但从第二次后起来就非常快了
​    注：程序到while(1)前面就进入掉电模式了，每次从掉电模式起来就到while(1)里了，执行的语句都是一模一样的啊，怎么会这样呢

2、由于第一点的迷惑，我怀疑单片机从掉电模式起来后是不是从while(1)哪里执行呢？不是说：从哪里掉电，起来时就从那里执行的吗？？

3、2G的卡写电流很稳定，但4G和16G的就不怎么稳定了，尤其是16G的，写卡时电流摆动很大，即使寻址范围大也不至于电流摆动那么大吧
   我的16G卡写电流摆动范围：50mA-100mA啊，天哪，手机里或别的电子产品里可不是这样啊，难道这是fatfs的原因，或是我写卡的逻辑不对？？？？？？？？？？？？？？？？？？？？

这是我写卡的程序：SD_Write_Flag是定时器里的一个变量，如果SPI数据寄存器满了（AD给的数据），他就为1，定时器每个500us去看一次,
​         if(SD_Write_Flag)
​         {
​            SD_Write_Flag=0;
​            if(buffer_number)
​            {
​              res1 = f_write(&file, buffer1, 512, &br);     //写入
​              f_sync(&file);
​              IO0SET |=1<<15;         //P0.15输出高，关闭LED
​            }
​            else
​            {
​              res1 = f_write(&file, buffer0, 512, &br);     //写入
​              f_sync(&file);
​              IO0CLR |=(1<<15);      //P0.15输出低，点亮LED
​            }
​         }

 -  ### UCosIII
 ##### 参考的是安富莱ucosiii的教程：http://forum.armfly.com/forum.php?mod=viewthread&tid=1788&extra=page%3D1

 调度器：使用相关算法来决定当前执行任务。 就绪任务和挂起任务 。激活一个就绪态的任务。
 合作式调度器：只有一个任务可以执行，不可被抢占。直到自愿放弃、
 抢占式调度器：激活就绪任务中优先级最大的一个（ucos是数值越低越高）
 时间片调度器:Round-robin算法。适用于不要求任务实时响应的情况。给同优先级的分配一张列表。运行一段时间长度以后切换。
##### 移植
M4比M3多了一个浮点单元。
内核OS特性：
双堆栈指针 ：MSP PSP
SysTick定时器：OS时钟
SVC和PendSV：两个异常 （任务切换）
非特权级执行模式：提高系统鲁棒性
独占式访问：独占式存储和加载指令作用于信号量和互斥操作。
R0-R15寄存器  R13为堆栈指针SP
R0-R7 低位寄存器 16位指令
R8-R12 高位寄存器 32位指令和部分16位指令
MSP 是默认堆栈指针。由OS内核，异常服务和特权访问的程序代码使用
PSP 常规程序代码使用
![](http://ww1.sinaimg.cn/large/006Esg89gy1frry0uylk6j30fa02ydfu.jpg)

R14是连接寄存器， 保存函数返回地址

R15是程序计数器 （PC） ，返回值是当前指令地址+4

程序状态寄存器 PSRs或xPSP

![](http://ww1.sinaimg.cn/large/006Esg89gy1frryl1hd3fj30mb091t9i.jpg)
中断屏蔽寄存器组

![](http://ww1.sinaimg.cn/large/006Esg89gy1frrylhppwfj30lv07cmy1.jpg)
如何操作

![](http://ww1.sinaimg.cn/large/006Esg89gy1frryzw4pjij30mc0bj752.jpg)
控制寄存器 ：定义特权级别和选择当前使用哪个堆栈指针。





 -  ### EMwin

 STM32 FPU 需要DSP库  在STM32Cubermx里

 STM32片内自带SRAM和FLASH，FLASH是用来存储程序的，SRAM是用来存储程序运行中的中间变量，通常不同型号的.STM32的SRAM和FLASH大小是不相同的，以我手边的STM32F103VET6来看，根据数据手册可以看到

EMWIN 编辑框可以通过接口获取输入的字符串值。

可以通过自定义回调函数来实现按钮的各种效果。

对话框好像不能显示背景图，窗口可以。

显示屏相关知识：

常用的三种显示屏搭配：

 RA8875(SSD1963) 芯片自带显存。支持低端单片机接口通过8080接口或者SPI或者IIC接口转成RGB 驱动大屏

 ili9948类显示屏，支持各种接口，显存集成，驱动的屏幕较小，也有RGB接口的屏幕。（同ili9341，ili9326，SPFD5420)

 STM32F429/439/769+ SDRAM+ RGB接口裸屏幕 ，利用芯片内部自带的LCD TFT 控制器 配合内外SDRAM ，整体作用和RA8875一样。

TFT裸屏中主要两个IC ：Gate Driver IC 和Source Driver IC ，很多引脚。 使用一块屏幕之前需要知道时序参数，如果没有，需要知道Driver IC型号，对应时序。


emWIN 的移植： 1.显示屏的移植： 关注一个文件：LCDConf_Lin_Template.c 12个宏配置和一个背光调节函数 LCD_SetBackLight

2.触摸的移植  ：电容触摸 配置GUI_PID_StoreState 函数存储 触摸坐标和触摸按下状态至FIFO  GT811 FT5X06

电阻触摸： 相比电容触摸，多了滤波处理，返回的是AD值。 STMPE811

移植STemwin 库：

1.下载官方软件包，将库文件和配置文件添加至工程

2.SDRAM 驱动，4MB 图层1 4MB 图层2 8MB emwin动态内存 参考链接 ：http://bbs.armfly.com/read.php?tid=1942

3.LTDC 引脚配置和时序配置    时序配置：需要提供 LCE_ConfigLTDC 参考链接：http://bbs.armfly.com/read.php?tid=18528   引脚配置也在这个函数中

：IO初始化   LTDC时钟使能，DMA2D时钟使能，LTDC引脚配置。

4.触摸屏驱动配置： 手册：http://bbs.armfly.com/read.php?tid=18581

原话：电容屏触摸的移植比较简单，如果用户用的触摸 IC 跟开发板一样， 直接拿来用即可，如果不一样，

需要先将触摸 IC 的驱动实现，然后按照开发板提供的 GT811 或者 FT5X06 的触摸扫描函数，照葫芦

画瓢实现一个即可。

 电阻屏的移植稍麻烦些，如果用户用的触摸 IC 跟开发板一样， 直接拿来用即可，如果不一样，需要

先将触摸 IC 的驱动实现，然后仿照 TOUCH_STMPE811.c 文件提供触摸按下状态函数， X 轴， Y 轴

的 ADC 数值读取函数和清除触摸中断标志函数。最后用重新实现的这几个函数替换 bsp_touch.c 文

件中的原函数即可。 另外要注意一点，这种方式实现后， 虽然触摸校准依然可以使用， 但是开发板的

触摸校准参数是保存在 EEPROM 中的，用户可以根据自己的实际情况选择存储介质。 另外， 触摸参

数的保存和读取在 param.c 文件实现。

 如果大家不想用开发板实现的方案，想自己重新实现一个，也是没问题的，注意跟 emWin 关联的方式， 即函数 GUI_PID_StoreState 的使用。

5.STemwin 底层接口函数配置

GUIConf.c 动态内存配置 内外部和大小

GUIConf.h 系统配置 图层 OS 触摸 鼠标 窗口管理器 存储 设备指针

LCDConf_Lin_Template.c 重要，配置了显示尺寸和显示驱动，颜色转换和底层优化。



GUIConf.c 文件 实现系统动态内存的分配
-  ### NB-Iot

5.25收到NBIOT模组，模块是移远，型号BC-05 电信卡。（6.7日 持续吃灰中。。）

Bc95 要往自己服务器上发东西得绑定IP（打电话添加白名单），否则得用电信云或者华为云。

BC25全网通，不存在这个问题。

-  ### 荔枝pi

收到了一块5寸触摸屏（电阻） nano一块

哇哇哇 吃灰一个礼拜 （ 答辩）

今天晚上10点，惊觉醒。 扒开一堆“电子废品” 找到了那块闪着光的Lichee Pi  有点小激动。

给补焊上去了，否则是分分钟要被我泰拳警告的。后来接屏+上电---果然白屏..溜回学校答辩。

打开了盒子，拿出了这块搭载全志fS100s应用级处理器的核心板（cortex A7）哇哇哇哇 高端，以前只摸过F7 （虽然树莓派已经吃灰很久了 = =）。

废话不多说，先在群里下载了各种资料，打开......MMP 没有我想要的上电就亮碉堡的镜像啊。咋办啊 ，各种资料，一度开始膨胀到linux下SDK的搭建，开始移植UBOOT。。。。

弄了半天发现我错了.. 因为我看到了这么一句话：首先进行的是安卓镜像的傻瓜是烧写，目的检测收到的荔枝皮一切都正常。烧写群主提供的镜像文件安4.1版本，启动成功！

惊了。 赶紧找那个就是我现在要搞的安卓镜像... 现在在下载。
30分钟过去了,发现下错了+网速卡。 好在终于百分百找到了我想要的那个，开始下载，坐骑云 美滋滋。
烧写完毕...白屏。定视一下,操。。 屏幕的排线被我弄断了  GG  界面显示不起来了。 咸鱼的那个7寸屏也点不亮。 明天找其他解决方案，暂时不排除lichee pi 本身的原因。
找到了原因了 Lichee本身是没有问题的。是我自己太菜。。。

6月7日，荔枝派Nano 移植U-Boot+点亮屏成功。需要注意LCD参数的修改。（稍晚填坑）。


-  ### STM32向量表

参考链接：https://www.cnblogs.com/WeyneChen/p/4846792.html
尝试使用手边的STM32F103使用汇编点亮LED，使其闪烁。 AHB2和GPIOA的地址应该都是算对的，可是未见实验现象（待解决）。


-  ### OPC

1、枚举本地服务器

2、获取服务器信息

3、列出了服务器上Tag

4、可以设置组的属性

5、读\写功能

6、可进行远程连接（DCOM需配置）

https://www.kepware.com/
暂缓
  采用力控软件模拟OPC服务器，在其界面按下F1帮助中查找开启OPC服务器。然后通过client软件在局域网内搜索开启的OPC服务器。此中涉及Windows的防火墙设置和DCOM设置，比较繁琐。 连上服务器后选择组，添加标签，获取值。（目前获取的值不更新，待解决）。

  所谓的OPC通俗的讲是为了方便写软件的人的，有些人不懂底层，不知道如何通过各种协议读出接入设备的其他传感器或者设备的传来的值。然后OPC就发挥它的作用，指定了一种标准，软件开发人员只要根据它提供的接口来获取值就可以。OPC服务器需要厂商提供，目前只支持WIN也是其弊端基于COM，但是现有OPC UA标准可以解决这个问题。

  -  ### 锅炉项目

  锅炉项目设计485通讯，指纹识别，以及RTOS和文件系统。目前正在开发。

  遇到了printf 中文输出乱码 编译乱码的问题，但是看上去就是比一般ansl编码的还要正常。重新改字体编码格式即可。

  大坑 ，调了大半天，串口使用硬件定时器来判断一振的字符串。 定时器抢占优先级一定要一样（最好，反正不一样今天就遇到了很玄学的问题，定时器不能触发中断！ ） 一定要一样 。指纹模块启动！

  天坑 24C02连续读写的时候中间一定要加延时，不然只能读首末字节。延时大约5-20ms。

  485用modbus 。线圈： 一位代表一个开关量，可读写，01 05 0f。保持寄存器，2个字节，可续写，04 06 10。 离散输入和输入寄存器，开关量和寄存器，不可写，02 04。

-  ### Linux

   首先，这是一个硬骨头，得啃。

  手上有一块mini2440的板子，很巧，用一下。 记录一点东西。

-  ### IAP技术

   运用IAP技术（应用程序内编程），最理想的就可以实现空中升级。

   整个过程按照如下步骤:
1. 解锁
2. 判断是否保护,有保护的话要先关闭保护
3. 擦除
4. 编程
5. 复位进入应用程序区

关于解锁:看资料的时候说的神乎其神,有个读/编程控制器叫”FPEC
有几个寄存器,专门负责 Flash 的,对这几个寄存器以一定得顺序访问并设置即可成功解锁
Flash,至于怎么访问,谁先谁后,数据手册上写的头晕,直接来个快刀斩乱麻 Flash_UnLock()函
数封装了这一系列的操作,有一点要注意,如果你是自己操作寄存器的话,如果操作的方法或
者顺序不对都会造成 Flash 的锁定,之后的所有操作都会返回一个错误,直到下次启动后才能
正常操作
关于保护,为了保护用户数据不被无意修改或者恶意读取,STM32 提供了对芯片 FLASH 的
写读等一系列的保护,加密方式是按照每 4 页为一个单位,也就是说,如果你想加密的话,你至
少要加密 4 页,也就至少 4K 的空间,至于高密的 STM32 是否就是 8K 了?这个我没仔细去看!还
待以后仔细查看?
关于擦除,擦除也是很简单,但是只能一页一页的擦除,ST 公司也提供了一个函数,至于这
个函数后面的输入地址参数,经过试验发现只要这个地址落在这个页里,就是擦除这个页,不
知道这样理解对不对,还需要验证???
FLASHStatus = FLASH_ErasePage(Address );
关于编程,STM32 编程一次只能以半字(16 位)的方式编程,库提供了两个函数
FLASH_Status FLASH_ProgramWord(u32 Address, u32 Data) 编程一个字
FLASH_Status FLASH_ProgramHalfWord(u32 Address, u16 Data) 编程半个字
在实际编程虽然你调用的一个字编程的时候内部操作仍然是按照半字的方式编程
另外还有个最最重要的一点:还要注意大小端的问题,有些你认为可移植性很好的代码,其实
并不一定,用位移组合成一个32位整型,然后当做参数来编程,由于大小端的模式,刚好第一个
字节在最后边了,最后一个字节在前面了,导致了AN2557下载我的代码可以使用,我自己的下
载我自己的代码竟然不能使用,很是郁闷了一阵子,读出整个Flash的内容就很容易看出来不
同了,这个没有想到也着实该死,后来用指针强指,不但效率高了,程序也方便了。
指定你的代码从哪个地址加载， 然后还要指定你的代码区的 Size， 也就是你芯片 flash 的总 size
减去 IAP 程序的 Size 后得到的， 这个设置的地方在 project Options for Target 的 IROM1
这个位置指定，还有一个地方，在 Linker 页面有个项也要指定
R/O Base 设置包含 RO 输出段的常量和代码域地址。地址必须为字对齐。如果没有指定，缺省的
RO 地址为 0x8000。 这里也要设置成应用程序启动的地址 R/O 基地址含有初始化程序的入口地址。
最最重要的一点，中断向量表的映射，因为应用程序从其它地方启动了，向量表同样也要增加偏移，
不然中断入口找不到可是个麻烦事
NVIC_SetVectorTable(NVIC_VectTab_FLASH, 0x2000);
这个函数就是做这个的，第二个参数是相对的，不是绝对的，也就是基地址+第二个参数

参考链接：http://bbs.eeworld.com.cn/thread-294115-1-1.html

AN2557

-  ### 以太网通信

MAC（链路）+PHY（物理）   PHY芯片是LAN8720   接口是RMII接口  7根线； 通过SMI 总线对PHY寄存器进行读写 可以控制32个芯片。

通过设置RXER/PHYAD0 引脚来设置PHY地址，下拉默认为0。 通过xxxhal_config.h里修改

设置nINTSEL 引脚（2号引脚） 来设置nINT/REFCLKO 引脚（14引脚）。A=0  B作为时钟源。

LWIP 协议栈使用 。能做WEB SERVER TCP SERVER TCP Client UDP..；

1.带操作系统的移植

① 下载源码+HAL库
② ..

首先要驱动EHT外设，即驱动RMII接口的LAN8720，然后移植LWIP协议，最后通过接口进行编程。

修改文件：

- #### ethernetif.c

MSP初始化硬件接口，low_level_init

初始化MAC相关工作外环境,初始化DMA描述符链表，使能。

low_level_output 是底层发送一帧数据函数 。

low_level_input底层接收一帧数据函数。

sys_now 获取当前时间。 ethernetif_init 初始化网络接口并调用 low_level_init

ethernet_input 调用low_level_input

- #### app_ethernet.c

主要是实际的网络初始化应用程序所在的.c

Netif_Config 是创建一个网络接口。

User_notfication 是用来获得网络状态的函数

- #### lan8720.c 和lan8720.h

初始化相关I/O。


嵌入式网络通信笔记。（阅读嵌入式网络那些事---LWIP协议深度剖析与实战演练）

OSI模型与传统TCP/IP模型可以对应起来。 不同层进行协议的收发或者修饰或者解析。

RISC指令集（精简指令集）  ARM是32位架构，有32条地址线，寻址范围是4GB ，内存存储最小单位为字节。

32位处理器中，一个字长等于四个字节。大端，高存低。小端相反。通信的时候，一般都是高字节先发来，然后进行存储，此时要注意通信时要注意大小端。

Thumb是16位指令集。在运行时，16位处理器首先要经过处理器解压为32位的指令



-  ### DMA

之前一直想玩DMA ， 奈何只是河边走走，穿着雨靴。 借着这次锅炉项目，打算将其中485的串口数据，直接通过DMA的方式，放进内存空间里。

开动！

F7一共有2个DMA ，16个通道。 有优先级关系。

每个通道有控制寄存器，写2:0，配置功能

仲裁器用来管理传输优先级 1.配置四个优先级2.如果一样，则数据流编号越低优先级越高。

DMA传输具有直接模式和FIFO模式。直接模式是请求时直接将数据从FIFO缓冲区传输到目标寄存器。

FIFO用于数据宽度控制。（还有一个突发传输）

DMA1没有外设端口没有连接至总线矩阵，只连接到了APB1外设，所以DMA1不能实现存储器到存储器传输。

流控制器控制传输数据个数，记录满后停止传输。SDIO具有发停止传输信号的功能。

循环模式：一次或循环

突发模式提高速度，与FIFO配合使用，要求阈值为传输的字节的整数倍。

直接模式不能用于存储器到存储器传输。

双缓冲模式，设置DBM位为1，并自动开启循环模式，大致意思是循环传输两个存储区。

串口接收用DMA方式实现。 不定长数据可以用DMA+串口空闲中断配合。（今天下午测试，串口接收到数据后，不进入串口空闲中断，感觉应该是串口配置的问题）

程序上电后，串口还没有接收到数据就进入了串口空闲中断。应该是配置问题、


-  ### STM32串口收发经验

一定要对应着STM32参考手册来进行串口程序的编写，很多坑里面都有描述。

采用消息队列的方式来进行数据的收发，更安全和高效。 暂未进行拆包和补包的处理。

消息队列的简单C语言实现方式：

''' C
typedef struct
{
  unsigned int Buff[100];
  unsigned int Len;
}DataInfo;

typedef struct
{
  DataInfo Databuf[20];
  unsigned int In;
  unsigned int Out;
  unsigned int Count;
  unsigned int Size;
}Cache;

DataInfo *p.
p = &Cache.Databuf[Cache.In];


'''



 -  ### 文本编码

在使用Keil5编译代码的时候出现，文本显示中文字符正常，编译时中文显示出错的问题。

ANSI 微软专用，每个地区locale对应不同的编码。中国默认的是GBK-936

GB2312 小型中文库。GB18030更全面，照顾少数名族兄弟。

Ascii 支持英文和一些标点符号和特殊符号。

Unicode 是包含了所有的编码。

UTF-8是传输方式，对应还存在UTF-16。网页上的字一般都是UTF-8进行显示。TXT默认是UTF-8。

现在使用Keil5时候，编译中文乱码。原因是txt转.c文件，这个.c文件还是UTF-8的解码方式，里面的中文字符不能被ARM CC编译（坑） ， 需要把这个UTF-8的.c用TXT打开，然后保存为ANSI，一切编译正常。keil里的中文又显示的怪怪的（那就对了）。

调试了一天...

- ### 项目产品

首先要明确需求。再产品设计之初，预留扩展空间（i/o，存储一类）。

模块化设计，达到可更换和快速修改原型的效果。



与实际使用场景相结合，考虑运输，损耗，信号强度等实际问题。

如果做系列产品，考虑接口的兼容性。

- ### 8080显示屏驱动以及显示
合理恰当的使用"_inline"内联函数关键词修饰字，可以减少函数调用时的消耗。

在8080驱动中，在初始化的函数中，大量使用了write_cmd和write_data函数，使用此关键字，将

内联函数的代码加入进来，进行代码优化。

刷新原理，字模取模技巧。

开窗 写数据。封装成各种函数。
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
*
