# 模块供电

>EdgePLUS可以使用24V直流电源供电，提供24V接线端子，或者使用5V电源，可以用电脑供电，USB线插在Debug口。一般推荐使用24V接线端子供电更加稳定。

# 设备连接

>1. 若PLC有网口，使用以太网直接连接：
一般用到西门子PLC、EdgePLUS、计算机和网线。EdgePLUS有两个网口。例如：按照图示连接硬件。

其中，Edge有两个网口，分别为Eth0和Eth1，这两个网口的可由用户设置IP地址，但是不能设置为同一网段。如果像图2-1一样连接，则PLC需要和Eth0在同一网段；计算机需要和Eth1在同一网段。  
例如：根据PLC的IP地址和计算机IP地址修改Edge的网口配置。

```设备	网口	网口
PLC	192.168.215.1	
计算机		192.168.1.200
EdgePLUS	Eth0:192.168.215.2	Eth1:192.168.1.118
```
      
>2. PLC有网口，使用交换机连接
如果您用交换机连接硬件，Edge只使用一个网口  
例如：Eth1,可以如下图所示进行连接。IP地址配置如下： 
```设备	网口	网口
PLC		192.168.1.201
计算机		192.168.1.200
EdgePLUS	Eth0:任意（不使用）	Eth1:192.168.1.118
```

- 硬件连接：  
 ![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=702257389,1274025419&fm=27&gp=0.jpg "区块链")

>3. PLC没有有网口，使用SCANET串口转网口
如果您的西门子PLC没有网口，则需要使用我们的SCANET模块将串口转成网口。  
SCANET为西门子SIMATIC自动化控制系统的以太网信息化数据采集提供一个统一的平台，在这里只做简单介绍。  
具体介绍请查看公司官网 (https://www.scapeak.com/gongyewulian/SCANET/80.html) 。

1. 型号：

 - S7PPI 型号应用于 SIMATIC-S7 的 S7-200、200 SMART 等 PLC 控制系统的以太网通讯，支持 PPI 和 MPI 总线； 
 - S7MPI 型号应用于 SIMATIC-S7 全系列 PLC 控制系统（包括 S7-200/300/400）以及 SINUMERIK 数控系统，支持 PPI、MPI 和 PROFIBUS 总线。
2. 硬件连接：

 - 将SCANET模块的九针公口直接插入到控制器的通讯母口（PPI、MPI 和 PROFIBUS 通讯口）即可；
 - 模块会自动从PLC通讯口获得24VDC 工作电源。如果PLC的通讯口不提供24V电源输出，则需要外接24V电源。
 - 当SCANET插入到 PLC 后会自动锁定PLC通讯口的波特率，支持最高 6Mbps。
3. 指示灯：

 - SCANET共有四个指示灯，位于面板的红色电源指示灯PWR和绿色总线指示灯 BUS，以及位于以太网插座的绿色Link指示灯和黄色Active指示灯。
 - 红色电源指示灯PWR：在SCANET插入到上电运行的PLC的通讯口时红色电源指示灯应常亮；如果不亮则可能是 PLC 通讯口无电源或者SCANET模块的电源部分损坏或者指示灯损坏；检查办法是用一个24VDC电源连接到SCANET外部供电端子，如果电源指示灯仍不亮则说明SCANET模块故障； 
 - 绿色总线指示灯BUS：当SCANET插入PLC后应在1-3秒内变为常亮，常亮代表SCANET自动锁定了PLC 通讯口的波特率；  
     - 如果闪烁则需要分辨以下情况：

     a)以一秒间隔持续闪烁：   
     代表总线地址冲突，SCANET模块的自身S7总线站地址默认为0，如果总线上还有其他站点地址为0，则可能出现此现象，请先将其他站点从S7总线上断开；后续通过SCANETPRO软件修改SCANET的站地址后再将其他设备接入S7总线；  

     b)连续闪烁两次后约5秒左右再连续闪烁两次,代表SCANET无法锁定PLC的通讯口波特率。  
     有三种可能性：  
     1. SCANET通讯器件故障；  
     2. 二是PLC通讯口故障；  
     3. PLC通讯口被设置为自由口通讯；
     >对于第一第二种情况，可以尝试用编程电缆来测试；  
     第三种情况通常为S7-200，请将PLC设置为STOP状态（当S7-200处于STOP模式时其通讯口为PPI协议）；以太网绿色连接指示灯 Link：当SCANET接入以太网络时Link指示灯常亮； 以太网黄色通讯指示灯Active：当上位机和SCANET无通讯时偶尔闪亮（网络查询），当上位机和 SCANET通讯时持续闪烁；  

4. SCANET配置：  
    - 方法一：在浏览器输入SCANET的IP地址。新的SCANE默认IP地址为192.168.1.188。如果您修改了IP地址，并且忘记了请采用方法二。  
    a)首页： SCANET基本信息，不做修改。  
    b)S7总线接口参数：修改S7通讯协议模式和总线波特率。您可以按照默认设置，不做修改。  
    c)以太网参数：
    修改IP地址，请查看PLC有网口时的设置，该IP地址相当于PLC的IP地址。
    通讯目标PLC地址由槽号决定：
    否，该模式下DSAP不起作用，SCANET总是连接到默认的MPI地址。一般一个SCANET连接一个PLC设置为否。
    是，多用于连接CNC，该模式下上位机设置DSAP的第二个字节值为MPI站号。
    
   - 方法二：使用SCANET配置软件  
   a)搜索SCANET模块：  
   b)修改IP地址：  
   c)修改通讯接口：  
   d)修改PLC通讯地址是否由槽号决定：  
      >否，该模式下DSAP不起作用，SCANET总是连接到默认的MPI地址。  
       是，多用于连接CNC，因为CNC中的PLC和NCK都要通讯，就必须由槽号（通过DSAP）决定。该模式下上位机设置DSAP的第二个字节值为MPI站号。  
       
       e)通讯接口模块诊断  
       f)以太网服务器模块诊断：可查看有几个上位机与PLC通讯。
