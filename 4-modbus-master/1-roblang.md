# 4.1 机器人语言操作

您可以使用机器人语言语句构造 Modbus 主查询并将其发送给从机。<br>
当语句执行时，数据会被传输和接收。<br>
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
| sid  | 从机 ID(1~247) (int)                                                      | 1 |
| fc | <p>功能代码 (int)</p><ul><li>3 : 读取保持寄存器</li><li>16 : 写入保持寄存器</li><li>4 : 读取输入寄存器</li></ul>                                         | 3 / 16 / 4 |
| addr  | 从机的起始地址 (0~65534) (int)                                                       | 0 |
| len  | 数据量 (1~127) (int)                                                     | 10 |
| wait  | 超时间隔（秒） (double)，如果未指定，则无限等待                                                       | 3.0 |
| var  | 整数类型的数组变量，数据内存(_mw0)，输入信号(fb2.diw0)，输出信号(fb3.dow0)                                                       | arr / _mw0 / fb1.diw0 |

<br>

#### 示例任务


``` python
Hyundai Robot Job File; { version: 2.0, mech_type: "", total_axis: -1, aux_axis: -1 }
     var arr # 本地变量定义
     arr=Array(20) # 定义为整数类型的数组变量
     modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr # 从地址 0 读取 10 个数据并将它们分配给 arr 变量
     print arr[0] # 打印 arr 变量的第 0 个索引值
     delay 2 # 时间延迟
     arr[0]=arr[0]+1 # 将 arr 变量的第 0 个索引值加 1
     modbus _enet0,sid=1,fc=16,addr=0,len=10,wait=3.0,var=arr # 将 arr 变量值写入地址 0 的 10 个数据
     delay 2 # 时间延迟
     end

```