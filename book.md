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
# 2. 시리얼 통신 설정

# 2.1 시리얼 케이블 연결

시리얼 케이블은 하기의 그림과 같이 COM2 포트에 직접 연결합니다.

![](../_assets/image7.jpeg)
# 2.2 시리얼 포트 용도 설정

시리얼 포크의 용도를 MODBUS 로 설정하는 것은 **\[설정 > 2: 제어 파라미터 > 3: 시리얼 포트]** 화면의 **\[일반 설정]** 탭에서 다음과 같이 설정할 수 있습니다.

![](../_assets/image8.jpeg)
# 2.3 모드버스 환경 설정

모드버스의 세부사항은 **\[모드 버스]** 탭에서 다음과 같이 설정할 수 있습니다.

![](../_assets/image9.jpeg)

*   **동작**: Master 로 운영할지, Slave 운영할지 선택합니다.

    Master로 운영 시 로봇언어 명령어에 의해 수행되기 때문에 슬레이브 ID와 모드는 사용하지 않습니다.
* **슬레이브 ID**: 모드버스 시리얼의 Slave 로 통신하기 위한 ID 를 설정합니다.
* **모드**: 모드버스 시리얼의 Slave 로 통신하기 위한 mode를 설정합니다.
# 3. 마스터 운영

# 3.1 로봇 언어

로봇언어 명령문을 사용하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. 해당 명령문을 실행할 때 데이터 송수신이 이루어지며 만약 주기적으로 데이터를 송수신하는 것이 필요하다면 이를 위한 작업 프로그램을 구성한 후 멀티 태스크로 구동할 수도 있겠지만 이 경우는 내장 PLC를 통하여 통신하는 것을 권장합니다.
# 3.1.1 명령어 (모드버스)

로봇 언어로 모드버스 마스트 통신을 하는 명령문 입니다.

모드버스 통신에 대한 이해를 위해서 별도로 학습하시기 바랍니다.

#### <mark style="color:green;">문법</mark>

```
modbus enet2,sid=65,fc=16,addr=0,len=3,wiat=3,var=arr
```

#### <mark style="color:green;">파라미터</mark>

| enet2 | 통신 객체 (이더넷 또는 시리얼)                                                                                      |           |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
|  sid  | 슬레이브 id                                                                                                 |  1 \~ 255 |
|   fc  | <p>펑션 코드</p><ul><li>03 = read holding register(multiple)</li><li>16 = write multiple register</li></ul> |   03, 16  |
|  addr | 시작 주소                                                                                                   | 0 \~65534 |
|  len  | 데이터 길이                                                                                                  |  1 \~ 127 |
|  wait | 통신 대기시간                                                                                                 |           |
|  var  | <p>데이터 송수신을 위한 배열 변수</p><p>(미지정시 자체 모드버스 맵이 사용됨)</p>                                                    |           |
# 3.1.2 샘플 프로그램

# 3.1.2.1 이더넷 통신

하기는 onRobot gripper 를 제어하기 위한 샘플 프로그램으로 Hi6 제어기와 onRobot gripper 는 Modbus tcp 로 통신합니다. 여기서 Hi6 제어기는 master 로 운영되며 gripper 가 slave 로 운영됩니다.

{% code title="0060.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
call 61,1 # onRobot module open
call 61,2,0 # onRobot gripper hold
delay(3)
call 61,2,300 # onRobot gripper release
call 61,0 # onRobot module close
delay(3)
end
```
{% endcode %}

{% code title="0061.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
param mode,grip
if (mode == 1) # enet module open
    import enet
    global enet2,arr
    if (arr==0)
        arr=Array(5)
    endif
    # onRobot gripper enet connect
    enet2=enet.ENet("tcp") #udp,tcp
    enet2.ip_addr="192.168.1.111" #OnRonot IP
    enet2.lport=502
    enet2.rport=502
    if (enet2.state() < 1)
        enet2.open
        enet2.connect #tcp인 경우
    else
        stop
    endif
    print "enet2.state", enet2.state()
elseif (mode == 0) # enet module close
    enet2.close
else # onRobot gripper operate
    arr[0] = 300 # force (0~400)
    arr[1] = grip # width (0~1100)
    arr[2] = 1 # control (1:grip, 8=stop, 16=offset grip) 
    modbus enet2,sid=65,fc=16,addr=0,len=3,wait=3,var=arr
endif
end
```
{% endcode %}
# 3.1.2.2 시리얼 통신

하기는 onRobot gripper를 시리얼 통신으로 제어한다고 가정하였을 때 샘플 프로그램입니다. 먼저 시리얼 통신 설정에서 포트 용도를 **\[MODBUS]**로 모드버스의Operation은 **\[master]** 로 설정되어야 합니다.

{% code title="0060.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
call 61,1 # onRobot module open
call 61,2,0 # onRobot gripper hold
delay(3)
call 61,2,300 # onRobot gripper release
call 61,0 # onRobot module close
delay(3)
end
```
{% endcode %}

{% code title="0061.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "780(YL012-0D)", total_axis: 6, aux_axis: 0 }
param mode,grip
if (mode == 1) # enet module open
    global sci2,arr
    if (arr==0)
        arr=Array(5)
    endif
    # onRobot gripper enet connect
    sci2=com.Sci(2) # serial port 2 object
elseif (mode == 0) # enet module close
    print “sci close”
else # onRobot gripper operate
    arr[0] = 300 # force (0~400)
    arr[1] = grip # width (0~1100)
    arr[2] = 1 # control (1:grip, 8=stop, 16=offset grip) 
    modbus sci2,sid=65,fc=16,addr=0,len=3,wait=3,var=arr
endif
end

```
{% endcode %}
# 3.2 내장 PLC

내장 PLC 래더 로직에 의해 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. 이를 사용하기 위해서는 내장 PLC 에 대한 지식이 필요하며 관련 설명서를 참고하십시오.
# 3.2.1 Native firmware library 추가

하기의 그림과 같이 **\[Libraies > Firmware Libray…]** 에서 “**Hi6\_FWLib.fwl**” 파일을 선택하여 추가합니다.

![](../../_assets/image10.png) ![](../../_assets/image11.png)

Edit Wizard 에서 ENET 통신과 관련된 펑션 블록들과 MODBUS\_TCP 와 관련된 펑션 블록이 추가된 것을 확인할 수 있습니다.

![](../../_assets/image12.png) ![](../../_assets/image13.png)
# 3.2.2 펑션 블록 추가

하기 그림과 같이 **\[Logical POUs > Insert > Function Block...]** 에서 Function Block을 추가합니다.

![](../../_assets/image14.png)

본 예제에서는 modbus라는 이름으로 ST 언어를 사용하겠다고 선택하였습니다. ****&#x20;

![](../../_assets/image15.png)
# 3.2.3 변수 추가

하기의 그림과 같이 변수 관리 화면에서 변수를 등록합니다.

![](../../_assets/image16.png)
# 3.2.4 펑션 블록 작성

하기의 그림과 같이 프로그램 작성 화면에서 사용자는 프로그램을 작성합니다.

#### <mark style="color:green;">FBD 언어</mark>

![](../../_assets/image17.png)

#### <mark style="color:green;">ST 언어</mark>

![](../../_assets/image18.png)
# 3.2.5 샘플 펑션 블록

#### <mark style="color:green;">FBD 언어</mark>

![](../../_assets/image19.png)

#### <mark style="color:green;">ST 언어</mark>

![](../../_assets/image20.png)
# 3.2.6 샘플 펑션 블록 설명

* PLC 를 RUN 하면 socket 변수가 0 으로 초기화 되어 있어 enet 통신을 위한 소켓을 자동으로 open 합니다.
* IPaddr 은 연결할 상대 장치의 ip address 를 지정합니다.

![](../../_assets/image21.png)

* connect 변수를 1 로 설정하면 슬레이브 장치와 연결 동작을 수행한 후 connect 변수를 0 으로 변경합니다.

![](../../_assets/image22.png)

* query 변수를 1 로 설정하면 Function:=3, Address:=0, Length:=3 에 의해 슬레이브에서 0 번지부터 3 개의 데이터를 얻어와 data의 배열 변수로 전달합니다. (read)
* Result 가 1 이면 query 를 2 로 설정하고 data\[0]의 변수 값을 1 증가 합니다.

![](../../_assets/image23.png)

* query 변수가 2 이면  Function:=16, Address:=0, Length:=3 에 의해 data 배열 변수에 설정된 값을 슬레이브의 0 번지부터 3 개의 데이러를 설정합니다. (write)
* Result가 1 이면 query 를 1 로 설정합니다.

![](../../_assets/image24.png)

* close 변수를 1 로 설정하면 소켓을 닫게 됩니다.

![](../../_assets/image25.png)
# 3.2.7 샘플 펑션 블록 가동

* main 이란 Program POU 에서 샘플로 작성한 펑션 블록을 호출하여 실행합니다.
* main 이란 Program POU 는 CYCLIC task 에서 200 ms 마다 실행합니다.

![](../../_assets/image26.png)
