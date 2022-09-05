# 3.1.1.1 마스터 선언: Modbus

모드버스 마스터를 선언 하는 명령문 입니다.

해당 마스터를 활용하여 통신을 수행할 수 있으므로 읽기/쓰기 이전에 선언되어야합니다.

반드시 사용하시기 전에 modbus 모듈을 가져와서 사용하시길 바랍니다. (예: import modbus)
#### <mark style="color:green;">문법</mark>

```
var master = modbus.Modbus(protocol,port,ip_addr)
```

#### <mark style="color:green;">파라미터</mark>

|파라미터| 설명                                                                                                    |    예    |
| :---: | ------------------------------------------------------------------------------------------------------- | :-------: |
| var master | <p>임의의 변수를 선언하여 모드버스 마스터 객체를 받아옴.</p>(전역변수도 가능)                                         | master |
| protocol | <p>통신프로토콜 타입(str)</p><ul><li>tcp</li><li>rtu</li></ul>                                         | "tcp" or "rtu" |
|   port  | <p>통신 포트(int)</p><ul><li>tcp: tcp 포트 번호 </li><li>rtu: 시리얼 포트번호 (0~2)</li></ul>        |  502(tcp) or 2(rtu)   |
| ip_addr  | [tcp만 사용] 모드버스 TCP 슬레이브 장치 IP 주소(str)                                                       | "192.168.1.1" |

