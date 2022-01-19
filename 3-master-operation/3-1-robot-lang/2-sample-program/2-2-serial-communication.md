# 3.1.2.2 Serial communication

The following shows a sample program when assuming that the onRobot gripper is controlled through serial communication. First, in the serial communication setting, port usage should be set as **\[MODBUS]** , and MODBUS operation should be set as **\[master]**.

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
