# Hi6 Robot Controller Function Manual - Modbus

{% hint style="warning" %}
The information provided in this product manual is the property of Hyundai Robotics.

It cannot be reproduced or redistributed in whole or in part without written consent from Hyundai Robotics, and it cannot be provided to a third party or used for other purposes.



This manual may change without prior notice.



**Copyright ⓒ 2020 by Hyundai Robotics**
{% endhint %}
# 1. Overview

# 1.1 Preparatory Information

To understand this manual, the following preparatory information is required.

1. **Knowledge about operating the Hi6 robot controller**
2. **Knowledge about the Modbus protocol**
# 1.2 Functions of the Modbus

The Hi6 robot controller supports the Modbus master and slave functions via serial and Ethernet communications.

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
| Operation of a master | <p>Settings of the controller  </p> | <p>Settings of the controller</p>               |
|  Operation of a slave|   Settings of the controller   | <p>IP: Settings of the controller</p><p>Port: 502(Fixed), possible 2 ports add</p> |



### <mark style="color:green;">4. Modbus TCP slave setting</mark>
    You can add up to 3 slaves from the system/Control parameter/Network/Service/Modbus slave menu. and it is also possible to monitor the current communication status. 

![](../_assets/image30.png)

*   **Port No.**

    Sets the port for MODBUS TCP communication. Each slave must be set to a different port number.


*   **Timeout**

    Sets the time to check the MODBUS TCP communication connection status.


*   **Max connection**

    Sets the maximum number of connections for the master that can be connected to each slave. Currently, you can get up to three.


### <mark style="color:green;">5. Transmission mode</mark>

| **Operation method** |              **Serial communication**              | **Ethernet commuication** |
| :-------: | :----------------------------------: | :--------: |
| Operation of a master |               binary mode              |  binary mode |
|  Operation of a slave | <p>ASCII mode</p><p>RTU (binary) mode</p> |  binary mode |



### <mark style="color:green;">6. Functions supported</mark>

| **Operation method** | 　　　　　　　　**Serial/Ethernet communication**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Operation of the master | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|  Operation of a slave | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">7. Slave address</mark>

* Slave address: 1–247
* Supports the broadcast function that operates all slaves regardless of the set address if the slave address of the command is 0.

### <mark style="color:green;">8. Serial communication connection</mark>

* Connector (D-sub 9pin female)

![](../_assets/image5.png)

* Pin map

![](../_assets/image6.png)

### <mark style="color:green;">9. Address map</mark>

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

### <mark style="color:green;">10. SW memory map</mark>&#x20;

<mark style="color:red;">\*This is information defined internally by the system. For more details, refer to the "Embedded PLC User Manual."</mark>
# 2. Setting Up the Serial Communication

# 2.1 Connecting the Serial Cable

Connect the serial cable directly to the COM2 port, as shown in the figure below.

![](../_assets/image7.jpeg)
# 2.2 Setting the Serial Port Usage

You can set the usage of the serial port to the Modbus by applying the settings as follows in the **\[General Setting]** tab of the **\[Setting > 2: Control Parameter > 3: Serial Port]** screen.

![](../_assets/image8.png)


# 2.3 Setting the Modbus Environment

The details of the Modbus can be set in the **\[Modbus]** tab as follows.

![](../_assets/image9.png)

*   **Operation**: Select whether to operate as the master or the slave.

    In cases of operations as the master, the slave ID and mode will not be used, as the execution will be performed by commands in robot language.
* **Slave ID**: Set the ID for communications as the slave of the Modbus serial communication.
* **Mode**: Set the mode for communications as the slave of the Modbus serial communication.
# 3. Operating the Master
The Modbus Master app is not currently supported on the Hi6 Open Platform.