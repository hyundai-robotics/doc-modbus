# 4.1 로봇언어로 동작

로봇언어 명령문을 사용하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. <br>
해당 명령문을 실행할 때 데이터 송수신이 이루어집니다. <br>
<br>

#### <mark style="color:green;">문법</mark>

```
modbus <통신 방법>,sid=<슬레이브ID>,fc=<펑션코드>,addr=<시작 주소>,len=<길이>,wait=<timeout 시간>,var=<송수신 데이터>,timeout_addr=<timeout 주소>
```

#### <mark style="color:green;">파라미터</mark>

| 항목 | 의미 | 예 | 기타 |
| - | - | - |-|
|통신 방법     |통신방법 설정(시리얼 or 이더넷)|"_sci2" or "_enet0"|시리얼포트 또는 이더넷객체 |
|슬레이브ID    |슬레이브의 ID|1|1~247|
|펑션코드      |펑션코드|3,16|[1.2.5 지원 펑션](../1-intro/2-modbus-func.md) 참고|
|시작주소      |슬레이브의 시작 주소|0|0~65534|
|길이          |데이터의 개수|10|words : 1~127, bits : 1~2000|
|timeout 시간  |지정한 시간 이상 소요 시 timeout 처리|3.0|지정하지 않으면 무한대기|
|송수신 데이터 |수신 시 대입할 변수/송신 시 송신할 변수|arr / _mw0 / fb1.diw0|데이터 메모리, 입력신호, 출력신호|
|timeout 주소 |timeout 시 분기할 주소|S1 / *timeout / 10|주소. 시간만 지정된 상태에서 timeout 시 에러|

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

