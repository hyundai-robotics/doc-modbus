# 4.2 Operation for query setting

When a user sets up a modbus master query, it can be automatically transmitted to the slaves based on this. <br>
Data transmission and reception occurs when executing a query in a configured query. <br>
<br>


Set on `[F2: System] - 2: Control parameter - 9: Network - 2: Service - 5: Modbus master` screen. <br>
You can add query using "+" button and possible monitoring of the current query execution status. <br>

![](../_assets/image33.png)

You can force stop of master execution by `[stop]` button and reexecute by `[execute]` button. <br>
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
    F01 : read coils (bits) <br>
    F02 : read discrete inputs (bits) <br>
    F03 : read holding registers <br>
    F04 : read input registers <br>
    F15 : write coils (bits) <br>
    F16 : write holding registers <br>


*   **Start address**

    Set the slave's starting address (0~65534). 


*   **Length**

    Set the number of data (words : 1~127, bits : 1~2000).


*   **Timeout**

    Sets the timeout period (in seconds). Setting it to 0 will wait indefinitely. 


*   **Relay name**

    Set to relay (data memory/input/output signal, etc.) name. 


*   **Delay time**

    Sets the delay between the execution of the current query and the execution of the next query. 


*   **State**

    The execution status of the currently running query is displayed by configuring it with robot language commands. 
