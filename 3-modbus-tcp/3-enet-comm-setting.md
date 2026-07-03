# 3.3 Ethernet communication setting
 
Before performing Modbus TCP master operations, you must first create and configure an Ethernet communication object.<br>

For details, please refer to the separate "[${cont_model} Controller Operation Manual - TP630](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-tp630/7-system/3-control-parameter/9-network-setting/2-service/4-enet-comm-setting?cont_model=${cont_model})" document.

*   **Protocol**

    For MODBUS TCP master operation, this must be set to "TCPc" (TCP client).


*   **Local port**

    Modbus communication uses port 502 by default.


*   **Remote port**

    Modbus communication uses port 502 by default.

<br>

To operate as a Modbus master, you must also execute the master function separately. (Refer to [4. Modbus Master](../4-modbus-master/README.md)) <br>

