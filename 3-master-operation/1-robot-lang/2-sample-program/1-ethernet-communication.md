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

