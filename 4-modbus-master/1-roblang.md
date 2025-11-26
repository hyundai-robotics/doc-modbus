# 4.1 로봇언어로 동작

로봇언어 명령문을 사용하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. <br>
해당 명령문을 실행할 때 데이터 송수신이 이루어집니다. <br>
<br>

#### <mark style="color:green;">문법</mark>

```
modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=fb1.diw0  # 시리얼통신
or
modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=_mw10  # 이더넷통신
```

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| _sci2 or _enet0 | <p>시리얼포트 또는 이더넷객체 (str)</p><ul><li>_sci2 : 시리얼포트 2</li><li>_enet0 : 이더넷객체 0</li></ul>                                         | "_sci2" or "_enet0" |
| sid  | 슬래이브 ID(1~247) (int)                                                      | 1 |
| fc | <p>펑션코드 (int)</p><ul><li>3 : read holding registers</li><li>16 : write holding registers</li><li>4 : read input registers</li></ul>                                         | 3 / 16 / 4 |
| addr  | 슬래이브의 시작주소 (0~65534) (int)                                                       | 0 |
| len  | 데이터 개수 (1~127) (int)                                                     | 10 |
| wait  | timeout 시간 (sec) (double), 지정하지 않으면 무한대기                                                       | 3.0 |
| var  | int형 배열 변수, 데이터 메모리(_mw0), 입력신호(fb2.diw0), 출력신호(fb3.dow0)                                                       | arr / _mw0 / fb1.diw0 |

<br>

#### <mark style="color:green;">샘플 프로그램</mark>


``` python
Hyundai Robot Job File; { version: 2.0, mech_type: "", total_axis: -1, aux_axis: -1 }
     var arr # 지역변수 정의
     arr=Array(20) # int타입의 배열 변수로 정의
     modbus _enet0,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr # address 0부터 10개의 데이터를 읽어 arr변수에 대입
     print arr[0] # arr변수의 0번 인덱스 값을 출력
     delay 2 #시간 지연
     arr[0]=arr[0]+1 # arr변수의 0번 인덱스 값에 1을 더하기
     modbus _enet0,sid=1,fc=16,addr=0,len=10,wait=3.0,var=arr # address 0부터 10개의 데이터에 arr변수 값을 쓰기
     delay 2 #시간 지연
     end

```

