﻿# 3.1.1.3 데이터 쓰기: write(function code: 16)

슬레이브 장치의 레지스터에 데이터를 쓰기 위한 명령문 입니다.

슬레이브의 레지스터 주소를 시작으로 레지스터 길이만큼 데이터를 쓰게 됩니다.

입력 데이터 길이보다 슬레이브 레지스터 길이가 작은 경우 0번지 부터 길이만큼 입력되게 되고,
반대로 슬레이브 레지스터 길이가 큰 경우에는 입력 데이터 길이 만큼만 쓰여지게 됩니다. 

앞서 선언한 마스터를 가져와 수행하게 됩니다.

#### <mark style="color:green;">문법</mark>

``` python
master.write(salve_id,addr,length,values,timeout)
```

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| slave_id | 슬레이브 장치 id(int)                                         | <p>1</p>(0~255) |
|   addr  | <p>슬레이브 레지스터 시작 주소(int)</p> (use devcimal numbers, not hex)      |  0  |
|  length  | 슬레이브 레지스터 길이(int)                                                        | 3 |
|  values  |  입력 데이터(array)                                                      | [0,1,2] |
|  timeout    |[선택사항]  타임아웃 (int), 단위: msec                                                      | 5000 |
|  addr_on_timeout    |[선택사항]  타임아웃 시 퇴피주소 (int or str)                                                      | 99 or S1 |

