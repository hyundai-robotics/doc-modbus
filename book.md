
[__SOURCE](README.md)
# ${cont_model} 控制器功能手册 - Modbus
[__SOURCE](0-about-this-manual/README.md)
# 关于手册
[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}
[__SOURCE](1-intro/README.md)
# 1. 概述
[__SOURCE](1-intro/1-preparatory-information.md)
# 1.1 准备信息

要理解本手册，需要以下准备信息。

1. **关于操作 ${cont_model} 控制器的知识**
2. **关于 Modbus 协议的知识**
[__SOURCE](1-intro/2-modbus-func.md)
# 1.2 Modbus的功能

{% hint style="warning" %}
\[警告\] 从外部设备、上级控制系统或网络接收到的信号超出了制造商的直接控制范围。用户应承担因这些信号造成的任何故障或事故的全部责任。
{% endhint %}

${cont_model}机器人控制器通过串行和以太网通信支持Modbus主站和从站功能。

### 1. Modbus主站的操作示例

*   **设备控制**

    启用对支持Modbus的设备（例如：抓手）的控制。

![](../_assets/image2.png)

### 2. MODBUS从站的操作示例

*   **操作面板功能**

    一个或多个机器人可以通过支持Modbus的低成本图形面板（GP）通过串行或以太网通信进行连接和使用。

*   **PLC通信**

    通过低成本解决方案启用与具有Modbus主站功能的PLC通信。

*   **基于PC的机器人操作系统**

    您可以构建一个PC基机器人操作系统，以监控或控制机器人的输入/输出信号。

![](../_assets/image3.png)

### 3. 支持方法

| **操作方法** | **串行通信** |   **以太网通信**   |
| :-------: | :--------: | :------------------------------------: |
| 主站的操作 | <p>机器人语言命令 <br>控制器设置 </p> | <p>机器人语言命令 <br>控制器设置</p>               |
|  从站的操作|   控制器设置   | <p>控制器设置</p><p>(可添加3个端口)</p> |

### 4. 传输模式

| **操作方法** |  **串行通信** | **以太网通信** |
| :-------: | :----------------------------------: | :--------: |
| 主站的操作 |               二进制模式              |  二进制模式 |
|  从站的操作 | <p>ASCII模式</p><p>RTU（二进制）模式</p> |  二进制模式 |

### 5. 支持的功能

* 01: 读取线圈（位）
* 02: 读取离散输入（位）
* 03: 读取保持寄存器 
* 04: 读取输入寄存器（多个）
* 05: 写入单个线圈（位）
* 06: 写入单个保持寄存器
* 15: 写入线圈（位）
* 16: 写入保持寄存器（多个）

<br>

### 6. 从站地址

* 从站地址：1-247

<div class="page-break"></div>

### 7. 地址映射

![](../_assets/image_10.png)

*   上表中斜体的大数字字符是用于Modbus的继电器组。

    * MW（用户数据内存）
    * RW（用户辅助内存）
    * KW（用户保持内存）
    * DO（数字输出）
    * SO（系统输出）
    * SI（系统输入）
    * SW（系统内存）
    * Y（输出继电器）
    * X（输入继电器）

*   数据格式

    在浮点格式中，使用的是IEEE单精度32位浮点。对于8位/16位/32位，将使用所有有符号整数。

* 对于继电器的字节序，使用小端序。

    示例：在浮点格式中，dof0=6.515625（0x40D08000）作为示例

    dol0=0x4D08000 -> dow0=0x8000, dow2=0x40D0 -> dob0=0x00, dob1=0x80, dob2=0xD0, dob3=0x40

{% hint style="info" %}
对于Modbus传输，字节序将是16位对齐的大端序。

换句话说，上述传输将按照0x80, 0x00, 0x40, 和0xD0的顺序发生。
{% endhint %}

### 8. SW内存映射

* 这是系统内部定义的信息。欲了解更多详细信息，请参考“嵌入式PLC用户手册(https://hrbook-hrc.web.app/#/view/doc-hi6-embedded-plc/zh/README?cont_model=${cont_model})”。
[__SOURCE](2-modbus-rtu/README.md)
# 2. Modbus 串行

This is about slave or master operation via Modbus serial communication.
[__SOURCE](2-modbus-rtu/1-serial-cable-connect.md)
# 2.1 连接串行电缆

将串行电缆直接连接到COM2端口，如下图所示。

![](../_assets/image7.jpeg)



* 连接器 (D-sub 9pin 母头)

![](../_assets/image5.png)

* 引脚图

![](../_assets/image6.png)
[__SOURCE](2-modbus-rtu/2-serial-port-usage-setting.md)
# 2.2 串行端口使用设置

您可以通过在`[F2: 系统] - 2: 控制参数 - 3: 串行端口 ([F2: System] - 2: Control parameter - 3: Serial port)`屏幕的`[General]`选项卡中应用以下设置，将串行端口的使用设置为Modbus。<br>
`[Modbus]`选项卡已启用。

![](../_assets/image8.png)
[__SOURCE](2-modbus-rtu/3-modbus-environment-setting.md)
# 2.3 Modbus 环境设置

Modbus 的详细信息可以在 `[Modbus]` 标签中设置如下。

![](../_assets/image9.png)

*   `运行 (Operation)`: 选择是以主设备还是从设备的身份运行。

    在作为主设备运行的情况下，从设备 ID 和模式将不被使用，因为执行将由主查询进行。 <br>
    在作为从设备运行的情况下，控制器响应主设备请求，因此仅在屏幕上的设置下操作。 <br> 
* `Slave ID`: 设置作为 modbus 串行通信的从设备的通信 ID。
* `模式 (Mode)`: 设置作为 modbus 串行通信的从设备的通信模式。
[__SOURCE](3-modbus-tcp/README.md)
# 3. Modbus TCP

这是关于modbus tcp从站通信和用于modbus主站操作的以太网通信。有关modbus主站操作，请参阅[4. Modbus master]。
[__SOURCE](3-modbus-tcp/1-network-setting.md)
# 3.1 网络设置

要使用 Ethernet 通信，首先在 `[F2: 系统] - 2: 控制参数 - 9: 网络 - 1: 使用环境设置 ([F2: System] - 2: Control parameter - 9: Network - 1: Environment setting)` 屏幕上设置与连接的 LAN 端口对应的网络地址。

![](../_assets/image31.png)
[__SOURCE](3-modbus-tcp/2-slave-setting.md)
# 3.2 Modbus 从站设置

在 `[F2: 系统] - 2: 控制参数 - 9: 网络 - 2: 服务 - 1: Modbus 从站 ([F2: System] - 2: Control parameter - 9: Network - 2: Service - 1: Modbus slave)` 屏幕上进行设置。<br>
最多可以使用三个从站，使用 "+" 按钮，并且可以监控当前的通信状态。<br>
作为从站运行时，控制器响应主站请求，因此仅根据屏幕上的设置进行操作。<br>

![](../_assets/image30.png)

*   **端口号**

    设置 MODBUS TCP 通信的端口。<br>
    每个从站必须设置为不同的端口号。


*   **超时**

    设置检查 MODBUS TCP 通信连接状态的时间。<br>
    如果在指定时间内没有来自主站的服务请求，连接将被强制终止。


*   **最大连接数**

    设置可以连接到每个从站的主站最大连接数。<br>
    当前最多可以连接三个。
[__SOURCE](3-modbus-tcp/3-enet-comm-setting.md)
# 3.3 以太网通信设置
 
在执行 Modbus TCP 主设备操作之前，必须首先创建和配置以太网通信对象。<br>

在 `[F2: 系统] - 2: 控制参数 - 9: 网络 - 2: 服务 - 4: 以太网通信 ([F2: System] - 2: Control parameter - 9: Network - 2: Service - 4: Ethernet communication)` 屏幕上设置。<br>
最多可以使用 "+" 按钮创建五个以太网对象并使用，可以监控当前通信状态。<br>
由于主设备使用机器人语言命令操作，因此必须单独编写和运行任务程序，而不是从屏幕设置中进行。 (参见 [3.4 Modbus 主设备操作]) <br>

![](../_assets/image32.png)

您可以通过 `[Close]` 按钮强制关闭相应以太网对象的套接字，并通过 `[Connect]` 按钮进行通信连接。<br>
当控制器启动时，它会自动与配置的以太网对象建立通信连接。<br>


*   **名称**

    以太网通信对象的名称。每个名称必须设置为 "enet0" ~ "enet4"。


*   **协议**

    选择通信协议。对于 MODBUS TCP 主设备操作，这必须设置为 "TCPc"（TCP 客户端）。


*   **IP 地址**

    设置从设备使用的 IP 地址。 


*   **本地端口**

    设置本地端口号。Modbus 通信默认使用端口 502。


*   **远程端口**

    设置远程端口号。Modbus 通信默认使用端口 502。


*   **状态**

    显示通信连接的状态。
[__SOURCE](4-modbus-master/README.md)
# 4. Modbus 主站

要操作 Modbus 主站，有两种方法：通过使用机器人语言命令执行，或在设置屏幕中注册查询并执行。
[__SOURCE](4-modbus-master/1-roblang.md)
# 4.1 机器人语言的操作

您可以使用机器人语言语句构建 modbus 主查询并将其发送到从设备。 <br>
当语句被执行时，数据将被传输和接收。 <br>
<br>

#### 语法

```
modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=fb1.diw0  # 串行通信
modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=_mw10  # 以太网通信
```

#### 参数

|参数| 描述                                                                                                    |    示例    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| _sci2 或 _enet0 | <p>串口或以太网对象 (str)</p><ul><li>_sci2 : 串口 2</li><li>_enet0 : 以太网对象 0</li></ul>                                         | "_sci2" 或 "_enet0" |
| sid  | 从设备 ID(1~247) (int)                                                      | 1 |
| fc | 功能代码 (int)，参考 1.2.5. 支持的功能  | 3 / 16 |
| addr  | 从设备的起始地址 (0~65534) (int)                                                       | 0 |
| len  | 数据的数量 (字 : 1~127, 位 : 1~2000) (int)                                                     | 10 |
| wait  | 超时 (秒) (double)，如果未指定，则无限等待                                                       | 3.0 |
| var  | 整数类型的数组变量，数据内存(_mw0)，输入信号(fb2.diw0)，输出信号(fb3.dow0)                                                       | arr / _mw0 / fb1.diw0 |

<br>

#### 示例职位


``` python
Hyundai Robot Job File; { version: 2.0, mech_type: "", total_axis: -1, aux_axis: -1 }
     var arr # 本地变量定义
     arr=Array(20) # 定义为 int 类型的数组变量
     modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr # 从地址 0 读取 10 个数据并将其分配给 arr 变量
     print arr[0] # 打印 arr 变量的 0 号索引值
     delay 2 # 时间延迟
     arr[0]=arr[0]+1 # 将 1 加到 arr 变量的 0 号索引值
     modbus _enet0,sid=1,fc=16,addr=0,len=10,wait=3.0,var=arr # 将 arr 变量的值写入地址 0 的 10 个数据
     delay 2 # 时间延迟
     end

```
[__SOURCE](4-modbus-master/2-setup.md)
# 4.2 查询设置操作

当用户设置 modbus 主站查询时，可以根据此自动传输到从站。<br>
在执行配置的查询时会发生数据传输和接收。<br>
<br>

在 `[F2: 系统] - 2: 控制参数 - 9: 网络 - 2: 服务 - 5: Modbus 主站 ([F2: System] - 2: Control parameter - 9: Network - 2: Service - 5: Modbus master)` 界面上设置。<br>
您可以使用 "+" 按钮添加主站。<br>

![](../_assets/image33.png)

您可以通过 `[stop]` 按钮强制停止主站执行，并通过 `[execute]` 按钮重新执行。<br>
您可以使用 [Query Setup] 按钮配置查询。<br>
控制器启动后，设置的查询会自动按顺序执行。<br>

*   **名称**

    主对象的名称。每个名称必须设置为 "master_?"。

*   **通信类型**

    选择通信方式：以太网通信或串行通信。

*   **对象编号**

    对于以太网通信，设置在 [3.3 Ethernet Communication](../3-modbus-tcp/3-enet-comm-setting.md) 中设置的 enet 编号。<br>
    对于串行通信，目前只有 2 个可用。

*   **状态**

    当前运行查询的执行状态通过配置机器人语言命令显示。如果操作被停止，将显示“Stopped”。

<br>
<br>

![](../_assets/image34.png)

您可以使用 [append] 按钮添加新的查询，并使用 [Delete] 按钮删除相应的查询。<br>

*   **从站 ID**

    设置从站 ID(1~247)。

*   **功能**

    设置功能代码。您可以使用 [function info.] 按钮检查支持的功能。

*   **起始地址**

    设置从站的起始地址 (0~65534)。

*   **长度**

    设置数据的数量 (字 : 1~127, 位 : 1~2000)。

*   **超时**

    设置超时时间（以秒计）。将其设置为 0 将无限期等待。

*   **继电器名称**

    设置继电器（数据内存/输入/输出信号等）名称。

*   **延迟时间**

    设置当前查询执行与下一个查询执行之间的延迟时间（毫秒）。