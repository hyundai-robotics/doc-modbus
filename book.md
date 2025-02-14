# Hi6 로봇제어기 기능설명서 - 모드버스

{% hint style="warning" %}
본 제품 설명서에서 제공되는 정보는 현대로보틱스의 자산입니다.

현대로보틱스의 서면에 의한 동의 없이 전부 또는 일부를 무단 전재 및 재배포할 수 없으며, 제3자에게 제공되거나 다른 목적에 사용할 수 없습니다.



본 설명서는 사전 예고 없이 변경될 수 있습니다.



**Copyright ⓒ 2020 by Hyundai Robotics**
{% endhint %}
# 1. 개요

# 1.1 사전 필요 정보

이 설명서를 이해하기 위해서는 다음과 같은 사전 정보가 필요합니다.

1. **Hi6 로봇 제어기 조작 지식**
2. **Modbus 프로토콜 지식**
# 1.2 모드버스 기능

Hi6 로봇 제어기는 시리얼 통신과 이더넷 통신에 의한 Modbus 마스터, 슬레이브 기능을 모두 지원합니다.

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
| Master 운영 | <p>제어기 설정</p> | <p>제어기 설정</p>               |
|  Slave 운영 |   제어기 설정   | <p>IP: 제어기 설정</p><p>Port: 502(고정), 2개 추가 가능 </p> |


### <mark style="color:green;">4. 모드버스 TCP 슬래이브 설정</mark>
    시스템/제어 파라미터/네트워크/서비스/모드버스 슬래이브 메뉴에서 슬래이브를 최대 3개까지 추가할 수 있습니다. 또한 현재 통신 상태에 대한 모니터링이 가능합니다.

![](../_assets/image30.png)

*   **포트 번호**

    MODBUS TCP 통신을 위한 포트를 설정합니다. 각각의 슬래이브는 반드시 서로 다른 포트 번호로 설정되어야 합니다.


*   **타임 아웃**

    MODBUS TCP 통신 연결 상태를 확인하기 위한 시간을 설정합니다.


*   **최대 연결**

    각각의 슬래이브에 연결할 수 있는 마스터의 최대 연결 수를 설정합니다. 현재는 최대 3개까지 가능합니다.


### <mark style="color:green;">5. 전송 모드</mark>

| **운영 방식** |              **시리얼 통신**              | **이더넷 통신** |
| :-------: | :----------------------------------: | :--------: |
| Master 운영 |               binary 모드              |  binary 모드 |
|  Slave 운영 | <p>ASCII 모드</p><p>RTU(binary) 모드</p> |  binary 모드 |



### <mark style="color:green;">6. 지원 펑션</mark>

| **운영 방식** | 　　　　　　　　**시리얼 / 이더넷 통신**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Master 운영 | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|  Slave 운영 | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">7. 슬레이브 주소</mark>

* 슬레이브 주소 : 1\~247
* 명령어의 슬레이브 주소가 0인 경우, 설정된 주소와 무관하게 모든 슬레이브가 동작하는 Broadcasting 기능을 지원합니다.

### <mark style="color:green;">8. 시리얼 통신 연결</mark>

* 커넥터 (DSUB - 9 pin : female)

![](../_assets/image5.png)

* 핀맵

![](../_assets/image6.png)

### <mark style="color:green;">9. Address 맵</mark>

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

### <mark style="color:green;">10. SW 메모리 맵</mark>&#x20;

<mark style="color:red;">\*시스템 내부적으로 정의된 정보로 이에 대한 세부 내용은 "내장 PLC 사용 설명서"를 참고하십시오.</mark>
# 2. 시리얼 통신 설정

# 2.1 시리얼 케이블 연결

시리얼 케이블은 하기의 그림과 같이 COM2 포트에 직접 연결합니다.

![](../_assets/image7.jpeg)
# 2.2 시리얼 포트 용도 설정

시리얼 포트의 용도를 MODBUS 로 설정하는 것은 **\[설정 > 2: 제어 파라미터 > 3: 시리얼 포트]** 화면의 **\[일반 설정]** 탭에서 다음과 같이 설정할 수 있습니다.

![](../_assets/image8.png)


# 2.3 모드버스 환경 설정

모드버스의 세부사항은 **\[모드 버스]** 탭에서 다음과 같이 설정할 수 있습니다.

![](../_assets/image9.png)

*   **동작**: Master 로 운영할지, Slave 운영할지 선택합니다.

    Master로 운영 시 로봇언어 명령어에 의해 수행되기 때문에 슬레이브 ID와 모드는 사용하지 않습니다.
* **슬레이브 ID**: 모드버스 시리얼의 Slave 로 통신하기 위한 ID 를 설정합니다.
* **모드**: 모드버스 시리얼의 Slave 로 통신하기 위한 mode를 설정합니다.
# 3. 마스터 운영
Hi6 개방화 플랫폼의 모드버스 마스터 app 은 현재 지원하지 않습니다.