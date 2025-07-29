**Date**: 2025-07-26 **Time**: 09:58
**Status**: #baby 
**Tags**: 
# Section 6 - ROS2 services
## 49. Intro
The two most important communication features in ROS 2 are Topics and Services.
Topics are used for data streams, and Services for a client/server interaction.
At the end of this section you’ll be able to make your nodes communicate with ROS2 Services.
Here’s what you’ll do:

- Understand what ROS Services are, and when to use them, thanks to a real life analogy.
    
- Write your own Service (client + server) in both Python and C++.
    
- Introspect Services from the terminal.
    
- Experiment on Turtlesim.

## 57. Service ROS2 command line
```bash
ros2 node list
ros2 node info <node_name> # can see the active services in that particular node

ros2 service list
ros2 service type <service_name> # to know more about services

ros2 interface show <Interface name>

ros2 service call <service name> <interface name> "{a: 4, b: 5}" # just an example
# the above thing can be done using a GUI
rqt -> Plugins -> Services -> Service caller.


```

## 58. Rename a Ros2 Service
For renaming the service:
`ros2 run my_py_pkg add_two_ints_server --ros-args -r <old_service_name>:=<new_service_name>`

>[!warning] Renaming service
>If you rename a Service in a Server, you also need to rename it on the client side also.







## References
