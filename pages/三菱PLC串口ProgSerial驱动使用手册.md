1、使用环境
===========

本手册陈诉用例应满足如下配置

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image1.png" width="80%"/>


2、驱动名称
===========

【FX系列编程口通讯】

3、硬件连接
===========

3.1连接方式
-----------

【串口】连接

3.2连接标准
-----------

三菱Fx的串口连接标准有【RS-232】、【RS-485】

3.3连接步骤
-----------

> 取下三菱PLC串口扩展模块

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image2.png" width="80%"/>


> 观察串口扩展模块的连接标准

FX3U-48MR的串口连接标准为【RS-232】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image3.png" width="80%"/>


> 将通讯线与接线端子连接，并插入串口扩展模块中（如果是九针接线端子，接2，3，5口）

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image4.png" width="40%"/>
<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image5.jpeg" width="40%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image6.png" width="80%"/>


> 根据EdgePLUS接线说明，通讯线连接EgdePLUS的接线端子，通讯线接线与PLC一一对应。
>
> <img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image7.png" width="80%"/>
> 

注：EdgePLUS的COM1与COM2口是两个RS232/485可切换的串口，COM3和COM4是两个RS232串口。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image8.jpeg" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image9.png" width="80%"/>


[]{#_Hlk47441176 .anchor}硬件连接实物图如下图所示

> []{#_Hlk47084454 .anchor}
> USB线插入Debug口与电脑连通，以太网线插入Eth0口（ip地址：192.168.10.118）与电脑连通

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image10.jpeg" width="80%"/>


4、通讯参数配置
===============

> 修改接口配置，连接成功硬件后，点击EdgePlant.exe，选择接口配置

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image11.png" width="80%"/>


> 选择【串口】——【单模式串口】——【COM4】（根据EdgePLUS硬件串行端口设置），根据PLC的【数据位长度】、【校验】、【停止位】和【波特率】对应配置
>
> <img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image12.png" width="80%"/>
> 

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image13.png" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image14.png" width="80%"/>


> 修改完串口配置文件，点击【配置】——【下载模块配置】进行保存.

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image15.png" width="80%"/>


5、配置标签采集指定变量
=======================

> PLC数据区采集说明
>
> <img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image16.png" width="80%"/>
> 
>
> 点击【应用软件】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image17.png" width="80%"/>


> 在【Project Default】中右击新建组别，在【NewGroup\_0】中新建一个连接

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image18.png" width="80%"/> 
<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image19.png" width="80%"/>


> 选择【驱动库】——【三菱】——【FX系列编程口通讯】驱动，修改连接名称和通讯接口，点击确认。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image20.png" width="80%"/>


> 在连接下新建标签开始OPCUA配置

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image21.png" width="80%"/>


> 右击添加驱动标签

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image22.png" width="80%"/>


> 修改【标签名称】、【数据类型】、【地址选择】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image23.png" width="80%"/>


举例：配置数据寄存器D0的有符号16位整数。

> 数据类型选择【有符号16位整数】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image24.png" width="80%"/>


> 数据区域选择【数据寄存器D\[0\~511\]】，参考地址选择【0】，勾选上【写值使能】。点击【确认】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image25.png" width="80%"/>


> 配置完标签后，点击【项目】——【下载模块配置】，将配置文件下载入EdgePLUS中

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image26.png" width="80%"/>


> 重启MDC\_OPCUA\_SERVER

点击【系统设置】——【系统信息】——【系统】，点击【进程列表】，双击运行【/opt/scapeak/MDC\_OPCUA\_SERVER/MDC\_OPCUA\_SERVER】，点击【重启进程】完成重启

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image27.png" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image28.png" width="80%"/>


若在“进程列表”中未找到“MDC\_OPCUA\_SERVER”软件，请到“软件管理”中右击“MDC\_OPCUA\_SERVER”添加为开机自启，然后点击“软件列表”，下载软件参数，最后重启模块

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image29.png" width="80%"/>


6、数据读写演示
===============

> 读取OPCUA客户端数据

点击【应用软件】——【数据采集】——【客户端测试】

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image30.png" width="80%"/>


点击【连接】——【Project\_Default】——【NewGroup\_0】——【FX3U-48MR】——【D0】——【订阅】，显示读取的D0数据。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image31.png" width="80%"/>


> 修改OPCUA客户端数据

举例：目前读到的D0当前值为0，修改为100

双击D0标签值区域，修改为100，按Enter提交。观察编程软件D0当前值，发现已经由0修改为100，修改成功。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image32.png" width="80%"/>


<img src="https://help.blob.core.chinacloudapi.cn/helppic/slprog/image33.png" width="80%"/>

