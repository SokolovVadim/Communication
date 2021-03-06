# Configure + Build
+ qibuild configure -c "toolchain name"
+ qibuild make -c "toolchain name"
# Set up Game Controller & Tester
+ move to ~/GameController2018/
+ java -jar GameController.jar
+ java -jar GameControllerTester.jar
# Structure:
Game controller files are conducted with gameControllData headers. 
Udp class uses the same data type to transfer information
# Port Numbers
+ File with port numbers:  
/GameController2018/include/RoboCupGameControlData.h
+ GC data transfering:  
Using broadcast address /192.168.1.255  
Listening on address /192.168.1.68  
# Achievements
+ Transfering messages from GC to GCTester using udp protocol and Linux Socket
+ adding some API to deliver messages with udp (25.02)
# Current ideas about GC
+ Run Game Controller
+ Analyse GameControlData class
+ Analyse GameController messages
# Current ideas about Transfering data
+ Easy way  
Read [InTeReStInG](https://github.com/UNSWComputing/rUNSWift-2018-release/tree/master/robot/gamecontroller) code.  
Find START message handler.  
Write module the same way.  
+ Cool way  
Use AL:: library and Brocker API to handler the START message.  
We can write separate class in which we'll use brockers to call methods from other modules
+ Current way  
We have to write special class, which will contain data and team info
# Test abilities
WiFi local network
+ Only computers  
On first PC GameController is executed. This PS is a message sender. Messages contains information about commands, game style, timeout etc. The second computer reads GCData from UDP port and parses data in readable way.
We can provide a reliable connection and data transfering. After that we can start the next step  
+ Comming soon ...

# address of PC with GC  
My PC 

  wlp3s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
  inet **192.168.1.68**  
# The solution of transmission problem
We used two programms written by [Vlad](https://github.com/MolVlad) in terms of OS course: server.c & client.c
On the first PC we made a broadcast point with GC. On the second one there was a running client program. This program reads data from all the users in a network, and we saw a broadcasting messages in Robocup GameController standart data type.  

# Toolchain problem
Coding a Communication module we faced a problem with boost warnings layout and CMakeList specific syntax.  
We added a special header file which had to provide us an ability to ignore warnings:  

    #include <boost/config/warning_disable.hpp>

We added prefixes to Cmake flags this way:

    SET(QIBUILD_COMPILE_FLAGS "-isystem")
    add_definitions(${QIBUILD_COMPILE_FLAGS})
or

    target_compile_options(all PRIVATE -isystem)
  
Both of these features didn't work properly.  
Then we rewrote a Communication module with new toolchain and now It works!  
We creates a connection dump to see the data transmission between robot & PC with GC. 

![UDP TRANSMISSION DUMP](https://github.com/SokolovVadim/Communication/blob/master/doc/Dump_screen.jpg)

# Already done task  
+ write a wrapper class which provides reading, writing and parsing data  
+ execute the program from nao robot

# Achievements
We have already write a wrapper class which is now a member of Communication class  
We tested our module with remote connection on NAO robot  
After that we turned to uploading the binary files .so onto NAO robot
There was a problem with a loop in constructor, but we found a solution  
The GC and NAO created a communication and we could receive messages from NAO  
to PC and set states of NAO by GC interface  

![GC TEST](https://github.com/SokolovVadim/Communication/blob/master/doc/TestGC.jpg)

TODO:  
Now we have to add a virtual function init which is inherited from the based class AlModule  


