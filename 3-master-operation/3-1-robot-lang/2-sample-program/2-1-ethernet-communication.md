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
