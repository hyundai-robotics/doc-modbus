# 4.1 Operation for robot language

You can use robot language statements to construct modbus master queries and send them to slaves. <br>
When the statement is executed, data is transmitted and received. <br>
<br>

#### Grammer

```
modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=fb1.diw0  # Serial communication
modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=_mw10  # Ethernet communication
```

#### Parameters

|Parameters| Description                                                                                                    |    example    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| _sci2 or _enet0 | <p>Serial port or Ethernet object (str)</p><ul><li>_sci2 : Serial port 2</li><li>_enet0 : Ethernet object 0</li></ul>                                         | "_sci2" or "_enet0" |
| sid  | Slave ID(1~247) (int)                                                      | 1 |
| fc | <p>Function code (int)</p><ul><li>3 : read holding registers</li><li>16 : write holding registers</li><li>4 : read input registers</li></ul>                                         | 3 / 16 / 4 |
| addr  | Start address of slave (0~65534) (int)                                                       | 0 |
| len  | Quantity of data (1~127) (int)                                                     | 10 |
| wait  | timeout (sec) (double), If not specified, infinite waiting                                                       | 3.0 |
| var  | Array variable of integer type, Data memory(_mw0), Input signal(fb2.diw0), Output signal(fb3.dow0)                                                       | arr / _mw0 / fb1.diw0 |

<br>

#### Sample job


``` python
Hyundai Robot Job File; { version: 2.0, mech_type: "", total_axis: -1, aux_axis: -1 }
     var arr # Local variable definition
     arr=Array(20) # Defined as an array variable of int type
     modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr # Read 10 data from address 0 and assign them to the arr variable
     print arr[0] # Print the 0th index value of the arr variable
     delay 2 # Time delay
     arr[0]=arr[0]+1 # Add 1 to the 0th index value of the arr variable
     modbus _enet0,sid=1,fc=16,addr=0,len=10,wait=3.0,var=arr # Write the arr variable value to 10 data from address 0
     delay 2 # Time delay
     end

```

