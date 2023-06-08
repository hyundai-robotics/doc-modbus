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
|  length  | Slave register length (int)                                                        | 3 |
|  values  |  Input data (array)                                                      | [0,1,2] |
|  timeout    |[Option]  Timeout (int), Unit: msec                                                      | 5000 |
|  addr_on_timeout    |[Option]  Escape address in case of timeout (int or str)                                                      | 99 or S1 |


