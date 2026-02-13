
[__SOURCE](README.md)
# ${cont_model} Controller Function Manual - Modbus

[__SOURCE](0-about-this-manual/precautions.md)
# Precautions

{% include url="https://hrcontentsrelay-bmgae5hdbzapc4bc.koreacentral-01.azurewebsites.net/api/proxy?path=doc-common-pages/en/precautions.md" %}

[__SOURCE](1-intro/README.md)
# 1. Overview


[__SOURCE](1-intro/1-preparatory-information.md)
# 1.1 Preparatory Information

To understand this manual, the following preparatory information is required.

1. **Knowledge about operating the ${cont_model} controller**
2. **Knowledge about the Modbus protocol**

[__SOURCE](1-intro/2-modbus-func.md)
# 1.2 Functions of the Modbus

The ${cont_model} robot controller supports the Modbus master and slave functions via serial and Ethernet communications.

### <mark style="color:green;">1. Operational example of a Modbus master </mark>

*   **Equipment control**

    Enables control over the equipment (ex. gripper) that supports the Modbus.

![](../_assets/image2.png)

### <mark style="color:green;">2. Operational example of a MODBUS slave</mark>

*   **Operation Panel Function**

    One or several robots can be connected and used through serial or Ethernet communications using an inexpensive grahic panel (GP) that supports the Modbus.


*   **PLC Communication**

    Enables communication with PLCs with the Modbus master function through inexpensive solutions.


*   **PC-Based Robot Operation System**

    You can build a PC-based robot operating system that monitors or controls the input/output signals of the robot.

![](../_assets/image3.png)

### <mark style="color:green;">3. Support method</mark>

| **Operation method** | **Serial communication** |               **Ethernet communication**               |
| :-------: | :--------: | :------------------------------------: |
| Operation of a master | <p>Robot language command <br>Controller setup </p> | <p>Robot language command <br>Controller setup</p>               |
|  Operation of a slave|   Settings of the controller   | <p>Settings of the controller</p><p>(possible 3 ports add)</p> |


### <mark style="color:green;">4. Transmission mode</mark>

| **Operation method** |              **Serial communication**              | **Ethernet commuication** |
| :-------: | :----------------------------------: | :--------: |
| Operation of a master |               binary mode              |  binary mode |
|  Operation of a slave | <p>ASCII mode</p><p>RTU (binary) mode</p> |  binary mode |



### <mark style="color:green;">5. Functions supported</mark>

| **Operation method** |         **Serial/Ethernet communication**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Operation of the master | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li><li>04: read input registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|  Operation of a slave | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">6. Slave address</mark>

* Slave address: 1-247

<br>

### <mark style="color:green;">7. Address map</mark>

![](<../_assets/image_10.png>)

*   The large numeric characters in italics in the table above are relay groups used in the Modbus.

    * MW (data memory for user)
    * DO (digital output)
    * SO (system output)
    * SI (system input)
    * SW (System memory)
    * Y (output relay)
    * X (input relay)


*   Data format

    When it comes to the floating-point format, the IEEE single-precision 32 bit float-point is used. For 8 bit/16 bit/32 bit, all signed integers will be used.


*   <mark style="color:red;background-color:yellow;">\*For the endian of the relay, little-endian is used.</mark>

    Example: In the case of dof0=6.515625 (0x40D08000) in float format as an example

    dol0=0x4D08000 -> dow0=0x8000, dow2=0x40D0 -> dob0=0x00, dob1=0x80, dob2=0xD0, dob3=0x40

{% hint style="info" %}
For Modbus transmissions, the endian will be 16-bit aligned big-endian.

In other words, the above transmission will occur in the order of 0x80, 0x00, 0x40, and 0xD0.
{% endhint %}

### <mark style="color:green;">8. SW memory map</mark>&#x20;

<mark style="color:red;">\*This is information defined internally by the system. For more details, refer to the "Embedded PLC User Manual."</mark>

[__SOURCE](2-modbus-rtu/README.md)
# 2. Modbus serial

This is about slave or master operation via Modbus serial communication.

[__SOURCE](2-modbus-rtu/1-serial-cable-connect.md)
# 2.1 Connecting the serial cable

Connect the serial cable directly to the COM2 port, as shown in the figure below.

![](../_assets/image7.jpeg)



* Connector (D-sub 9pin female)

![](../_assets/image5.png)

* Pin map

![](../_assets/image6.png)

[__SOURCE](2-modbus-rtu/2-serial-port-usage-setting.md)
# 2.2 Serial port usage setting 

You can set the usage of the serial port to the Modbus by applying the settings as follows in the **\[General]** tab of the **\[System > 2: Control parameter > 3: Serial port]** screen. <br>
**\[Modbus]** tab is enable.

![](../_assets/image8.png)



[__SOURCE](2-modbus-rtu/3-modbus-environment-setting.md)
# 2.3 Modbus environment setting 

The details of the Modbus can be set in the **\[Modbus]** tab as follows.

![](../_assets/image9.png)

*   **Operation**: Select whether to operate as the master or the slave.

    In cases of operations as the master, the slave ID and mode will not be used, as the execution will be performed by master query. <br>
    In cases of operations as the slave, the controller responds to master requests, so it operates only with the settings on the screen. <br> 
* **Slave ID**: Set the ID for communications as the slave of the modbus serial communication.
* **Mode**: Set the mode for communications as the slave of the modbus serial communication.

[__SOURCE](3-modbus-tcp/README.md)
# 3. Modbus TCP

This is about modbus tcp slave communication and ethernet communication for modbus master operation. For modbus master operation refer to the [4. Modbus master]. 

[__SOURCE](3-modbus-tcp/1-network-setting.md)
# 3.2 Network setting

To use ethernet communication, first set the network address corresponding to the connected LAN port on the **\[System > 2: Control parameter > 9: Network > 1: Environment setting]** screen.

![](../_assets/image31.png)



[__SOURCE](3-modbus-tcp/2-slave-setting.md)
# 3.2 Modbus slave setting

Set on the **\[System > 2: Control parameter > 9: Network > 2: Service > 1: Modbus slave]** screen. <br>
Up to three slaves can be used using "+" button, and the current communication status can also be monitored. <br>
When operating as a slave, the controller responds to master requests, so it operates only with the settings on the screen. <br>

![](../_assets/image30.png)

*   **Port No.**

    Sets the port for MODBUS TCP communication. <br>
    Each slave must be set to a different port number.


*   **Timeout**

    Sets the time to check the MODBUS TCP communication connection status. <br>
    If there is no service request from the master for a specified period of time, the connection is forcibly terminated.


*   **Max connection**

    Sets the maximum number of connections for the master that can be connected to each slave. <br>
    Currently, you can get up to three.

[__SOURCE](3-modbus-tcp/3-enet-comm-setting.md)
# 3.3 Ethernet communication setting
 
Before performing Modbus TCP master operations, you must first create and configure an Ethernet communication object.<br>

Set on **\[System > 2: Control parameter > 9: Network > 2: Service > 4: Ethernet communication]** screen. <br>
Up to five Ethernet objects can be created using "+" button and used, and the current communication status can also be monitored. <br>
Since Master operates using robot language commands, you must write and run a task program separately from the screen settings. (Refer to [3.4 Modbus master operation]) <br>

![](../_assets/image32.png)

You can force close the socket of the corresponding Ethernet object with the [Close] button, and perform a communication connection with the [Connect] button. <br>
When the controller boots, it automatically establishes a communication connection with the configured Ethernet object. <br>


*   **Name**

    The name of the Ethernet communication object. Each name must be set to "enet0" ~ "enet4".


*   **Protocol**

    Select the communication protocol. For MODBUS TCP master operation, this must be set to "TCPc" (TCP client).


*   **IP address**

    Sets the IP address used by the slave. 


*   **Local port**

    Sets the local port number. Modbus communication uses port 502 by default.


*   **Remote port**

    Sets the remote port number. Modbus communication uses port 502 by default.


*   **State**

    Displays the status of the communication connection. 

[__SOURCE](4-modbus-master/README.md)
# 4. Modbus master

To operate the Modbus master, there are two ways: by executing it using robot language commands, or by registering a query in the settings screen and executing it. 

[__SOURCE](4-modbus-master/1-roblang.md)
# 4.1 Operation for robot language

You can use robot language statements to construct modbus master queries and send them to slaves. <br>
When the statement is executed, data is transmitted and received. <br>
<br>

#### <mark style="color:green;">Grammer</mark>

```
modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=fb1.diw0  # Serial communication
modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=_mw10  # Ethernet communication
```

#### <mark style="color:green;">Parameters</mark>

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

#### <mark style="color:green;">Sample job</mark>


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


[__SOURCE](4-modbus-master/2-setup.md)
# 4.2 Operation for query setting

When a user sets up a modbus master query, it can be automatically transmitted to the slaves based on this. <br>
Data transmission and reception occurs when executing a query in a configured query. <br>
<br>


Set on **\[System > 2: Control parameter > 9: Network > 2: Service > 5: Modbus master]** screen. <br>
You can add query using "+" button and possible monitoring of the current query execution status. <br>

![](../_assets/image33.png)

You can force stop of master execution by [stop] button and reexecute by [execute] button. <br>
After the controller boots, the set queries are automatically executed sequentially. <br>


*   **Name**

    The name of the query object. Each name must be set to "query_?".


*   **Communication type**

    Select the communication method: Ethernet communication or serial communication.


*   **Object number**

    For Ethernet communication, set the enet number set in [3.3 Ethernet Communication]. <br> 
    For serial communication, only 2 are currently available.


*   **Slave id**

    Set the Slave ID(1~247).


*   **Function**

    Set the function code. <br>
    F03 : read holding registers <br>
    F16 : write holding registers <br>
    F04 : read input registers <br>


*   **Start address**

    Set the slave's starting address (0~65534). 


*   **Length**

    Set the number of data (1 to 127).


*   **Timeout**

    Sets the timeout period (in seconds). Setting it to 0 will wait indefinitely. 


*   **Relay name/Address**

    Set to relay (data memory/input/output signal, etc.) name or Modbus slave address. 


*   **Delay time**

    Sets the delay between the execution of the current query and the execution of the next query. 


*   **State**

    The execution status of the currently running query is displayed by configuring it with robot language commands. 
