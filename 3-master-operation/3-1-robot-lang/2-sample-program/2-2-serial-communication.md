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
