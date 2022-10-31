﻿# 3.1.1.4 모드버스 마스터 상태: status

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

