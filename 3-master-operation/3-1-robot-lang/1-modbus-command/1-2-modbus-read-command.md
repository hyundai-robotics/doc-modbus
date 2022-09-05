# 3.1.1.2 데이터 읽기: read(function code: 03)

슬레이브 장치의 레지스터 데이터를 읽기 위한 명령문 입니다.

슬레이브의 레지스터 주소를 시작으로 레지스터 길이만큼을 읽어옵니다. 

앞서 선언한 마스터를 가져와 수행하게 됩니다.

#### <mark style="color:green;">문법</mark>

```
var value = master.read(salve_id,addr,length,timeout,addr_on_timeout)
```

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| var value | <p>임의의 변수를 선언하여 데이터를 읽어옴.(array)</p>(전역변수도 가능)                                      | [0,1,2] |
| slave_id | 슬레이브 장치 id (int)                                         | <p>1</p>(0~255) |
|   addr  | <p>슬레이브 레지스터 시작 주소 (int)</p> (use devcimal numbers, not hex)      |  0  |
|  length  | 슬레이브 레지스터 길이 (int)                                                        | 3 |
|  timeout    |[선택사항]  타임아웃 (int), 단위: msec                                                      | 5000 |
|  addr_on_timeout    |[선택사항]  타임아웃 시 퇴피주소 (int or str)                                                      | 99 or S1 |

