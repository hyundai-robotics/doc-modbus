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