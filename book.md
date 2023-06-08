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
| Master 운영 |  <p>로봇 언어 명령문</p><p>제어기 설정</p> |                <p>로봇 언어 명령문</p><p>제어기 설정</p>               |
|  Slave 운영 |   제어기 설정   | <p>IP: 제어기 설정</p><p>Port: 502, 5000, 5001(고정)</p> |



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

### <mark style="color:green;">9. SW 메모리 맵</mark>&#x20;

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
모드버스 마스터의 경우 Hi6 개방화 플랫폼의 app 형태로 개발되었습니다.

app의 정보는 **\[서비스 > 10: 앱(app) ]** 에서 위치를 Main 으로 변경 후 확인할 수 있습니다.

![](../_assets/image29.png)# 3.1 로봇 언어

로봇언어 명령문을 사용하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. 해당 명령문을 실행할 때 데이터 송수신이 이루어지며 만약 주기적으로 데이터를 송수신하는 것이 필요하다면 제어기 내부 설정을 통하여 통신할 수 있습니다.
# 3.1.1 명령어

로봇 언어로 모드버스 마스터 통신을 하는 명령문 입니다.
모드버스 통신에 대한 이해를 위해서 별도로 학습하시기 바랍니다.# 3.1.1.1 모드버스 마스터 생성자: Modbus

모드버스 마스터를 선언 하는 명령문 입니다.

해당 마스터를 활용하여 통신을 수행할 수 있으므로 읽기/쓰기 이전에 선언되어야합니다.

반드시 사용하시기 전에 modbus 모듈을 가져와서 사용하시길 바랍니다. (예: import modbus)
#### <mark style="color:green;">문법</mark>

```
var master = modbus.Modbus(protocol,port,ip_addr)
```
#### <mark style="color:green;">리턴값</mark>
- 모드버스 마스터 객체

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| protocol | <p>통신프로토콜 타입(str)</p><ul><li>tcp</li><li>rtu</li></ul>                                         | "tcp" or "rtu" |
|   port  | <p>통신 포트(int)</p><ul><li>tcp: tcp 포트 번호 </li><li>rtu: 시리얼 포트번호 (0~2)</li></ul>        |  502(tcp) or 2(rtu)   |
| ip_addr  | [tcp만 사용] 모드버스 TCP 슬레이브 장치 IP 주소(str)                                                       | "192.168.1.1" |



# 3.1.1.2 데이터 읽기: read(function code: 03)

슬레이브 장치의 레지스터 데이터를 읽기 위한 명령문 입니다.

슬레이브의 레지스터 주소를 시작으로 레지스터 길이만큼을 읽어옵니다. 

앞서 선언한 마스터를 가져와 수행하게 됩니다.

#### <mark style="color:green;">문법</mark>

```
var value = master.read(salve_id,addr,length,timeout,addr_on_timeout)
```

#### <mark style="color:green;">리턴값</mark>
- 송신 데이터(array type)

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| slave_id | 슬레이브 장치 id (int)                                         | <p>1</p>(0~255) |
|   addr  | <p>슬레이브 레지스터 시작 주소 (int)</p> (use devcimal numbers, not hex)      |  0  |
|  length  | 슬레이브 레지스터 길이 (int)                                                        | 3 |
|  timeout    |[선택사항]  타임아웃 (int), 단위: msec                                                      | 5000 |
|  addr_on_timeout    |[선택사항]  타임아웃 시 퇴피주소 (int or str)                                                      | 99 or S1 |

# 3.1.1.3 데이터 쓰기: write(function code: 16)

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


# 3.1.1.4 모드버스 마스터 상태: status

모드버스 통신 상태를 확인 하기 위한 명령문 입니다.
앞서 선언한 마스터를 가져와 수행하게 됩니다.

#### <mark style="color:green;">문법</mark>

```
var status = master.staus
```

#### <mark style="color:green;">리턴값</mark>
- 모드버스 통신 상태

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    
| :---: | ------------------------------------------------------------------------------------------------------- 
|   state  | <p>통신상태</p><ul><li>초기상태 (연결 안됨) = 0</li><li>정상 = 1</li><li>통신에러 = -1</li><li>시간초과 에러 = -2</li></ul>    | 


# 3.1.2 샘플 프로그램

# 3.1.2.1 이더넷 통신

하기는 onRobot gripper 를 제어하기 위한 샘플 프로그램으로 Hi6 제어기와 onRobot gripper 는 Modbus tcp 로 통신합니다. 여기서 Hi6 제어기는 master 로 운영되며 gripper 가 slave 로 운영됩니다.

```python
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
    
     # modbus 모듈 import 후, 생성자로 modbus master 객체 생성(통신 타입 및 포트 번호, ip 주소 설정) 
     import modbus
     var master=modbus.Modbus("tcp",502,"192.168.1.1")

     # 전송 데이터
     global arr
     arr=Array(300)
     arr[0]=300 #force(0~400)
     arr[1]=200 #width (0~1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)
     
     # write 동작 수행, (선택 옵션) 3000msec 초과시 99행으로 분기 처리
     master.write(65,0,3,arr,3000,99)

     # 통신 상태 확인 (0: 연결안됨(초기상태), 1: 정상 상태, -1: 통신 실패, -2: 타임아웃 에러 )
     if master.status<0
       print "write communication error"
       stop
     endif

     # read 동작 수행, (선택 옵션) 3000msec 초과시 99행으로 분기 처리
     var recv_data=master.read(65,267,2,3000,99)
     if master.status<0
       print "read communication error"
       stop
     endif

     # 수신 데이터 처리
     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end

  99 print "timeout"



```

# 3.1.2.2 시리얼 통신

하기는 onRobot gripper를 시리얼 통신으로 제어한다고 가정하였을 때 샘플 프로그램입니다. 먼저 시리얼 통신 설정에서 포트 용도를 **\[MODBUS]**로 모드버스의Operation은 **\[master]** 로 설정되어야 합니다.


``` python
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
     
     # modbus 모듈 import 후, 생성자로 modbus master 객체 생성(통신 타입 및 포트 번호 설정) 
     import modbus
     var master=modbus.Modbus("rtu",2)
     
     # 전송 데이터
     global arr
     arr=Array(300)
     arr[0]=300 #force(0~400)
     arr[1]=200 #width (0~1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)
     
     # write 동작 수행, (선택 옵션) 3000msec 초과시 99행으로 분기 처리
     master.write(65,0,3,arr,3000,99)
     
     # 통신 상태 확인 (0: 연결안됨(초기상태), 1: 정상 상태, -1: 통신 실패, -2: 타임아웃 에러 )
     if master.status<0
       print "write communication error"
       stop
     endif
     
     # read 동작 수행, (선택 옵션) 3000msec 초과시 99행으로 분기 처리
     var recv_data=master.read(65,267,2,3000,99)
     if master.status<0
       print "read communication error"
       stop
     endif

     # 수신 데이터 처리
     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end

  99 print "timeout"

```
# 3.2 제어기 설정

제어기에서의 설정을 통하여 모드버스 마스터 쿼리를 구성하고 이를 슬레이브에 전송할 수 있습니다. Scan Rate를 설정함으로써 주기적으로 데이터를 송수신하는 것이 가능합니다. 해당 설정을 통하여 일대일 마스터와 슬레이브 설정이 가능합니다.

모드버스 마스터를 설정하는 것은 **\[설정 > 2: 제어 파라미터 > Modbus]** 화면에서 통신 방식에 따라 설정할 수 있습니다.

설정파라미터가 보여지지 않는 경우 **\[서비스 > 10: 앱(app) > MAIN]** 화면에서 시작 및 상태를 확인 후 가동시켜 주십시오. 
# 3.2.1 모드버스 TCP 마스터 설정

모드버스 TCP 마스터 쿼리를 구성하고 이를 슬레이브에 전송하는 설정입니다.

해당 마스터를 활용하여 통신을 수행할 수 있습니다.


모드버스 TCP 마스터를 설정하는 것은 **\[설정 > 2: 제어 파라미터 > Modbus > 1: Modbus TCP Master settings]** 화면에서 설정할 수 있습니다.

{% hint style="warning" %}
변경된 파라미터로 통신하기 위해서는 [확인] 혹은 [적용(shift + 확인)] 을 통해 변경 내용을 반영하신 뒤에 올바른 작업을 하실 수 있습니다.
{% endhint %}

![](../../_assets/image27.png)

<li>IP address: 슬레이브 장치 IP 주소</li>
<li>Port: TCP 통신 포트 번호 (기본값:502)</li>
<li>Salve ID: 슬레이브 장치 ID </li>
<li>Function code: 함수 코드 (03: Read Holding Registers /16: Write Multiple registers)
<li>Address: 슬레이브 레지스터 시작 주소</li>
<li>Length: 슬레이브 레지스터 길이</li>
<li>Scan Rate: 통신 주기 (단위: 5msec) </li>
<li>Value: 03: 읽어오는 데이터 값/16: 쓰여지는 데이터 값 (예) 1개 데이터: 0 / 3개 데이터 : 0,1,2 </li>
<li>Timeout: 타임 아웃 (단위:msec) </li>
<li>Connect: 연결 및 데이터 송수신 시작</li>
<li>Reset: 선택된 파라미터의 이전 저장 값으로 초기화 & value(read)의 경우, 읽어 온 값 갱신 </li>
<li>ResetAll: 모든 파라미터들의 이전 저장 값으로 초기화</li>

# 3.2.2 모드버스 RTU 마스터 설정

모드버스 RTU 마스터 쿼리를 구성하고 이를 슬레이브에 전송하는 설정입니다.

해당 마스터를 활용하여 통신을 수행할 수 있습니다.


모드버스 RTU 마스터를 설정하는 것은 **\[설정 > 2: 제어 파라미터 > Modbus > 2: Modbus RTU Master settings]** 화면에서 설정할 수 있습니다.

{% hint style="warning" %}
변경된 파라미터로 통신하기 위해서는 [확인] 혹은 [적용(shift + 확인)] 을 통해 변경 내용을 반영하신 뒤에 올바른 작업을 하실 수 있습니다.
{% endhint %}

![](../../_assets/image28.png)

<li>Serial port: 시리얼 통신 포트 번호 (기본값: 2)</li>
<li>Salve ID: 슬레이브 장치 ID </li>
<li>Function code: 함수 코드 (03: Read Holding Registers /16: Write Multiple registers)
<li>Address: 슬레이브 레지스터 시작 주소</li>
<li>Length: 슬레이브 레지스터 길이</li>
<li>Scan Rate: 통신 주기 (단위: 5msec) </li>
<li>Value: 03: 읽어오는 데이터 값/16: 쓰여지는 데이터 값 (예) 1개 데이터: 0 / 3개 데이터 : 0,1,2 </li>
<li>Timeout: 타임 아웃 (단위:msec) </li>
<li>Connect: 연결 및 데이터 송수신 시작</li>
<li>Reset: 선택된 파라미터의 이전 저장 값으로 초기화 & value(read)의 경우, 읽어 온 값 갱신 </li>
<li>ResetAll: 모든 파라미터들의 이전 저장 값으로 초기화</li>