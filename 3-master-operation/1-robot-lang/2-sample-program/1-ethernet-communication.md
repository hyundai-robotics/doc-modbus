# 3.1.2.1 Ethernet Communication

 The following shows a sample program that controls the onRobot gripper. The Hi6 controller and the onRobot gripper communicate via the Modbus tcp. Here, the Hi6 controller is operated as the master, while the gripper is operated as a slave.

```python
Hyundai Robot Job File; { version: 1.6, mech_type: "368(HA006A-01)", total_axis: 6, aux_axis: 0 }
    
     # Import the modbus module, then create a modbus master object using a constructor (set the communication type, port number, and IP address.) 
     import modbus
     var master=modbus.Modbus("tcp",502,"192.168.1.1")

     # Data to transmit
     global arr
     arr=Array(300)
     arr[0]=300 #force(0–400)
     arr[1]=200 #width (0–1100)
     arr[2]=1 #control(1:grip, 8: stop, 16: offset grip)
     
     # Execute the write operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     master.write(65,0,3,arr,3000,99)

     # Check the communication status (0: no connection (default state), 1: normal state, -1: communication failure, -2: timeout error.)
     if master.status<0
       print "write communication error"
       stop
     endif

     # Execute the read operation. (Option) Branch to the 99th line when exceeding 3000 msec.
     var recv_data=master.read(65,267,2,3000,99)
     if master.status<0
       print "read communication error"
       stop
     endif

     # Handle the received data.
     arr[267]=recv_data[0] #actual width
     arr[268]=recv_data[1] #status (0: busy, 1:grip detected ,,,etc)
     if arr[268]<1 and arr[268]>7
       print "check the gripper status"
       stop
     endif
     end

  99 print "timeout"



```

