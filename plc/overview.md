# 概述

>边缘计算网关Edge模块主要执行数据收集和现场级数据策略,详情请查看公司官网关于Scaedge的介绍 https://www.scapeak.com/ 。

# 版本

- 西门子PLC以太网驱动OPCUA_SimaticTcpClient(基础版)
- OPCUA_ SimaticTcpClient_Advance(高级版)

安装在Edge模块内的通过OPCUA服务器采集西门子SIMATIC-S7系列的PLC数据的驱动。
**适用于西门子SIMATIC-S7系列的PLC，常见的有S7-200/300/400，SMART200，S7-1200，S7-1500**

# 软件介绍
>- 软件名称：MDC_OPCUA_SERVER（一般出厂默认安装）
>- 软件说明：OPCUA数据采集服务
>- 软件文件夹路径：/opt/scapeak/MDC_OPCUA_SERVER
>- 软件路径：/opt/scapeak/MDC_OPCUA_SERVER/MDC_OPCUA_SERVER

Edge模块内置凌顶自主开发的 OPCUA 服务器MDC_OPCUA_SERVER，可以集成各种数据采集的驱动，例如西门子PLC以太网驱动。

# 驱动介绍


1. **驱动名称：OPCUA_SimaticTcpClient（一般出厂默认安装）:**

    1. 驱动说明：OPCUA驱动西门子PLC以太网(基本版)
    2. 驱动文件夹路径：/opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient
    3. 驱动路径：/opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient/libSimaticTcpClient.so

2. **驱动名称：OPCUA_ SimaticTcpClient_Advance（一般出厂默认安装）**

    1. 驱动说明：OPCUA驱动西门子PLC以太网(高级版)
    2. 驱动文件夹路径: /opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient_Advance
    3. 驱动路径：opt/scapeak/MDC_OPCUA_SERVER/protocols/SimaticTcpClient_Advance/libSimaticTcpClient_Advance.so


>其中，西门子PLC以太网(基本版)和西门子PLC以太网(高级版)的驱动都可以采集数据，区别在于基础版会连续读地址错误的变量，因此当PLC的变量地址有效时会立即读出数据，OPC不需要重启。而高级版为了更快的读取数据，在启动时会读取一次所有变量的值，剔除无效变量后再进行变量地址合并优化后再连续通讯。因此高级版不会读取无效变量，因此当PLC的变量地址有效时也不予理会，需要重启MDC_OPCUA_SERVER才行。
例如：DB100对于PLC来说是无效变量，基础版每次仍会读取该变量，高级版第一次读取该变量无效时会剔除它，之后不会再读，相比基础版效率更高。但是如果您重新编写PLC程序后，DB100变成有效变量，基础版会直接读出该数值，而高级版因为已经剔除该变量就不会读取到，需要重启MDC_OPCUA_SERVER。

# 适用类型
1. **S7-200系列：**

    1. 该系列的CPU本体已集成PPI/MPI通讯口，可采用SCANET模块将其转换为以太网通讯。（SCANET模块）
    2. 如果有EM277扩展通讯模块，其有MPI通讯口，采用SCANET模块将其转换为以太网通讯。
    3. 如果有扩展CP243-1以太网模块，可直接通讯。

2. **SMART200系列：**

    1. 该系列CPU本体已集成MPI通讯口，采用SCANET模块将其转换为以太网通讯。
    2. 该系列CPU本体也集成以太网通讯口，可直接通讯。

3. **S7-300系列：**

    1. 该系列的CPU本地已集成MPI通讯口，可采用SCANET模块将其转换为以太网通讯。
    2. 如果有CP343-1以太网扩展模块，可直接通讯。

4. **S7-400系列：**

    1. 该系列的CPU本地已集成MPI/PROFIBUS通讯口，可采用SCANET模块将其转换为以太网通讯。
    2. 如果有CP443-1以太网扩展模块，可直接通讯。

5. **S7-1200/S7-1500系列：**

    1. 该系列CPU已集成以太网通讯口，可直接通讯。
    2. 注意：驱动不能读写优化DB块，要取消优化块访问。该系列的PLC内部的数据块（DB）有两种，一种是标准DB块，一种是优化DB块。优化DB块是无法通过数据地址进行访问的。(详细步骤可查看S7-1200测试示例)

    ![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=702257389,1274025419&fm=27&gp=0.jpg "区块链")

    3. 注意：DB块需要开放读/写权限。

    ![blockchain](https://ss0.bdstatic.com/70cFvHSh_Q1YnxGkpoWK1HF6hhy/it/u=702257389,1274025419&fm=27&gp=0.jpg "区块链")


