**Date**: 2025-07-26 **Time**: 09:58
**Status**: #baby #ROS2
**Tags**: [[command]] [[ROS2]]
# Section 6 - ROS 2 services
## 49. Intro
The two most important communication features in ROS 2 are Topics and Services.
Topics are used for data streams, and Services for a client/server interaction.
At the end of this section you’ll be able to make your nodes communicate with ROS2 Services.
Here’s what you’ll do:

- Understand what ROS Services are, and when to use them, thanks to a real life analogy.
    
- Write your own Service (client + server) in both Python and C++.
    
- Introspect Services from the terminal.
    
- Experiment on Turtlesim.
---
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
---
## 58. Rename a Ros2 Service
For renaming the service:
`ros2 run my_py_pkg add_two_ints_server --ros-args -r <old_service_name>:=<new_service_name>`

>[!warning] Renaming service
>If you rename a Service in a Server, you also need to rename it on the client side also.

---
## 62. Conclusion
In this section you have discovered ROS2 Services, and seen how you can use them to add client/server communications between your nodes.

To recap, Services are:

- Used for client/server types of communication.
    
- Synchronous or asynchronous (though it’s recommended to use them asynchronously, even if you decide to wait after in the thread).
    
- Anonymous: a client does not know which node is behind the service, it just calls the service. And the server does not know which nodes are clients, it just receives requests and responds to them.
    

To implement Services inside your nodes:

- Create a node or start from an existing one. Add as many Service servers as you want (all with different names)
    
- When you call a Service server from a Service client, make sure that the Service name, as well as the Service type (request + response) are identical.
    
- You can only create one server for a Service, but you can create many clients.
    

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2022-04-18_07-52-48-f481c239b90fef62e3bf8c9a41a8c883.png)

When you want to add a new communication between your nodes, ask yourself this question: “Am I just sending some data, or do I expect a response after I send the message?”. This will tell you if you need to use a Topic or a Service. And, as you progress with ROS2, it will eventually become quite obvious for you.

---   

So, now you can create nodes and make them communicate between each other. But, you’ve only used existing messages so far. What if you need to use other message types for your Topics and Services?

Well, in this case, you’ll need to build your own message types, and that’s what we’ll see in the next section.





## References
https://www.udemy.com/course/ros2-for-beginners/learn/lecture/21305856#overview