# 3.1.2.2 시리얼 통신

하기는 onRobot gripper를 시리얼 통신으로 제어한다고 가정하였을 때 샘플 프로그램입니다. 먼저 시리얼 통신 설정에서 포트 용도를 **\[MODBUS]**로 모드버스의Operation은 **\[master]** 로 설정되어야 합니다.

{% code title="0060.job" %}
```
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
     import modbus
     var master=modbus.Modbus("rtu",2)
     
     global arr
     arr=Array(300)
     arr[0]=300 #force(0~400)
     arr[1]=200 #width (0~1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)

     master.write(65,0,3,arr,3000,99)
     var status=master.status
     if status<0
       print "write communication error"
       stop
     endif

     var recv_data=master.read(65,267,2,3000,99)
     status=master.status
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
{% endcode %}
