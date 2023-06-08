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
|  length  | Slave register length (int)                                                        | 3 |
|  timeout    |[Option]  Timeout (int), Unit: msec                                                      | 5000 |
|  addr_on_timeout    |[Option]  Escape address in case of timeout (int or str)                                                      | 99 or S1 |

