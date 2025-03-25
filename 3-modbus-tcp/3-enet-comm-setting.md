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
