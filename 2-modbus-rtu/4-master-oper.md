# 2.4 모드버스 마스터 동작

로봇언어 명령문을 사용하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. <br>
해당 명령문을 실행할 때 데이터 송수신이 이루어집니다. <br>
<br>

#### <mark style="color:green;">문법</mark>

```
modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr
```

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| _sci2 | <p>시리얼포트 또는 이더넷객체 (str)</p><ul><li>_sci2 : 시리얼포트 2</li><li>_enet0 : 이더넷객체 0</li></ul>                                         | "_sci2" or "_enet0" |
| sid  | 슬레이브 ID(1~247) (int)                                                      | 1 |
| fc | <p>펑션코드 (int)</p><ul><li>3 : read holding registers (multiple)</li><li>16 : write holding registers (multiple)</li></ul>                                         | 3 or 16 |
| addr  | 슬레이브의 시작주소 (0~65534) (int)                                                       | 0 |
| len  | 데이터 개수 (1~127) (int)                                                     | 10 |
| wait  | timeout 시간 (sec) (double), 지정하지 않으면 무한대기                                                       | 3.0 |
| var  | int형 배열 변수, 지정하지 않으면 제어기의 슬레이브 어드레스와 매칭됨                                                        | arr |

<br>

#### <mark style="color:green;">샘플 프로그램</mark>


``` python
Hyundai Robot Job File; { version: 2.0, mech_type: "", total_axis: -1, aux_axis: -1 }
     var arr # 지역변수 정의
     arr=Array(20) # int타입의 배열 변수로 정의
     modbus _sci2,sid=1,fc=3,addr=0,len=10,wait=3.0,var=arr # address 0부터 10개의 데이터를 읽어 arr변수에 대입
     print arr[0] # arr변수의 0번 인덱스 값을 출력
     delay 2 #시간 지연
     arr[0]=arr[0]+1 # arr변수의 0번 인덱스 값에 1을 더하기
     modbus _sci2,sid=1,fc=16,addr=0,len=10,wait=3.0,var=arr # address 0부터 10개의 데이터에 arr변수 값을 쓰기
     delay 2 #시간 지연
     end

```

