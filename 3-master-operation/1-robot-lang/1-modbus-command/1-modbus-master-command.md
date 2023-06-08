# 3.1.1.1 Modbus Master Constructor: Modbus

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



