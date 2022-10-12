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
