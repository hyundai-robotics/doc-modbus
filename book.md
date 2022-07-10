# Hi6 Robot Controller Function Manual - Modbus

{% hint style="warning" %}
The information presented in this manual is the property of Hyundai Robotics.

The manual may neither be copied, in part or in full, nor redistributed without prior&#x20;

written consent from Hyundai Robotics.

It may neither be provided to any third party nor used for any other purposes.



Hyundai Robotics reserves the right to modify this document without prior notification.



**Copyright ⓒ 2020 by Hyundai Robotics**
{% endhint %}
# 1. Overview

# 1.1 Preparatory information

The following information is needed in advance to understand the manual.

1. **Knowledge about the operation of the Hi6 robot controller**
2. **Knowledge about the MODBUS protocol**
# 1.2 Functions of MODBUS

The Hi6 robot controller supports the MODBUS master and slave functions both through serial communication and Ethernet communication.

### <mark style="color:green;">1. Example of MODBUS master operation</mark>

*   **Control of equipment :**

    Capable of controlling the equipment (e.g., gripper) that supports MODBUS

![](../_assets/image2.png)

### <mark style="color:green;">2. Example of MODBUS slave operation</mark>

*   **Function as an operation panel:**

    With an inexpensive graphic panel (GP) that supports MODBUS, you can use one or multiple robots by connecting them through serial or Ethernet communication.


*   **Programmable Logic Controller (PLC) communication:**

    Provides an inexpensive solution for communication with PLCs that have the MODBUS master function.


*   **PC-based robot operation system:**

    Allows a robot operation system to be built that monitors or controls the robot’s input and output signals using a PC.

![](../_assets/image3.png)

![](<../_assets/image_8.png>)

### <mark style="color:green;">3. Support method</mark>

| **Kind of operation** | **Serial communication** |          **Ethernet communication**         |
| :-------------------: | :----------------------: | :-----------------------------------------: |
|  Operation of master  | Robot language statement |           Robot language statement          |
|   Operation of slave  |   Setting in controller  | IP: Setting in controller Port: 502 (fixed) |



### <mark style="color:green;">4. Transmission mode</mark>

| **Kind of operation** |         **Serial communication**         | **Ethernet communication** |
| :-------------------: | :--------------------------------------: | :------------------------: |
|  Operation of master  |                binary mode               |         binary mode        |
|   Operation of slave  | <p>ASCII mode</p><p>RTU(binary) mode</p> |         binary mode        |



### <mark style="color:green;">5. Functions supported</mark>

| **Kind of operation** | 　　　　**Serial / Ethernet communication**                                                                                                                                                                                                                                                                                                                                                                      |
| :-------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
|  Operation of master  | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|   Operation of slave  | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">6. Slave address</mark>

* Slave address: 1–247
* If the slave address of a command is 0, the broadcasting function, which allows all slaves to operate regardless of the set address, will be supported.

### <mark style="color:green;">7. Serial communication connection</mark>

* Connector (DSUB – 9pin: female)

![](<../_assets/image_10.png>)

* Pin map

![](../_assets/image6.png)

### <mark style="color:green;">8. Address map</mark>

![](<../_assets/image_17.png>)

*   The enlarged numbers in italics in the table above represent the relay groups used in the MODBUS function.

    * MW(data memory for user)
    * DO(digital output)
    * SO(system output)
    * SI(system input)
    * SW(System memory)


*   Data format:

    For the float format, IEEE single-precision 32-bit float point is used, and, in the case of 8 bit / 16 bit / 32 bit, signed integers are used all.


*   <mark style="color:red;background-color:yellow;">\*For the relay's endian, little endian is used</mark>

    Example: In the case of dof0=6.515625(0x40D08000), which is in the float format

    dol0=0x40D08000 → dow0=0x8000, dow2=0x40D0 → dob0=0x00, dob1=0x80, dob2=0xD0, dob3=0x40

{% hint style="info" %}
Explanation: In MODBUS transmission, big endian of 16-bit align is used.&#x20;

In other words, the transmission described above will be performed in the order of 0x80, 0x00, 0x40, and 0xD0.
{% endhint %}

### <mark style="color:green;">9. SW memory map</mark>&#x20;

![](<../_assets/image_21.png>)

#### PLC-related

![](<../_assets/image_20.png>)

#### Software version

![](<../_assets/image_4.png>)

#### Program counter

![](<../_assets/image_2.png>)

#### Total time of operation

![](<../_assets/image_22.png>)

#### Robot position

![](<../_assets/image_16.png>)

#### Robot speed

![](<../_assets/image_15.png>)

#### Robot load factor

![](<../_assets/image_18.png>)

#### Conveyor synchronization

![](<../_assets/image_19.png>)
# 2. 2. Serial Communication Setting

# 2.1 Serial cable connection

The serial cable is erectly connected to the COM2 port as shown in the figure below.

![](../_assets/image7.jpeg)
# 2.2 Serial port usage setting

The usage of the serial port can be set as MODBUS as shown below in the **\[General]** tab on the **\[Set Up → 2: Control parameter → 3: Serial port]** screen.

![](../_assets/image.png)
# 2.3 MODBUS environment setting

Details regarding MODBUS can be set in the **\[Modbus]**tab, as shown below.

![](<../_assets/image_9.png>)

* **Operation** : You can select whether to operate the port as master or slave. When it is operated as master, the operation will be executed according to the robot language commands; thus, the Slave id and Mode will not be used.
* **Slave id** : You can set the id for the communication as slave for MODBUS serial communication.
* **Mode** : You can set the mode for the communication as slave for MODBUS serial communication
# 3. Operation as Master

# 3.1 Robot Language

You can configure a Modbus master query using robot language statements and transmit it to the slave. Data will be transmitted and received when the statement is executed. In case it is necessary to transmit and receive data periodically, you can configure a job program for that purpose and then perform operation in multitask mode. However, in this case, it is recommended to communicate through the built-in PLC.
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
# 3.1.2 Sample program

# 3.1.2.1 Ethernet communication

The following shows a sample program for controlling the onRobot gripper. The Hi6 controller and onRobot gripper communicate through MODBUS tcp. In the example, the Hi6 controller is operated as master and the gripper as slave.

{% code title="0060.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
call 61,1 # onRobot module open
call 61,2,0 # onRobot gripper hold
delay(3)
call 61,2,300 # onRobot gripper release
call 61,0 # onRobot module close
delay(3)
end
```
{% endcode %}

{% code title="0061.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
param mode,grip
if (mode == 1) # enet module open
    import enet
    global enet2,arr
    if (arr==0)
        arr=Array(5)
    endif
    # onRobot gripper enet connect
    enet2=enet.ENet("tcp") #udp,tcp
    enet2.ip_addr="192.168.1.111" #OnRonot IP
    enet2.lport=502
    enet2.rport=502
    if (enet2.state() < 1)
        enet2.open
        enet2.connect #tcp인 경우
    else
        stop
    endif
    print "enet2.state", enet2.state()
elseif (mode == 0) # enet module close
    enet2.close
else # onRobot gripper operate
    arr[0] = 300 # force (0~400)
    arr[1] = grip # width (0~1100)
    arr[2] = 1 # control (1:grip, 8=stop, 16=offset grip) 
    modbus enet2,sid=65,fc=16,addr=0,len=3,wait=3,var=arr
endif
end
```
{% endcode %}
# 3.1.2.2 Serial communication

The following shows a sample program when assuming that the onRobot gripper is controlled through serial communication. First, in the serial communication setting, port usage should be set as **\[MODBUS]** , and MODBUS operation should be set as **\[master]**.

{% code title="0060.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
call 61,1 # onRobot module open
call 61,2,0 # onRobot gripper hold
delay(3)
call 61,2,300 # onRobot gripper release
call 61,0 # onRobot module close
delay(3)
end
```
{% endcode %}

{% code title="0061.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
param mode,grip
if (mode == 1) # enet module open
    global sci2,arr
    if (arr==0)
        arr=Array(5)
    endif
    # onRobot gripper enet connect
    sci2=com.Sci(2) # serial port 2 object
elseif (mode == 0) # enet module close
    print “sci close”
else # onRobot gripper operate
    arr[0] = 300 # force (0~400)
    arr[1] = grip # width (0~1100)
    arr[2] = 1 # control (1:grip, 8=stop, 16=offset grip) 
    modbus sci2,sid=65,fc=16,addr=0,len=3,wait=3,var=arr
endif
end

```
{% endcode %}
# 3.2 Embedded PLC

The built-in PLC ladder logic makes it possible to configure a Modbus master query and transmit it to the slave. You need knowledge about the built-in PLC and should refer to the related manuals to use this function.
# 3.2.1 Addition of native firmware library

In **\[Libraries > Insert > Firmware Library...]**, select the “**Hi6\_FWLib.fwl**” file, and add it, as shown below.

![](../../_assets/image10.png) ![](../../_assets/image11.png)

In the Edit Wizard, you can see that the function blocks related to ENET communication and those related to MODBUS\_TCP have been added.

![](../../_assets/image12.png) ![](../../_assets/image13.png)
# 3.2.2 Addition of function blocks

In **\[Logical POUs > Insert > Function Block...]**, as shown in the figure below, you can add a function block.

![](../../_assets/image14.png)

In this example, we have chosen to use the ST language under Modbus.

![](../../_assets/image15.png)
# 3.2.3 Addition of variables

In the variable management screen, you can register variables, as shown in the figure below.

![](../../_assets/image16.png)
# 3.2.4 Creation of function blocks

As shown in the following figure, the user can write a program on the program writing screen.

#### <mark style="color:green;">FBD language</mark>

![](../../_assets/image17.png)

#### <mark style="color:green;">ST language</mark>

![](../../_assets/image18.png)
# 3.2.5 Sample function blocks

#### <mark style="color:green;">FBD language</mark>

![](../../_assets/image19.png)

#### <mark style="color:green;">ST  language</mark>

![](../../_assets/image20.png)
# 3.2.6 Description of sample function blocks

* Once the PLC is run, the socket for ENET communication will be automatically opened because the socket variable has been already initialized to “0.”
* Regarding IPaddr, you must designate the IP address of the counterpart device that you need to connect to.

![](../../_assets/image21.png)

* In case the connect variable is set to “1,” the operation of connection to the slave device will be performed, and then the connect variable will be changed to “0.”

![](../../_assets/image22.png)

* In case the query variable is set to “1,” 3 data results will be obtained from the address 0 of the slave in compliance with Function:=3, Address:=0, and Length:=3 and then transferred to the array variable of data (read).
* In case the result is “1,” the query will be set to “2,” and the variable value of data\[0] will be increased by 1.

![](../../_assets/image23.png)

* In case the query variable is set to “2,” the value set in the data array variable will be set as the 3 data to the address 0 of the slave in compliance with Function:=16, Address:=0, and Length:=3 (write).
* In case the result is “1,” the query will be set to “1.”

![](../../_assets/image24.png)

* In case the close variable is set to “1,” the socket will be closed.

![](../../_assets/image25.png)
# 3.2.7 Operation of sample function blocks

* The function blocks written as samples will be called and executed from main, which is a program POU.
* Main, a program POU, will be executed every 200 ms in the Cycle task.

![](../../_assets/image26.png)
