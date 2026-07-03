# 3.3 이더넷 통신 설정

모드버스 TCP 마스터 동작을 수행하기 이전에 먼저 이더넷 통신 객체를 생성하고 설정해야 합니다. <br>

이에 대한 내용은 별도의 "[${cont_model} 제어기 조작 설명서 - TP630](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-tp630/7-system/3-control-parameter/9-network-setting/2-service/4-enet-comm-setting?cont_model=${cont_model})"를 참조하십시오. 

*   **프로토콜**

    MODBUS TCP 마스터 동작을 위해서는 반드시 "TCPc"(TCP client)로 반드시 설정되어야 합니다.


*   **로컬 포트**

    모드버스 통신은 기본값으로 502번 포트를 사용합니다.


*   **원격 포트**

    모드버스 통신은 기본값으로 502번 포트를 사용합니다.

<br>
모드버스 마스터로 동작하기 위해서는 이와 별도로 마스터를 실행해야 합니다. 

([4. 모드버스 마스터](../4-modbus-master/README.md) 참조)
