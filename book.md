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
| Operation of a master |  <p>Statements in robot language</p><p>Settings of the controller  </p> |                <p>Statements in robot language</p><p>Settings of the controller</p>               |
|  Operation of a slave|   Settings of the controller   | <p>IP: Settings of the controller</p><p>Port: 502, 5000, 5001 (Fixed)</p> |



### <mark style="color:green;">4. Transmission mode</mark>

| **Operation method** |              **Serial communication**              | **Ethernet commuication** |
| :-------: | :----------------------------------: | :--------: |
| Operation of a master |               binary mode              |  binary mode |
|  Operation of a slave | <p>ASCII mode</p><p>RTU (binary) mode</p> |  binary mode |



### <mark style="color:green;">5. Functions supported</mark>

| **Operation method** | 　　　　　　　　**Serial/Ethernet communication**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Operation of the master | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|  Operation of a slave | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">6. Slave address</mark>

* Slave address: 1–247
* Supports the broadcast function that operates all slaves regardless of the set address if the slave address of the command is 0.

### <mark style="color:green;">7. Serial communication connection</mark>

* Connector (D-sub 9pin female)

![](../_assets/image5.png)

* Pin map

![](../_assets/image6.png)

### <mark style="color:green;">8. Address map</mark>

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

### <mark style="color:green;">9. SW memory map</mark>&#x20;

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
The Modbus Master was developed as an app for the Hi6 open platform.

You can check the app information after changing its location to Main in **\[Service > 10: App.]**

![](../_assets/image29.png)# 3.1 Robot Language

By using statements in robot language, you can configure a Modbus master query and send it to the slave. If the periodic transmission and reception of data are required, you can communicate through the internal settings of the controller.
# 3.1.1 Commands

Commands are used to communicate with the Modbus master in robot language. To understand Modbus communication, you will also need to learn about Modbus communication.# 3.1.1.1 Modbus Master Constructor: Modbus

This is a statement that declares the Modbus master.

As commnunications can be performed by utilizing the master, it should be delcared before the read/write operation.

The Modbus module must be loaded first before the us (example: import modbus.)
#### <mark style="color:green;">Grammar</mark>

```
var master = modbus.Modbus(protocol,port,ip_addr)
```
#### <mark style="color:green;">Return value</mark>
- Modbus master object

#### <mark style="color:green;">Parameter</mark>

|Parameter| Description                                                                                                    |    Example    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| protocol | <p>Communication protocol type(str)</p><ul><li>tcp</li><li>rtu</li></ul>                                         | "tcp" or "rtu" |
|   port  | <p>Communication port (int)</p><ul><li>tcp: TCP  port number </li><li>rtu: Serial port number (0–2)</li></ul>        |  502 (tcp) or 2 (rtu)   |
| ip_addr  | [Use tcp only] Modbus TCP slave device IP address(str)                                                       | "192.168.1.1" |



# 3.1.1.2 Reading the Data: read (function code: 03)

This is a statement for reading the register data of the slave device.

It will read as much as the register length starting from the register address of the slave. 

It will perform upon loading the master declared earlier.

#### <mark style="color:green;">Grammar</mark>

```
var value = master.read(salve_id,addr,length,timeout,addr_on_timeout)
```

#### <mark style="color:green;">Return value</mark>
- Data to transmit (array type)

#### <mark style="color:green;">Parameter</mark>

|Parameter| Description                                                                                                    |    Example    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| slave_id | Slave device id (int)                                         | <p>1</p>(0–255) |
|   addr  | <p>Slave register start address (int)</p> (use decimal numbers, not hex)      |  0  |
|  length  | Slave register length (int, 1<= length <= 125)                                 | 3 |
|  timeout    |[Option]  Timeout (int), Unit: msec                                                      | 5000 |
|  addr_on_timeout    |[Option]  Escape address in case of timeout (int or str)                                                      | 99 or S1 |

# 3.1.1.3 Writing the Data: write (function code: 16)

This is a statement for writing data in the register of the slave device.

It will write as much as the register length starting from the register address of the slave.

If the slave register length is smaller than the length of the input data, data will be inputted as much as the register length starting from address 0. 
Conversely, if the slave register length is larger, data will be written as much as the length of the input data. 

It will perform upon loading the master declared earlier.

#### <mark style="color:green;">Grammar</mark>

``` python
master.write(salve_id,addr,length,values,timeout)
```

#### <mark style="color:green;">Parameter</mark>

|Parameter| Description                                                                                                    |    Example   |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| slave_id | Slave device id (int)                                         | <p>1</p>(0–255) |
|   addr  | <p>Slave register start address (int)</p> (use decimal numbers, not hex)      |  0  |
|  length  | Slave register length (int, 1<= length <= 123)                                 | 3 |
|  values  |  Input data (array)                                                      | [0,1,2] |
|  timeout    |[Option]  Timeout (int), Unit: msec                                                      | 5000 |
|  addr_on_timeout    |[Option]  Escape address in case of timeout (int or str)                                                      | 99 or S1 |


# 3.1.1.4 Status of the Modbus Master: status

This is a statement for checking the Modbus communication status.
It will perform upon loading the master declared earlier.

#### <mark style="color:green;">Grammar</mark>

```
var status = master.staus
```

#### <mark style="color:green;">Return value</mark>
- Status of the Modbus communication

#### <mark style="color:green;">Parameter</mark>

|Parameter| Description                                                                                                    
| :---: | ------------------------------------------------------------------------------------------------------- 
|   state  | <p>Communication status</p><ul><li>Default state (no connection) = 0</li><li>Normal = 1</li><li>Communication error = -1</li><li>Timeout error = -2</li></ul>    | 


# 3.1.2 Sample Programs

# 3.1.2.1 Ethernet Communication

 The following shows a sample program that controls the onRobot gripper. The Hi6 controller and the onRobot gripper communicate via the Modbus tcp. Here, the Hi6 controller is operated as the master, while the gripper is operated as a slave.

```python
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
    
     # Import the modbus module, then create a modbus master object using a constructor (set the communication type, port number, and IP address.) 
     import modbus
     var master=modbus.Modbus("tcp",502,"192.168.1.1")

     # Data to transmit
     global arr
     arr=Array(300)
     arr[0]=300 #force(0–400)
     arr[1]=200 #width (0–1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)
     
     # Execute the write operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     master.write(65,0,3,arr,3000,99)

     # Check the communication status (0: no connection (default state), 1: normal state, -1: communication failure, -2: timeout error.)
     if master.status<0
       print "write communication error"
       stop
     endif

     # Execute the read operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     var recv_data=master.read(65,267,2,3000,99)
     if master.status<0
       print "read communication error"
       stop
     endif

     # Handle the received data.
     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end

  99 print "timeout"



```

# 3.1.2.2 Serial Communication

 The following shows a sample program based on the assumption that the onRobot gripper will be controlled via serial communication. First, in the serial communication settings, set the usage of the port to **\[MODBUS]** and the operation of the Modbus to **\[master.]**


``` python
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
     
     # Import the modbus module, then create a modbus master object with a constructor (set the communication type and port number.)  
     import modbus
     var master=modbus.Modbus("rtu",2)
     
     # Data to transmit
     global arr
     arr=Array(300)
     arr[0]=300 #force(0–400)
     arr[1]=200 #width (0–1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)
     
     # Execute the write operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     master.write(65,0,3,arr,3000,99)
     
     # Check the communication status (0: no connection (default state), 1: normal state, -1: communication failure, -2: timeout error.)
     if master.status<0
       print "write communication error"
       stop
     endif
     
     # Execute the read operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     var recv_data=master.read(65,267,2,3000,99)
     if master.status<0
       print "read communication error"
       stop
     endif

     # Handle the received data.
     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end

  99 print "timeout"

```
# 3.2 Setting the Controller

By setting up the controller, you can configure a Modbus master query and send it to a slave. You can transmit and receive data periodically by setting a scan rate. Through the settings, you can set the master and slave one-to-one.

You can set the Modbus master in the **\[Setting > 2: Control parameter > Modbus]** screen according to the communication method.

If the set parameter is not shown, check the Start and Status data in the **\[Service> 10: App > Main]** screen before performing the operation.  
# 3.2.1 Setting the Modbus TCP Master

This setting is used for configuring a Modbus TCP master query and sending it to a slave.

Using the relevant master, you can perform communications.


You can set the Modbus TCP master in the **\[Setting > 2: Control parameter > Modbus > 1: Modbus TCP Master settings]** screen.

{% hint style="warning" %}
To communicate using the changed parameters, the changes should be reflected through [Confirm] or [Apply (shift + confirm)]. Doing so will enable you to perform a job correctly.
{% endhint %}

![](../../_assets/image27.png)

<li>IP address: Slave device IP address</li>
<li>Port: TCP communication port number (default value: 502)</li>
<li>Slave ID: Slave device ID</li>
<li>Function code: Function code (03: Read Holding Registers /16: Write Multiple registers)
<li>Address: Slave register start address</li>
<li>Length: Slave register length</li>
<li>Scan Rate: Communication cycle (unit: 5 msec)</li>
<li>Value: 03: Data value to be read/16: Data value to be written (example) 1 unit of data: 0 / 3 units of data: 0,1,2 </li>
<li>Timeout: Time out (unit: msec)</li>
<li>Connect: Connects and starts transmitting and receiving data</li>
<li>Reset: Initializes to the previously saved value of the selected parameter. For an & value (read), the value that was read should be updated</li>
<li>ResetAll: Initializes to the previously saved values of all parameters</li>

# 3.2.2 Setting the Modbus RTU Master

This setting is used for configuring a Modbus RTU master query and sending it to a slave.

Using the relevant master, you can perform communications.


You can set the Modbus RTU master in the **\[Setting > 2: Control parameter > Modbus > 2: Modbus RTU Master settings]** screen.

{% hint style="warning" %}
To communicate using the changed parameters, the changes should be reflected through [Confirm] or [Apply (shift + confirm)]. Doing so will enable you to perform a job correctly.
{% endhint %}

![](../../_assets/image28.png)

<li>Serial port: Serial communication port number (default value: 2)</li>
<li>Slave ID: Slave device ID</li>
<li>Function code: Function code (03: Read Holding Registers /16: Write Multiple registers)
<li>Address: Slave register start address</li>
<li>Length: Slave register length</li>
<li>Scan Rate: Communication cycle (unit: 5 msec)</li>
<li>Value: 03: Data value to be read/16: Data value to be written (example) 1 unit of data: 0 / 3 units of data: 0,1,2</li>
<li>Timeout: Time out (unit: msec)</li>
<li>Connect: Connects and starts transmitting and receiving data</li>
<li>Reset: Initializes to the previously saved value of the selected parameter. For an & value (read), the value that was read should be updated.</li>
<li>ResetAll: Initializes to the previously saved values of all parameters.</li>