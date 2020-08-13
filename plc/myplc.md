
>西门子PLC 
====


第1章 概述
==========

边缘计算网关Edge模块主要执行数据收集和现场级数据策略(详情请查看公司官网关于Scaedge的介绍 https://www.scapeak.com/ )。西门子PLC以太网驱动OPCUA_SimaticTcpClient(基础版)，OPCUA\_
SimaticTcpClient_Advance(高级版)是安装在Edge模块内的通过OPCUA服务器采集西门子SIMATIC-S7系列的PLC数据的驱动。适用于西门子SIMATIC-S7系列的PLC，常见的有S7-200/300/400，SMART200，S7-1200，S7-1500。

1.1 软件介绍
------------

软件名称：MDC_OPCUA_SERVER（一般出厂默认安装）

软件说明：OPCUA数据采集服务

软件文件夹路径：/opt/scapeak/MDC_OPCUA_SERVER

软件路径：/opt/scapeak/MDC_OPCUA_SERVER/MDC_OPCUA_SERVER

Edge模块内置凌顶自主开发的 OPCUA
服务器MDC_OPCUA_SERVER，可以集成各种数据采集的驱动，例如西门子PLC以太网驱动。

1.2 驱动介绍
------------

1.  驱动名称：OPCUA_SimaticTcpClient（一般出厂默认安装）


1)  驱动说明：OPCUA驱动西门子PLC以太网(基本版)

2)  驱动文件夹路径：

> /opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient

3)  驱动路径：

> /opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient/libSimaticTcpClient.so

2.  驱动名称：OPCUA\_ SimaticTcpClient_Advance（一般出厂默认安装）


1)  驱动说明：OPCUA驱动西门子PLC以太网(高级版)

2)  驱动文件夹路径：

> /opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient_Advance

3)  驱动路径：

> /opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient_Advance/libSimaticTcpClient_Advance.so

其中，西门子PLC以太网(基本版)和西门子PLC以太网(高级版)的驱动都可以采集数据，区别在于基础版会连续读地址错误的变量，因此当PLC的变量地址有效时会立即读出数据，OPC不需要重启。而高级版为了更快的读取数据，在启动时会读取一次所有变量的值，剔除无效变量后再进行变量地址合并优化后再连续通讯。因此高级版不会读取无效变量，因此当PLC的变量地址有效时也不予理会，需要重启MDC_OPCUA_SERVER才行。

例如：DB100对于PLC来说是无效变量，基础版每次仍会读取该变量，高级版第一次读取该变量无效时会剔除它，之后不会再读，相比基础版效率更高。但是如果您重新编写PLC程序后，DB100变成有效变量，基础版会直接读出该数值，而高级版因为已经剔除该变量就不会读取到，需要重启MDC_OPCUA_SERVER。

1.3 适用的PLC类型
-----------------

1.  S7-200系列：


1)  该系列的CPU本体已集成PPI/MPI通讯口，可采用SCANET模块将其转换为以太网通讯。（SCANET模块）

2)  如果有EM277扩展通讯模块，其有MPI通讯口，采用SCANET模块将其转换为以太网通讯。

3)  如果有扩展CP243-1以太网模块，可直接通讯。


2.  SMART200系列：


1)  该系列CPU本体已集成MPI通讯口，采用SCANET模块将其转换为以太网通讯。

2)  该系列CPU本体也集成以太网通讯口，可直接通讯。


3.  S7-300系列：


1)  该系列的CPU本地已集成MPI通讯口，可采用SCANET模块将其转换为以太网通讯。

2)  如果有CP343-1以太网扩展模块，可直接通讯。


4.  S7-400系列：


1)  该系列的CPU本地已集成MPI/PROFIBUS通讯口，可采用SCANET模块将其转换为以太网通讯。

2)  如果有CP443-1以太网扩展模块，可直接通讯。


5.  S7-1200/S7-1500系列：


1.  该系列CPU已集成以太网通讯口，可直接通讯。

2.  注意：驱动不能读写优化DB块，要取消优化块访问。该系列的PLC内部的数据块（DB）有两种，一种是标准DB块，一种是优化DB块。优化DB块是无法通过数据地址进行访问的。(详细步骤可查看S7-1200测试示例)
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image4.png" width="80%"/>
<!-- <div align=center>
 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image4.png" width="80%"/>
</div> -->
3.  注意：DB块需要开放读/写权限。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image5.png" width="80%"/>

第2章 硬件连接
==============

2.1 模块供电
------------

Edge模块供电（手册使用的是EdgePLUS，以下统一使用EdgePLUS）：

EdgePLUS可以使用24V直流电源供电，提供24V接线端子，或者使用5V电源，可以用电脑供电，USB线插在Debug口。一般推荐使用24V接线端子供电更加稳定。

2.2 设备连接
------------

1.  若PLC有网口，使用以太网直接连接：

> 一般用到西门子PLC、EdgePLUS、计算机和网线。EdgePLUS有两个网口。例如：按照图示连接硬件。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image6.png" width="80%"/>

其中，Edge有两个网口，分别为Eth0和Eth1，这两个网口的可由用户设置IP地址，但是不能设置为同一网段。如果像图2-1一样连接，则PLC需要和Eth0在同一网段；计算机需要和Eth1在同一网段。例如：根据PLC的IP地址和计算机IP地址修改Edge的网口配置。

  设备       网口                 网口
  ---------- -------------------- --------------------
  PLC        192.168.215.1        
  计算机                          192.168.1.200
  EdgePLUS   Eth0:192.168.215.2   Eth1:192.168.1.118

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image7.png" width="30%"/>


2.  PLC有网口，使用交换机连接

> 如果您用交换机连接硬件，Edge只使用一个网口例如：Eth1,可以如下图所示进行连接。IP地址配置如下：

  设备       网口                  网口
  ---------- --------------------- --------------------
  PLC                              192.168.1.201
  计算机                           192.168.1.200
  EdgePLUS   Eth0:任意（不使用）   Eth1:192.168.1.118

> 硬件连接：
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image8.png" width="80%"/>


3.  PLC没有有网口，使用SCANET串口转网口

> 如果您的西门子PLC没有网口，则需要使用我们的SCANET模块将串口转成网口。SCANET为西门子SIMATIC自动化控制系统的以太网信息化数据采集提供一个统一的平台，在这里只做简单介绍，具体介绍请查看公司官网(<https://www.scapeak.com/gongyewulian/SCANET/80.html>)。

1)  型号：

-   S7PPI 型号应用于 SIMATIC-S7 的 S7-200、200 SMART 等 PLC
    控制系统的以太网通讯，支持 PPI 和 MPI 总线；

-   S7MPI 型号应用于 SIMATIC-S7 全系列 PLC 控制系统（包括
    S7-200/300/400）以及 SINUMERIK 数控系统，支持 PPI、MPI 和 PROFIBUS
    总线。

2)  硬件连接：

-   将SCANET模块的九针公口直接插入到控制器的通讯母口（PPI、MPI 和
    PROFIBUS 通讯口）即可；

-   模块会自动从PLC通讯口获得24VDC
    工作电源。如果PLC的通讯口不提供24V电源输出，则需要外接24V电源。

-   当SCANET插入到 PLC 后会自动锁定PLC通讯口的波特率，支持最高 6Mbps。

3)  指示灯：

-   SCANET共有四个指示灯，位于面板的红色电源指示灯PWR和绿色总线指示灯
    BUS，以及位于以太网插座的绿色Link指示灯和黄色Active指示灯。

-   红色电源指示灯PWR：在SCANET插入到上电运行的PLC的通讯口时红色电源指示灯应常亮；如果不亮则可能是
    PLC
    通讯口无电源或者SCANET模块的电源部分损坏或者指示灯损坏；检查办法是用一个24VDC电源连接到SCANET外部供电端子，如果电源指示灯仍不亮则说明SCANET模块故障；

-   绿色总线指示灯BUS：当SCANET插入PLC后应在1-3秒内变为常亮，常亮代表SCANET自动锁定了PLC
    通讯口的波特率；如果闪烁则需要分辨以下情况：

a)  以一秒间隔持续闪烁：代表总线地址冲突，SCANET模块的自身S7总线站地址默认为0，如果总线上还有其他站点地址为0，则可能出现此现象，请先将其他站点从S7总线上断开；后续通过SCANETPRO软件修改SCANET的站地址后再将其他设备接入S7总线；

b)  连续闪烁两次后约5秒左右再连续闪烁两次：代表SCANET无法锁定PLC的通讯口波特率；有三种可能性，一是SCANET通讯器件故障；二是PLC通讯口故障；三是PLC通讯口被设置为自由口通讯；对于第一第二种情况，可以尝试用编程电缆来测试；第三种情况通常为S7-200，请将PLC设置为STOP状态（当S7-200处于STOP模式时其通讯口为PPI协议）；以太网绿色连接指示灯
    Link：当SCANET接入以太网络时Link指示灯常亮；
    以太网黄色通讯指示灯Active：当上位机和SCANET无通讯时偶尔闪亮（网络查询），当上位机和
    SCANET通讯时持续闪烁；


4)  SCANET配置：

-   方法一：在浏览器输入SCANET的IP地址。新的SCANE默认IP地址为192.168.1.188。如果您修改了IP地址，并且忘记了请采用方法二。

a)  首页： SCANET基本信息，不做修改。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image9.png" width="80%"/>


b)  S7总线接口参数：修改S7通讯协议模式和总线波特率。您可以按照默认设置，不做修改。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image10.png" width="80%"/>


c)  以太网参数：

> 修改IP地址，请查看PLC有网口时的设置，该IP地址相当于PLC的IP地址。
>
> 通讯目标PLC地址由槽号决定：
>
> 否，该模式下DSAP不起作用，SCANET总是连接到默认的MPI地址。一般一个SCANET连接一个PLC设置为否。
>
> 是，多用于连接CNC，该模式下上位机设置DSAP的第二个字节值为MPI站号。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image11.png" width="80%"/>


-   方法二：使用SCANET配置软件

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image12.png" width="80%"/>


a)  搜索SCANET模块：

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image13.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image14.png" width="80%"/>


b)  修改IP地址：

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image15.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image16.png" width="80%"/>


c)  修改通讯接口：

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image17.png" width="80%"/>


d)  修改PLC通讯地址是否由槽号决定：

> 否，该模式下DSAP不起作用，SCANET总是连接到默认的MPI地址。
>
> 是，多用于连接CNC，因为CNC中的PLC和NCK都要通讯，就必须由槽号（通过DSAP）决定。该模式下上位机设置DSAP的第二个字节值为MPI站号。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image18.png" width="80%"/>


e)  通讯接口模块诊断

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image19.png" width="80%"/>

f)  以太网服务器模块诊断：可查看有几个上位机与PLC通讯。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image20.png" width="80%"/>


第3章 EdgePLUS配置
==================

3.1 EdgePLUS的IP地址配置
------------------------

1.  打开EdgePlant配置软件，搜索Edge模块。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image21.png" width="50%"/>
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image22.png" width="60%"/>


2.  修改IP地址

> 如果您需要修改IP地址，可以按照您的需求修改IP地址，可参考硬件连接。修改完成先点击下载按钮再点击重启按钮。重启完成的标志是SYS灯重新常亮。
>
> 注意：您还可以选择硬件断电重启，即给EdgePLUS断电重启。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image23.png" width="80%"/>


3.2 EdgePLUS配置变量
--------------------

1.  选择应用软件------数据采集，Project_Default右击新建一个组别。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image24.png" width="80%"/>

2.  组别名称：可以自定义修改。

组别启用：是，表示运行MDC_OPCUA_SERVER时，该组别变量有效。如果选择否，则表示该组别无效，所有变量均不读取。

如果配置文件中有组别不想使用，您可以右击该组别，选择删除，也可以组别启用选择否。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image25.png" width="80%"/>
height="0.9902777777777778in"}

3.  新建连接，右击该组别（siemens），新建一个连接

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image26.png" width="80%"/>


4.  选择连接驱动SIMATIC.TCP客户端（基本版）或者（高级版），二者区别请查看1.2
    驱动介绍。

> 按照如下配置驱动参数：

-   连接名称：可自定义修改；

-   连接启用：是（表示采集该连接配置的变量），否（表示不采集该连接的变量）；

-   连接说明：相当于备注；

-   IP地址是PLC的IP地址；

-   通讯端口：102，西门子默认通讯端口不能修改；

-   访问终结点：03.01（一般西门子1200是：03.01）；

-   通讯超时（毫秒）：设备连接超时，如果设备没有连接上，那么间隔多少毫秒重新尝试连接；

-   通讯间隔（毫秒）：所有变量读取一遍后的延时时间，设置为0就是最快速度读取。

-   勾选添加内部驱动就是添加设备是否在线和标签更新时间两个标签。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image27.png" width="80%"/>
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image28.png" width="80%"/>


5.  新建标签：右击该连接，新建一个标签。您要采集每个变量都用一个标签来表示。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image29.png" width="80%"/>


-   首先根据要采集的变量确定数据类型。

> 数据类型包括位值，有符号8位整数，无符号8位整数，有符号16位整数，无符号16位整数，有符号32位整数，无符号32位整数，有符号64位整数，无符号64位整数，32位单精度浮点数，64位单精度浮点数，字符串，日期时间，字节数组。其中，有符号64位整数，无符号64位整数，64位单精度浮点数和日期时间是无效的地址类型。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image30.png" width="80%"/>

-   再选择该变量的地址。

> 地址类型包括：输入映像区(I)，输出映像区(Q)，内部标志位(M)，内部数据块(DB)，顺序控制器(S)，特殊标志位(SM),标准计数器(C)，模拟量输入(AI)，模拟量输出(AQ)，高速计数器(HC)。
>
> 其中，输入映像区(I)，输出映像区(Q)，内部标志位(M)，内部数据块(DB)是所有西门子PLC都有的数据区。
>
> 顺序控制器(S)，特殊标志位(SM),标准计数器(C)，模拟量输入(AI)，模拟量输出(AQ)，高速计数器(HC)。都是S7-200才有的数据区。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image31.png" width="60%"/>

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image32.png" width="60%"/>


-   如果该变量需要配置成可写入，即可以在客户端修改变量值，需要将写值使能修改成是。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image33.png" width="80%"/>


-   第一个标签建立成功

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image34.png" width="80%"/>


6.  测试过程中，为西门子PLC1200配置了如下变量，包括：输入映像区(I)，输出映像区(Q)，内部标志位(M)，内部数据块(DB)以及他们分别可选择的数据类型。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image35.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image36.png" width="40%"/>

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image37.png" width="40%"/>


3.3 下载配置到EdgePLUS中
------------------------

1.  在项目------下载配置模块。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image38.png" width="80%"/>


 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image39.png" width="80%"/>
height="2.598611111111111in"}

2.  提示完成。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image40.png" width="80%"/>

3.4 启动MDC_OPCUA_SERVER
------------------------

1.  系统设置------系统------系统信息------进程列表。

> 如果您看到有/opt/scapeak/MDC_OPCUA_SERVER/MDC_OPCUA_SERVER进程，您可以双击重启该进程。
>
> 修改配置变量之后也需要重启进程。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image41.png" width="80%"/>
height="2.967361111111111in"}

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image42.png" width="80%"/>
height="2.6694444444444443in"}

2.  如果您在进程列表中没有看到MDC_OPCUA_SERVER的进程，您需要将MDC_OPCUA_SERVER加入开机启动中。

> 在系统设置------软件管理------开机启动------新建。

-   软件名称：MDC_OPCUA_SERVER

-   软件路径：/opt/scapeak/MDC_OPCUA_SERVER/MDC_OPCUA_SERVER

-   命令行：表示启动该软件所需的命令，MDC_OPCUA_SERVER不需要额外命令，为空。

-   启动延时：0毫秒，表示开机之后间隔多长时间启动该软件，如果想要开机即启动就设置为0毫秒。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image43.png" width="80%"/>

-   配置完成，点击下载配置，就可以将MDC_OPCUA_SERVER设置为开机启动。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image44.png" width="80%"/>


-   然后重启Edge模块，您可以选择断电重启，也可以通过EdgePlant软件选择模块重启，如果重启成功，Edge模块的指示灯POW和SYS常亮。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image45.png" width="40%"/>

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image46.jpeg" width="40%"/>


-   重新搜索模块，您可以在系统设置------系统信息------进程列表看到MDC_OPCUA_SERVER的进程。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image47.png" width="80%"/>

3.5 OPCUA客户端展示
-------------------

1.  读取：在应用软件------数据采集------客户端测试，软件会自动获取服务器地址。

-   点击连接，可以搜索到该PLC1200，点击PLC1200，可以看到配置的变量的数据。

-   点击订阅，数据会实时刷新。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image48.png" width="80%"/>
height="2.0833333333333335in"}

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image49.png" width="80%"/>
height="3.316666666666667in"}

2.  写入：配置变量选择写值使能为是的变量可以写入。例如变量：MB100有符号8位整数，写值使能:是

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image50.png" width="80%"/>


-   您可以用客户端测试写入，或者您开发的OPCUA客户端软件进行写值。示例：将100值改为50。

-   修改方法：直接双击该标签值，写入新值

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image51.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image52.png" width="80%"/>


第4章 变量介绍
==============

4.1 S7-1200数据区域（通用数据区域）说明
---------------------------------------

  变量区域   变量类型            变量地址     读写               说明
  ---------- ------------------- ------------ ------------------ -------------------------------------------------------------------------------------------------------
  I区        位值                I0.0         可读，部分不可写   可读，I区是输入区，在物理影像区，不能写。
             有/无符号8位整数    IB0                             
             有/无符号16位整数   IW0                             
             有/无符号32位整数   ID0                             
             单精度浮点数        ID0                             
             字符串              IB0:1                           
             字节数组            IB0:1                           
  Q区        位值                Q0.0         可读，可写         被PLC程序控制输出的Q区域，写入后会被程序刷新。再次读取仍会读取PLC程序赋予的新值。
             有/无符号8位整数    QB0                             
             有/无符号16位整数   QW0                             
             有/无符号32位整数   QD0                             
             单精度浮点数        QD0                             
             字符串              QB0:1                           
             字节数组            QB0:1                           
  M区        位值                M0.0         可读，可写         作为触点的M区域，可以写入，作为线圈输出的M区域写入后会被程序刷新，再次读取仍会读取PLC程序赋予的新值。
             有/无符号8位整数    MB0                             
             有/无符号16位整数   MW0                             
             有/无符号32位整数   MD0                             
             单精度浮点数        MD0                             
             字符串              MB0:1                           
             字节数组            MB0:1                           
  DB区       位值                DB3.DBX0.0   可读，可写         被PLC程序控制的DB区域，写入后会被程序刷新。再次读取仍会读取PLC程序赋予的新值。
             有/无符号8位整数    DB3.DBB0                        
             有/无符号16位整数   DB3.DBW0                        
             有/无符号32位整数   DB3.DBD0                        
             单精度浮点数        DB3.DBD0                        
             字符串              DB3.DBB0:1                      
             字节数组            DB3.DBB0:1                      

注意：字符串类型和字节数组类型虽然每个区域都可以选择该类型，但是不太常用。例如，DB10中存放了String类型的数据,占用255个字节。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image53.png" width="80%"/>
height="1.611398731408574in"}

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image54.png" width="80%"/>
height="2.2847222222222223in"}

-   字节数组类型：字节数组代表一块数据，不是一个变量，是一块数据，读一块数据区。如从DB10.DBB0到DB10.DBB200。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image55.png" width="80%"/>

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image56.png" width="80%"/>
height="1.3395833333333333in"}

-   字符串类型：例如DB10中存放了String类型的数据'HELLO,SCAPEAK!',字符串类型，写值使能：是。

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image57.png" width="80%"/>
height="4.633333333333334in"}

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image58.png" width="80%"/>
height="1.4152777777777779in"}

> 修改该值

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image59.png" width="80%"/>
height="1.1513888888888888in"}

4.2 S7-200数据区域说明（除通用数据区域之外）
--------------------------------------------

> 注意：S7-200的V区请选择DB块号1。例如：采集VB1，应如下设置。
>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image60.png" width="80%"/>


  变量区域   变量类型            变量地址     读写           说明
  ---------- ------------------- ------------ -------------- --------------------------------------------------------------------------------
  VB区域     位值                DB1.DBX0.0   可读，可写     被PLC程序控制的DB区域，写入后会被程序刷新。再次读取仍会读取PLC程序赋予的新值。
             有/无符号8位整数    DB1.DBB0                    
             有/无符号16位整数   DB1.DBW0                    
             有/无符号32位整数   DB1.DBD0                    
             单精度浮点数        DB1.DBD0                    
             字符串              DB1.DBB0:1                  
             字节数组            DB1.DBB0:1                  
  S区        位值                S0.0         可读,可写      顺序控制继电器S，可读可写。
             有/无符号8位整数    SB0                         
             有/无符号16位整数   SW0                         
             有/无符号32位整数   SD0                         
             单精度浮点数        SD0                         
             字符串              SB0:1                       
             字节数组            SB0:1                       
  SM区       位值                SM0.0        可读，不可写   特殊标志位存储器SM，是由PLC的CPU控制的，有固定含义的存储器，只能读，不能写。
             有/无符号8位整数    SMB0                        
             有/无符号16位整数   SMW0                        
             有/无符号32位整数   SMD0                        
             单精度浮点数        SMD0                        
             字符串              SMB0:1                      
             字节数组            SMB0:1                      
  C区        位值                C0           可读，不可写   计数器由程序控制，不能修改位值。
             有/无符号16位整数   C0计数值                    
  T区        位值                T33          可读，不可写   定时器由程序控制，不能修改位值。
             有/无符号32位整数   T33                         

第5章 PLC基本配置
=================

5.1 PLC基本配置------西门子S7-1200数据采集
------------------------------------------

1.  PLC型号：西门子S7-1200,CPU 1211C DC/DC/DC,6ES7 211-1AE40-0XB0

2.  PLC编程软件：TIA Portal V15

3.  硬件连接：将PLC和计算机用网线连接起来


4.  TIA Portal V15使用方法：


1)  打开软件，创建新项目

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image61.png" width="80%"/>


2)  打开项目视图

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image62.png" width="80%"/>


3)  搜索可访问设备：在线访问------Realtek------更新可访问的设备，就会搜到PLC

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image63.png" width="80%"/>


4)  添加设备，选择对应的PLC型号

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image64.png" width="80%"/>


5)  选择该PLC，双击PLC，配置PLC的IP地址。在属性------常规------PROFINET接口------以太网地址------添加子网------设置PLC的IP地址和子网掩码

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image65.png" width="80%"/>


6)  配置访问权限：属性------常规------防护与安全------访问级别

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image66.png" width="80%"/>


7)  属性------常规------防护与安全------连接机制，允许来自远程对象的访问

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image67.png" width="80%"/>


8)  如果您用到时钟脉冲，需要先开通系统和时钟存储器。脉冲发生器------系统和时钟存储器，启用系统存储器字节，启用时钟存储器字节。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image68.png" width="80%"/>


9)  转至在线：PG/PC接口类型：PN/IE；PG/PC接口：Realtek；接口/子网的连接：PN/IE_1，开始搜索，搜索到该PLC，选中它，转至在线。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image69.png" width="80%"/>


10) 开始编程，程序块------Main(OB块)，写梯形图程序

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image70.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image71.png" width="80%"/>



11) 添加数据块，程序块------添加新块，添加DB数据块

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image73.png" width="80%"/>


12) 右键选择新添加的DB数据块，选择属性

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image74.png" width="80%"/>


13) 在对话框中选择"常规/属性"，将"优化的块访问" 取消勾选，确认修改

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image75.png" width="80%"/>


14) 修改完成，要重新编译（要先编译才能看到偏移量）

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image76.png" width="80%"/>


15) 添加相应寄存器，正确选择数据类型，并下载至PLC中

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image77.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image77.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image78.PNG" width="80%"/>


16) 监控：监控与强制表-监控表_1，添加要监控的变量，就可以对变量进行监控。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image79.png" width="80%"/>


 5.2 PLC基本配置------西门子S7-300数据采集
-----------------------------------------

1.  PLC型号：西门子S7-300，6ES7318-2AJ00-0AB0

2.  PLC编程软件：SIMATIC Manager

3.  硬件连接：将PLC和计算机用网线连接起来

4.  SIMATIC Manager使用方法：


1)  打开软件，PLC-Upload Station to PG，就是将程序从PLC下载到电脑。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image80.png" width="80%"/>


2)  寻找该PLC

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image81.png" width="80%"/>


3)  打开OB块，开始编写梯形图程序

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image82.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image83.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image84.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image85.png" width="80%"/>


4)  添加DB块，Insert-S7 Block-Data Block

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image86.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image87.png" width="80%"/>


5)  将新修改的程序和DB块下载到PLC中。请注意，PLC要在STOP模式下才能下载，也就是将PLC打在STOP位置。

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image88.png" width="80%"/>


6)  如果出现PLC的SF报警，可以将硬件HardWare重新下载

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image89.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image90.png" width="80%"/>


7)  Online/Offline

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image91.png" width="80%"/>


8)  监控

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image92.png" width="80%"/>


 5.3 PLC基本配置------西门子S7-200数据采集
-----------------------------------------

1.  PLC型号：西门子S7-200,CPU 224 AC/DC/RLY

2.  PLC编程软件：STEP7 MicroWin

3.  硬件连接：将PLC和计算机用网线连接起来

4.  STEP7 MicroWin使用方法：


1)  设置PG/PC接口：设置完成点击确定

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image93.png" width="80%"/>


2)  设置通信，与PLC建立通信

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image94.png" width="80%"/>

3)  将程序上载到电脑

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image95.png" width="80%"/>


4)  编写PLC程序，下载到PLC中

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image96.png" width="80%"/>

>
>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image97.png" width="80%"/>


5)  运行PLC程序，监控

>  <img src="https://help.blob.core.chinacloudapi.cn/helppic/plc/image98.png" width="80%"/>

