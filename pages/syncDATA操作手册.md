数据库同步使用手册（syncDATA）
============

一、设备连接
----------------------

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image1.jpeg" width="80%"/>



EdgePLUS通过网线分别连接PC和PLC，EdgePLUS通过USB接口连接PC通电。

二、使用流程及相关配置
----------------------

1.首先进入网路配适器，如下图：

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image2.png" width="80%"/>



其中1为路径（可通过cortana直接搜索），进入以太网配置PC的IP。

2.点击属性（1），然后选择Internet协议版本4，设置其为使用下面的IP地址，IP地址可以自己设置。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image3.png" width="80%"/>



3.打开EdgePlant，选择以太网端口连接EdgePLUS，在下图的步骤3中进行Eth1和Eth0网口的配置，将PLC和EdgePLUS，PC和EdgePLUS分别放在同一网段下。（连接PLC和PC的网口）

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image4.png" width="80%"/>



点击应用软件中的数据采集，会显示下图信息，在3中的目录下可以创建新的节点用来存储需要的PLU的相关数据（同步的数据），4是PLC的IP，可自行设置（注意要和EdgePLUS在同一网段下）。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image5.png" width="80%"/>
 

然后通过系统设置中的软件管理进行软件安装，这里需要安装的是下面的MES\_DATA\_SERVER（数据存储）和syncDATA\_MsSQL（数据同步）。（注意：syncDATA\_MsSQL的安装路径为/opt/scapeak/sync\_DATA\_MsSQL，M32ES\_DATA\_SERVER的安装路径为/opt/scapeak/MES\_DATA\_SERVER）。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image6.png" width="80%"/>
 

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image7.png" width="80%"/>



安装完后，在系统信息的软件列表进行列表更新，然后在4.数据存储中可以看到安装的软件。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image8.png" width="80%"/>
 

软件安装完后，在EdgePlant中进行下图的步骤1，下载模块配置，再进行客户端测试，将其中的服务器地址和选择的标签id的信息复制下来。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image9.png" width="80%"/>



然后再选择下图中的1进行MES_DATA_SERVER的配置，在2中进行数据库和数据表，还有数据表节点的创建,注意：在创建节点时， **要创建一个自增主键ID，名称改为ID，自增主键ID必须放在第一个节点的位置，**其他的信息都不用更改。还有一个固定数值字段，名称为syncFlag，字段声明为INT，数据格式符为0，字段数据源为数据格式符，这个节点的位置不用固定。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image10.png" width="80%"/>



数据表类型一般使用定时记录表，并且自动创建和自动删除都选择“是”。

<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image11.png" width="80%"/>

然后进入应用软件修改上图1中的数据库同步软件配置，在2中SourceDb标签中设置源数据库的信息（要被同步的数据库）。    
DbType：源数据库（默认MYSQL），DbName：数据库名称，User：默认为scaedge，Password：默认为scapeak，Host：默认为127.0.0.1，SyncMode：软件执行模式（insert（存储）或者update（更新）），Port：端口号，在DeatDb标签中设置目标数据库的信息（数据同步到的目标数据库）。DbType：：目标数据库（MYSQL或者MSSQL），DeleteOldTable：是否在启动时删除原来的表。


<img src="https://help.blob.core.chinacloudapi.cn/helppic/syncdata/image12.png" width="80%"/>



然后回到系统设置的软件管理，鼠标右键1中的内容，将其加入开机启动（2中）（注意：一次只能将一个软件加入开机启动，加入一个软件后需要点击2中的“下载配置”），将软件加入开机启动后，点击3的重启模块，完成设置。
