# 3.1.2.1 이더넷 통신

하기는 onRobot gripper 를 제어하기 위한 샘플 프로그램으로 Hi6 제어기와 onRobot gripper 는 Modbus tcp 로 통신합니다. 여기서 Hi6 제어기는 master 로 운영되며 gripper 가 slave 로 운영됩니다.

```
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
     import modbus
     var master=modbus.Modbus("tcp",502,"192.168.1.11")

     global arr
     arr=Array(300)
     arr[0]=300 #force(0~400)
     arr[1]=200 #width (0~1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)

     master.write(65,0,3,arr,3000,99)
     var status=master.state
     if status<0
       print "write communication error"
       stop
     endif
     var recv_data=master.read(65,267,2,3000,99)
     status=master.state
     if status<0
       print "read communication error"
       stop
     endif

     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end
     
  99 print "timeout"


```

