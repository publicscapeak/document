

![image-20200729090535309](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729090535309.png)

SCAEdge驱动使用手册\__Beta_
====
|  适用| Mitusubishi FX/Q 、SIEMENS、Schneider、OMRON、发那科、光洋Koyo |
| -------- | ------------------------ |
| 发布者   | IIOT物联网部门          |
| 发布日期 | Aug/2020    |
| 发布版本 | V1.5                    |
| 公司网址 | www.scapeak.com          |
<div STYLE="page-break-after: always;"></div>

[TOC]

<div STYLE="page-break-after: always;"></div>

引言
===

本手册为Edge模块的用户使用手册，介绍将Edge模块投入运行所需信息。Edge模块内部为嵌入式ARM-Linux系统，因此具备Linux相关知识将有助于更深入的使用Edge模块。
Edge嵌入式软件的版本迭代和功能描述不会立即反映在本手册中，具体需求请垂询本公司服务热线400-8544-418。
[引用]





# 使用环境

* 本手册陈述用例应满足如下配置

| 操作系统 | Windows 7/8/8.1/10        |
| -------- | -------------------------|
| 应用软件 | EdgePlant                 |
| 硬件     | PC、PLC、Power&Data Cable |

<div STYLE="page-break-after: always;"></div>

# 数据采集服务-MDC.OPCUA.SERVER

## 1.FX系列
略


## 2.Q系列

### 如何开始1个新工程？

* 新建1个组别 默认第1个组别的名称是 _NewGroup_0_
* _组别名称_ 可自定义，例如本例以Mitsubishi的Q06HCPU示范，可更名为 __Q06H__

* ![image-20200729140853084](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729140853084.png)





### MelsecUSB

#### 硬件连接及通讯参数配置

* 硬件连接实物图
如下图所示，Q06HCPU与EdgePLUS通过USB线缆建立物理连接。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731134611206.png" alt="image-20200731134611206" style="zoom: 50%;" />



* 新建1个连接
* 选择 _三菱Melsec.USB通讯_  
* 勾选 _内部驱动标签_ ，点击确认
* _连接名称_ 更名为 __MelsecUSB__
    ![image-20200729101118992](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729101118992.png)
* 配置USB接口
* 如图示，选择图示位置 _驱动参数_  _设备位置_  __功能按钮__
    ![image-20200729101416160](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729101416160.png)
* 点击 __更新__，展开列表，可显示当前已连接至EdgePLUS并识别的PLC
* 点击图示 MITSUBISHI ，单击 **确定** ，即可自动适应并完成USB配置
     <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729102040754.png" alt="image-20200729102040754" style="zoom: 67%;" />
#### USB可扩展功能

EdgePLUS的USB驱动支持USB集线器，因此用户可通过外接USB集线器，实现对多个PLC访问。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731170203188.png" alt="image-20200731170203188" style="zoom:50%;" />

#### 配置标签采集指定变量
* 新建1个标签

    <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/屏幕截图(577).png" alt="屏幕截图(577)" style="zoom:67%;" />
* 欲读取PLC中的D0，将_标签名称_手动输入为__D0__
* 选择_数据类型_为__有符号16位整数__
* 在弹出框中保持如图所示配置
  * 数据区域 **数据寄存器|D**
  * 参考地址 **0** ==（输入框的数值进制为十进制）==
  * 勾选**写值使能** ，对PLC允许写入的软元件可通过EdgePLUS的OPCUA服务实现写值。
    ![image-20200729141804153](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729141804153.png)
* 依次点击**项目** **下载模块配置** ，即可将当前配置烧写进模块
     <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/屏幕截图(579).png" alt="屏幕截图(579)" style="zoom:75%;" />
* 依次操作如图所示步骤，找到进程列表中的MDC_OPCUA_SERVER进程并鼠标左键双击，点击**重启进程**，待进程重启完成后本次配置生效
    <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200729171750016.png" width="80%"/> 
  
* 至此，EdgePLUS可通过MelsecUSB读写PLC中的D0。





### Melsec4C

#### 硬件连接及通讯参数配置

* 硬件连接实物图
<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731141227762.png" alt="image-20200731141227762" style="zoom: 50%;" />
  * 本例使用了EdgePLUS的COM1（多模式串口）
* PC通过GX Works2 对PLC编程的端口配置
<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731142232850.png" alt="image-20200731142232850" style="zoom:50%;" />
* 使用EdgePLUS的COM1（多模式串口）采集PLC数据的端口配置参数应与上图一致。

| Options    | Value      | Remark |
| ---------- | ---------- | ------ |
| 波特率     | BPS_115200 |        |
| 数据位长度 | CS_8       |        |
| 校验       | ODD        | 奇校验 |
| 停止位       | 1        |        |



 <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731141911032.png" alt="image-20200731141911032" style="zoom: 50%;" />
* 配置Melsec4C驱动选择COM1
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731141534425.png" alt="image-20200731141534425" style="zoom: 50%;" />


* 至此，Melsec4C通讯配置完成，标签配置请参照前文**MelsecUSB**->**配置标签采集指定变量**





### Melsec3E

#### 通过QJ71E71-100 以太网模块进行3E帧通讯

##### 硬件连接及通讯参数配置

* 硬件连接实物图

  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731154026816.png" alt="image-20200731154026816" style="zoom:50%;" />
  
    * PLC的以太网模块通过网线与EdgePLUS的Eth1建立物理连接

###### 例1，PLC IP：192.168.1.101/24

* 通讯参数
  * 本例PLC的以太网模块相关配置参数

  | Options  | Value            | Remark                           |
  | -------- | ---------------- | -------------------------------- |
  | 网络类型 | 以太网           |                                  |
  | IP地址   | 192.168.1.101/24 | "/24"，即子网掩码为255.255.255.0 |
  | 网络号   | 1                |                                  |
  | 站号     | 3                | PLC的站号是3                     |
  | 协议     | TCP              | 推荐设置                         |
  | 打开方式 | MELSOFT连接      | 推荐设置                         |
  
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731154911645.png" alt="image-20200731154911645" style="zoom:50%;" />
  
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731160153843.png" alt="image-20200731160153843" style="zoom:50%;" />
  
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200813164146504.png" alt="image-20200813164146504" style="zoom:50%;" />
  
  
  
  * EdgePLUS应保持的配置参数
  
  | Options  | Value         | Remark                                       |
  | -------- | ------------- | -------------------------------------------- |
  | IP地址   | 192.168.1.101 | 即PLC的以太网模块地址                        |
  | 网络编号 | 01            | 此处输入框数据的进制为16进制                 |
  | PLC编号  | 03            | 此处输入框数据的进制为16进制                 |
  | IO编号   | FF.03         | 通讯协议默认                                 |
  | 站编号   | 02            | EdgePLUS的站号，此处输入框数据的进制为16进制 |

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731161058922.png" alt="image-20200731161058922" style="zoom:50%;" />

* 至此，Melsec3E通讯配置完成，标签配置请参照前文**MelsecUSB**->**配置标签采集指定变量**

###### 例2，PLC IP：192.168.10.222/24

* 通讯参数
  * 本例PLC的以太网模块相关配置参数

  | Options  | Value             | Remark                           |
  | -------- | ----------------- | -------------------------------- |
  | 网络类型 | 以太网            |                                  |
  | IP地址   | 192.168.10.222/24 | "/24"，即子网掩码为255.255.255.0 |
  | 网络号   | 10                |                                  |
  | 站号     | 3                 | PLC的站号是3                     |
  | 协议     | TCP               | 推荐设置                         |
  | 打开方式 | MELSOFT连接       | 推荐设置                         |
  
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200813094226637.png" alt="image-20200813094226637" style="zoom:50%;" />
  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731160153843.png" alt="image-20200731160153843" style="zoom:50%;" />
  
  * EdgePLUS应保持的配置参数
  
  | Options  | Value          | Remark                                             |
  | -------- | -------------- | -------------------------------------------------- |
  | IP地址   | 192.168.10.222 | 即PLC的以太网模块地址                              |
  | 网络编号 | 0A             | 0A(H)即10(D),此处输入框数据的进制为16进制          |
  | PLC编号  | 03             | 此处输入框数据的进制为16进制                       |
  | IO编号   | FF.03          | 通讯协议默认                                       |
  | 站编号   | 02             | 自定义EdgePLUS的站号，此处输入框数据的进制为16进制 |

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200813094605764.png" alt="image-20200813094605764" style="zoom:50%;" />

* 至此，Melsec3E通讯配置完成，标签配置请参照前文**MelsecUSB**->**配置标签采集指定变量**

#### 通过内置以太网口进行3E帧通讯

* 说明

本驱动适用的PLC若选用内置以太网口进行3E通讯，由于其上位机软件无“站号”等相关参数配置选项，请根据目标PLC通讯协议设定。

* 硬件连接实物图

略

* 本例PLC的应保持的配置

| Options    | Value      | Remark |
| ---------- | ---------- | ------ |
| IP地址  | 192.168.1.130 | 自定义 |
| 协议 | TCP    |        |
| 打开方式   | MC协议    |  |
| 本站端口号  | 5200    | 自定义 |



<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200818164200127.png" alt="image-20200818164200127" style="zoom:50%;" />

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200818164051664.png" alt="image-20200818164051664" style="zoom:50%;" />

* EdgePLUS应保持的配置参数

| Options  | Value         | Remark                    |
| -------- | ------------- | ------------------------- |
| IP地址   | 192.168.1.130 | 即PLC的内置以太网口IP地址 |
| 网络编号 | 00            | 推荐缺省值                |
| PLC编号  | FF            | 推荐缺省值                |
| IO编号   | FF.03         | 通讯协议默认              |
| 站编号   | 00            | 推荐缺省值                |

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200818164703221.png" alt="image-20200818164703221" style="zoom:50%;" />


### Melsec1E
#### 硬件连接及通讯参数配置
* 硬件连接实物图

  <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731154026816.png" alt="image-20200731154026816" style="zoom:50%;" />

* 通讯参数配置

  | Options | Value         | Remark                 |
  | ------- | ------------- | ---------------------- |
  | IP地址  | 192.168.1.101 | 即PLC的以太网模块地址  |
  | PLC编号 | FF            | 通讯协议规定的固定参数 |
  

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200731163155044.png" alt="image-20200731163155044" style="zoom:50%;" />

* 至此，Melsec1E通讯配置完成，标签配置请参照前文**MelsecUSB**->**配置标签采集指定变量**

### 软元件的允许访问范围



#### 1.通过QnA兼容3E/3C/4C/4E帧通信

* 基本型CPU

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092354427.png" alt="image-20200803092354427" style="zoom:67%;" />

* 高性能型QCPU、过程CPU、冗余CPU、QnACPU

 <img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092446486.png" alt="image-20200803092446486" style="zoom:67%;" />

* 通用型QCPU、LCPU

<img src="https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092520473.png" alt="image-20200803092520473"  />

* 安全CPU

 ![image-20200803092552317](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092552317.png)



#### 2.通过QnA兼容1E帧通信

* 软元件列表(无限制CPU 模块)

![image-20200803092854532](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092854532.png)

![image-20200803092932258](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803092932258.png)

* 软元件列表(带限制CPU 模块)
* ![image-20200803093024416](https://help.blob.core.chinacloudapi.cn/helppic/scaedge/image-20200803093024416.png)

# FAQ

* Melsec1E协议不支持访问三菱Q系列PLC的ZR软元件
* 数据类型  **日期时间** 为缺省状态，此手册对应的SCAEdge不支持此数据类型

