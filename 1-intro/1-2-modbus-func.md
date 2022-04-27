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

![](<../_assets/image_5.png>)

### <mark style="color:green;">3. 지원 방식</mark>

| **운영 방식** | **시리얼 통신** |               **이더넷 통신**               |
| :-------: | :--------: | :------------------------------------: |
| Master 운영 |  로봇 언어 명령문 |                로봇 언어 명령문               |
|  Slave 운영 |   제어기 설정   | <p>IP: 제어기 설정</p><p>Port: 502 (고정)</p> |



### <mark style="color:green;">4. 전송 모드</mark>

| **운영 방식** |              **시리얼 통신**              | **이더넷 통신** |
| :-------: | :----------------------------------: | :--------: |
| Master 운영 |               binary 모드              |  binary 모드 |
|  Slave 운영 | <p>ASCII 모드</p><p>RTU(binary) 모드</p> |  binary 모드 |



### <mark style="color:green;">5. 지원 펑션</mark>

| **운영 방식** | 　　　　　　　　**시리얼 / 이더넷 통신**                                                                                                                                                                                                                                                                                                                                                                                     |
| :-------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Master 운영 | <ul><li>03: read holding registers (multiple)</li><li>16: write holding registers (multiple)</li></ul>                                                                                                                                                                                                                                                                                                       |
|  Slave 운영 | <ul><li>01: read coils (bits)</li><li>02: read discrete inputs (bits)</li><li>03: read holding registers (multiple)                                                              </li><li>04: read input registers (multiple)</li><li>05: write single coil (bit)</li><li>06: write single holding register</li><li>15: write coils (multiple bits)</li><li>16: write holding registers (multiple)</li></ul> |



### <mark style="color:green;">6. 슬레이브 주소</mark>

* 슬레이브 주소 : 1\~247
* 명령어의 슬레이브 주소가 0인 경우, 설정된 주소와 무관하게 모든 슬레이브가 동작하는 Broadcasting 기능을 지원합니다.

### <mark style="color:green;">7. 시리얼 통신 연결</mark>

* 커넥터 (DSUB - 9 pin : female)

![](../_assets/image5.png)

* 핀맵

![](../_assets/image6.png)

### <mark style="color:green;">8. Address 맵</mark>

![](<../_assets/image_10.png>)

*   상기 표의 기울임 꼴 큰 숫자는 Modbus 에서 사용하는 relay 그룹임

    * MW(data memory for user)
    * DO(digital output)
    * SO(system output)
    * SI(system input)
    * SW(System memory)


*   Data 형식

    Float 형식은 IEEE single-precision 32 bit float point를 사용하고, 8 bit/16 bit/32 bit 들은 전부 signed 정수를 사용함.


*   <mark style="color:red;background-color:yellow;">\*Relay 의 Endian 은 Little Endian 을 사용</mark>

    예, Float 형식인 dof0=6.515625(0x40D08000)인 경우

    dol0=0x4D08000 -> dow0=0x8000, dow2=0x40D0 -> dob0=0x00, dob1=0x80, dob2=0xD0, dob3=0x40

{% hint style="info" %}
Modbus 전송은 16 bit align 의 Big Endian 임.

즉, 상기 전송은 0x80, 0x00, 0x40, 0xD0 의 순서로 전송됨.
{% endhint %}

### <mark style="color:green;">9. SW 메모리 맵</mark>&#x20;

<mark style="color:red;">\*추가된 맵을 사용하기 위해서 당사에 요청하십시오.</mark>

![](<../_assets/image_3.png>)

#### PLC 관련

![](<../_assets/image_9.png>)

#### 소프트 웨어 버전

![](../_assets/image.png)

#### 프로그램 카운터

![](<../_assets/image_8.png>)

#### 통산 가동시간

![](<../_assets/image_2.png>)

#### 로봇 위치

![](<../_assets/image_7.png>)

#### 로봇 속도

![](<../_assets/image_6.png>)

#### 로봇 부하율

![](<../_assets/image_4.png>)

#### 컨베이어 동기

![](<../_assets/image_1.png>)
