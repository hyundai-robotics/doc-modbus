# 3.1.1 Command (Modbus)

This statement is for performing Modbus master communication in robot language.

Please learn and study this separately for understanding Modbus communication.

#### <mark style="color:green;">Syntax</mark>

```
modbus enet2,sid=65,fc=16,addr=0,len=3,wiat=3,var=arr
```

#### <mark style="color:green;">Parameter</mark>

| enet2 | Object of communication (Ethernet or Serial)                                                                    |           |
| :---: | --------------------------------------------------------------------------------------------------------------- | :-------: |
|  sid  | Slave id                                                                                                        |  1 \~ 255 |
|   fc  | <p>Function code</p><ul><li>03 = read holding register(multiple)</li><li>16 = write multiple register</li></ul> |   03, 16  |
|  addr | Start address                                                                                                   | 0 \~65534 |
|  len  | Data length                                                                                                     |  1 \~ 127 |
|  wait | Communication waiting time                                                                                      |           |
|  var  | <p>Array variable for transmitting/receiving data</p><p>(an internal Modbus map is used if not specified)</p>   |           |
