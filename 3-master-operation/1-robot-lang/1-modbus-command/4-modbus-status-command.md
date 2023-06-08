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


