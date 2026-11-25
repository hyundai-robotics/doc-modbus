# 4.1 Operation for robot language

You can use robot language statements to construct modbus master queries and send them to slaves. <br>
When the statement is executed, data is transmitted and received. <br>
<br>

#### Syntax

```hrscript
modbus <communication method>,sid=<slave ID>,fc=<function code>,addr=<start address>,len=<length>,wait=<timeout duration>,var=<data to send or receive>,timeout_addr=<timeout branch address>
```

#### Parameters

| Parameter | Description | Example | Notes |
| - | - | - | - |
| Communication method | Sets the communication method (serial or Ethernet) | "_sci2" <br> "_enet0" <br> "_tio" | "_sci2": Serial port <br> "_enet0": Ethernet object <br> "_tio": Cobot Tool IO - RS485 Connector |
| Slave ID | ID of the slave | 1 | 1–247 |
| Function code | Modbus function code | 3, 16 | See [1.2.5 Supported Functions](../1-intro/2-modbus-func.md) |
| Start address | Starting address on the slave | 0 | 0–65534 |
| Length | Number of data items | 10 | Words: 1–127; bits: 1–2000 |
| Timeout duration | A timeout occurs if the operation takes longer than the specified duration | 3.0 | Waits indefinitely if omitted |
| Data to send or receive | Variable to store received data in, or variable containing data to send | arr / _mw0 / fb1.diw0 | Data memory, input signals, or output signals |
| Timeout branch address | Address to branch to when a timeout occurs | S1 / *timeout / 10 | Branch address. If only the timeout duration is specified, a timeout causes an error. |

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

