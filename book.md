
[__SOURCE](README.md)
# ${cont_model} 로봇제어기 기능설명서 - 모드버스

[__SOURCE](1-intro/README.md)
# 1. 개요


[__SOURCE](1-intro/1-preparatory-information.md)
# 1.1 사전 필요 정보

이 설명서를 이해하기 위해서는 다음과 같은 사전 정보가 필요합니다.

1. **${cont_model} 로봇 제어기 조작 지식**
2. **Modbus 프로토콜 지식**

[__SOURCE](1-intro/2-modbus-func.md)
# 1.2 모드버스 기능

${cont_model} 로봇 제어기는 시리얼 통신과 이더넷 통신에 의한 Modbus 마스터, 슬레이브 기능을 모두 지원합니다.

### <mark style="color:green;">1. MODBUS master 운용 예</mark>

*   **장비 제어**

    MODBUS를 지원하는 장비(ex. Gripper)를 제어할 수 있습니다.

![](../_assets/image2.png)

### <mark style="color:green;">2. MODBUS slave 운용 예</mark>

*   **조작반 기능**

    MODBUS 를 지원하는 저렴한 GP(Grahic Panel)로 한 대 혹은 여러 대의 로봇을 시리얼 또는 이더넷 통신으로 연결하여 사용할 수 있습니다.


*   **PLC 통신**

    MODBUS Master 기능을 갖는 PLC 들과의 통신을 저렴한 Solution 으로 제공합니다.


*   **PC 로봇운영 시스템**

    PC를 이용하여 로봇의 입출력 신호를 모니터링 하거나 제어하는 로봇 운영 시스템을 구축할 수 있습니다.

![](../_assets/image3.png)

### <mark style="color:green;">3. 지원 방식</mark>

| **운영 방식** | **시리얼 통신** |               **이더넷 통신**               |
| :-------: | :--------: | :------------------------------------: |
| Master 운영 |  <p>로봇 언어 명령문</p><p>마스터 쿼리 설정</p> |                <p>로봇 언어 명령문</p><p>마스터 쿼리 설정</p>               |
|  Slave 운영 |   제어기 설정   | <p>제어기 설정</p><p>(3개 추가 가능)</p> |



### <mark style="color:green;">4. 전송 모드</mark>

| **운영 방식** |              **시리얼 통신**              | **이더넷 통신** |
| :-------: | :----------------------------------: | :--------: |
| Master 운영 |               binary 모드              |  binary 모드 |
|  Slave 운영 | <p>ASCII 모드</p><p>RTU(binary) 모드</p> |  binary 모드 |



### <mark style="color:green;">5. 지원 펑션</mark>

| **운영 방식** | 　　　　　　　　**시리얼 / 이더넷 통신**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Master 운영 | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)<li>04: read input registers (multiple)</li></li></ul>                                                                                                                                                                                                                                                                                                       |
|  Slave 운영 | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">6. 슬레이브 주소</mark>

* 슬레이브 주소 : 1\~247

<br>

### <mark style="color:green;">7. Address 맵</mark>

![](<../_assets/image_10.png>)

*   상기 표의 기울임 꼴 큰 숫자는 Modbus 에서 사용하는 relay 그룹임

    * MW(data memory for user)
    * RW(auxiliary memory for user)
    * KW(keep memory for user)
    * DO(digital output)
    * SO(system output)
    * SI(system input)
    * SW(System memory)
    * Y(output relay)
    * X(input relay)


*   Data 형식

    Float 형식은 IEEE single-precision 32 bit float point를 사용하고, 8 bit/16 bit/32 bit 들은 전부 signed 정수를 사용함.


*   <mark style="color:red;background-color:yellow;">\*Relay 의 Endian 은 Little Endian 을 사용</mark>

    예, Float 형식인 dof0=6.515625(0x40D08000)인 경우

    dol0=0x4D08000 -> dow0=0x8000, dow2=0x40D0 -> dob0=0x00, dob1=0x80, dob2=0xD0, dob3=0x40

{% hint style="info" %}
Modbus 전송은 16 bit align 의 Big Endian 임.

즉, 상기 전송은 0x80, 0x00, 0x40, 0xD0 의 순서로 전송됨.
{% endhint %}

### <mark style="color:green;">8. SW 메모리 맵</mark>&#x20;

<mark style="color:red;">시스템 내부적으로 정의된 정보로 이에 대한 세부 내용은 "[내장 PLC 사용 설명서](https://hrbook-hrc.web.app/#/view/doc-hi6-embedded-plc/korean/README)"를 참고하십시오.</mark>
[__SOURCE](2-modbus-rtu/README.md)
# 2. 모드버스 시리얼


[__SOURCE](2-modbus-rtu/1-serial-cable-connect.md)
# 2.1 시리얼 케이블 연결

시리얼 케이블은 하기의 그림과 같이 COM2 포트에 직접 연결합니다.

![](../_assets/image7.jpeg)




* 커넥터 (DSUB - 9 pin : female)

![](../_assets/image5.png)

* 핀맵

![](../_assets/image6.png)


[__SOURCE](2-modbus-rtu/2-serial-port-usage-setting.md)
# 2.2 시리얼 포트 용도 설정

**\[설정 > 2: 제어 파라미터 > 3: 시리얼 포트]** 화면의 **\[일반 설정]** 탭에서 
시리얼 포트의 용도를 [MODBUS]로 설정합니다. <br>
**\[모드버스]** 탭이 활성화 됩니다.

![](../_assets/image8.png)



[__SOURCE](2-modbus-rtu/3-modbus-environment-setting.md)
# 2.3 모드버스 환경 설정

모드버스의 세부사항은 **\[모드 버스]** 탭에서 다음과 같이 설정할 수 있습니다.

![](../_assets/image9.png)

*   **동작**: Master 로 운영할지, Slave 운영할지 선택합니다. <br>
    Master로 운영 시 로봇언어 명령어 또는 마스터 설정에 의해 수행되기 때문에 슬레이브 ID와 모드는 사용하지 않습니다. <br>
    Slave로 운영 시 제어기는 마스터 요청에 응답을 수행하기 때문에 화면의 설정만으로 동작합니다. <br>
* **슬레이브 ID**: 모드버스 시리얼의 Slave 로 통신하기 위한 ID 를 설정합니다.
* **모드**: 모드버스 시리얼의 Slave 로 통신하기 위한 mode를 설정합니다.

[__SOURCE](3-modbus-tcp/README.md)
# 3. 모드버스 TCP

모드버스 TCP 통신에 의한 슬레이브 또는 마스터 동작을 위한 이더넷 설정에 관한 내용입니다. 

[__SOURCE](3-modbus-tcp/1-network-setting.md)
# 3.2 네트워크 설정

이더넷 통신을 사용하기 위해서는  **\[시스템 > 2: 제어 파라미터 > 9: 네트워크 > 1: 사용환경 설정]** 화면에서 연결된 LAN 포트에 해당하는 네트워크 주소를 설정하는 것이 가장 먼저입니다.

![](../_assets/image31.png)



[__SOURCE](3-modbus-tcp/2-slave-setting.md)
# 3.2 모드버스 슬레이브 설정

**\[시스템 > 2: 제어 파라미터 > 9: 네트워크 > 2: 서비스 > 1: 모드버스 슬레이브]** 화면에서 설정합니다. <br>
"+"버튼을 사용하여 슬레이브는 최대 3개까지 사용할 수 있으며 또한 현재 통신 상태에 대한 모니터링이 가능합니다. <br>
Slave로 운영 시 제어기는 마스터 요청에 응답을 수행하기 때문에 화면의 설정만으로 동작합니다. <br>

![](../_assets/image30.png)

*   **포트 번호**

    MODBUS TCP 통신을 위한 포트를 설정합니다. 각각의 슬래이브는 반드시 서로 다른 포트 번호로 설정되어야 합니다.


*   **타임 아웃**

    MODBUS TCP 통신 연결 상태를 확인하기 위한 시간을 설정합니다. 마스터에서 지정된 시간동안 서비스 요청이 없으면 강제로 연결을 종료합니다.


*   **최대 연결**

    각각의 슬래이브에 연결할 수 있는 마스터의 최대 연결 수를 설정합니다. 현재는 최대 3개까지 가능합니다.

[__SOURCE](3-modbus-tcp/3-enet-comm-setting.md)
# 3.3 이더넷 통신 설정

모드버스 TCP 마스터 동작을 수행하기 이전에 먼저 이더넷 통신 객체를 생성하고 설정해야 합니다. <br>

**\[시스템 > 2: 제어 파라미터 > 9: 네트워크 > 2: 서비스 > 4: 이더넷 통신]** 화면에서 설정합니다. <br>
"+"버튼을 사용하여 이더넷 객체는 최대 5개까지 생성하여 사용할 수 있으며 또한 현재 통신 상태에 대한 모니터링이 가능합니다. <br>
모드버스 마스터로 동작하기 위해서는 본 화면의 설정과 별도로 마스터를 실행해야 합니다. ([4. 모드버스 마스터] 참조) <br>

![](../_assets/image32.png)

[닫기] 버튼으로 해당 이더넷 객체의 소켓을 강제로 닫을 수 있으며 [연결] 버튼으로 통신 연결을 수행할 수 있습니다. <br>
제어기 부팅시에는 설정된 이더넷 객체로 통신 연결을 자동으로 수행합니다. <br>


*   **이름**

    이더넷 통신 객체의 이름입니다. 각각의 이름은 반드시 "enet0" ~ "enet4"로 설정되어야 합니다.


*   **프로토콜**

    통신 프로토콜을 선택합니다. MODBUS TCP 마스터 동작을 위해서는 반드시 "TCPc"(TCP client)로 설정되어야 합니다.


*   **IP 주소**

    슬레이브에서 사용된 IP 주소를 설정합니다. 


*   **로컬 포트**

    지역 포트 번호를 설정합니다. 모드버스 통신은 기본값으로 502번 포트를 사용합니다.


*   **원격 포트**

    원격 포트 번호를 설정합니다. 모드버스 통신은 기본값으로 502번 포트를 사용합니다.


*   **상태**

    통신 연결 상태를 표시합니다. 

[__SOURCE](4-modbus-master/README.md)
# 4. 모드버스 마스터

모드버스 마스터를 동작하기 위해서는 로봇언어 명령어로 실행하는 방법과 사용자가 설정화면에서 쿼리를 등록하여 실행하는 방법이 있습니다. 

[__SOURCE](4-modbus-master/1-roblang.md)
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


[__SOURCE](4-modbus-master/2-setup.md)
# 4.2 쿼리설정으로 동작

사용자가 모드버스 마스터 쿼리를 설정하면 이를 기반으로 자동으로 슬레이브에 전송할 수 있습니다. <br>
구성된 쿼리에서 해당 쿼리를 실행할 때 데이터 송수신이 이루어집니다. <br>
<br>


**\[시스템 > 2: 제어 파라미터 > 9: 네트워크 > 2: 서비스 > 5: 모드버스 마스터]** 화면에서 설정합니다. <br>
"+"버튼을 사용하여 쿼리를 추가할 수 있으며 또한 현재 쿼리 실행 상태에 대한 모니터링이 가능합니다. <br>

![](../_assets/image33.png)

[정지] 버튼으로 마스터 실행을 강제로 정지할 수 있으며 [실행] 버튼으로 다시 실행할 수 있습니다. <br>
제어기 부팅후에는 설정된 쿼리가 순차적으로 자동으로 실행됩니다. <br>


*   **이름**

    쿼리 객체의 이름입니다. 각각의 이름은 반드시 "query_?"로 설정되어야 합니다.


*   **통신 방식**

    이더넷 통신 또는 시리얼 통신의 통신 방식을 선택합니다.


*   **객체 번호**

    이더넷 통신의 경우 [3.3 이더넷 통신]에 설정된 enet 번호를 설정합니다. <br> 
    시리얼 통신의 경우는 현재 2만 사용이 가능합니다. 


*   **슬레이브 ID**

    슬레이브 ID(1~247)를 설정합니다.


*   **기능**

    펑션코드를 설정합니다. <br>
    F03 : read holding registers <br>
    F16 : write holding registers <br>
    F04 : read input registers <br>


*   **시작 주소**

    슬레이브의 시작주소(0~65534)를 설정합니다. 


*   **길이**

    데이터 개수(1~127)를 설정합니다. 


*   **타임 아웃**

    타임아웃 시간(sec)을 설정합니다. 0으로 설정하면 무한대기 합니다. 


*   **릴레이/주소**

    릴레이(데이터 메모리/입출력신호 등) 명칭이나 모드버스 슬래이브 주소로 설정합니다. 


*   **지연 시간**

    해당 쿼리가 실행된 후 다음 쿼리가 실행하기까지의 지연시간을 설정합니다. 


*   **상태**

    동작중 쿼리의 실행 상태를 로봇언어 명령어로 구성하여 표시합니다. 
