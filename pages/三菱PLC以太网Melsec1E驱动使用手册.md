1、使用环境
===========

本手册陈诉用例应满足如下配置

  **操作系统**   **Windows 7/8/8.1/10**
  -------------- ------------------------------------------------------------------------------------------------
  **应用软件**   EdgePlant
  **硬件**       EdgePLUS一个、三菱FX系列PLC一个（这里使用FX3U-48MR）、24V电源模块一个、USB线一条、以太网线两条

2、驱动名称
===========

【三菱Q/A/FX系列以太网驱动】

3、硬件连接
===========

3.1 连接方式
------------

【以太网口】连接

3.2 连接步骤
------------

> 将以太网扩展模块通电（24V电压）

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image1.png" width="80%"/> 

> 查询PLC模块的IP地址（FX3U-48MR的IP为192.168.1.220）
>
> 将以太网扩展模块通过以太网线连接到EdgePLUS的Eth1以太网接口（Eth1的IP为192.168.1.118）
>
> 将Eth0以太网接口（Eth0的IP为192.168.10.118）通过以太网线与电脑连起来
>
> USB线插入Debug口与电脑连通

硬件连接实物图如下图所示

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image2.png" width="80%"/>


4、OPCUA的配置
==============

PLC数据区采集说明

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image3.png" width="80%"/>


> 点击【应用软件】——【数据采集】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image4.png" width="80%"/>


> 在【Project Default】中右击新建组别，在【NewGroup\_0】中新建一个连接

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image5.png" width="80%"/> 
<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image6.png" width="80%"/>


> 选择【驱动库】——【三菱】——【Melsec1E.TCP客户端】驱动，修改连接名称和IP地址，此IP地址设置为PLC的IP，点击确认。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image7.png" width="80%"/>


> 在连接下新建标签开始OPCUA配置。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image8.png" width="80%"/>


> 右击添加驱动标签

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image9.png" width="80%"/>


> 修改【标签名称】、【数据类型】、【地址选择】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image10.png" width="80%"/>


举例：配置数据寄存器D0的有符号16位整数。

> 数据类型选择【有符号16位整数】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image11.png" width="80%"/>


> 数据区域选择【数据寄存器D\[0\~511\]】，参考地址选择【0】，勾选上【写值使能】。点击【确认】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image12.png" width="80%"/>


> 配置完标签后，点击【项目】——【下载模块配置】，将配置文件下载入EdgePLUS中。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image13.png" width="80%"/>


> 重启MDC\_OPCUA\_SERVER

点击【系统设置】——【系统信息】——【系统】，点击【进程列表】，双击运行【/opt/scapeak/MDC\_OPCUA\_SERVER/MDC\_OPCUA\_SERVER】，点击【重启进程】完成重启

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image14.png" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image15.png" width="80%"/>


若在“进程列表”中未找到“MDC\_OPCUA\_SERVER”软件，请到“软件管理”中右击“MDC\_OPCUA\_SERVER”添加为开机自启，然后点击“软件列表”，下载软件参数，最后重启模块。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image16.png" width="80%"/>


5、数据读写演示
===============

> 读取OPCUA客户端数据

点击【应用软件】——【数据采集】——【客户端测试】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image17.png" width="80%"/>


点击【连接】——【Project\_Default】——【NewGroup\_0】——【FX3U-48MR】——【D0】——【订阅】，显示读取的D0数据。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image18.png" width="80%"/>


> 修改OPCUA客户端数据

举例：目前读到的D0当前值为0，修改为100

双击D0标签值区域，修改为100，按Enter提交。观察编程软件D0当前值，发现已经由0修改为100，修改成功。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image19.png" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slm1/image20.png" width="80%"/>

