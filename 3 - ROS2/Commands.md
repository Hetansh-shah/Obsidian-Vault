Building:
```
colcon build
colcon build --symlink-install
colcon build --packages-select <package_name>
```
Node: 
```sh
ros2 run <node_name> <executable_name> # to run the node
ros2 node list # to list all the active nodes
ros2 node info <node_name> # to access more information abt that node
```
Topics: 
```sh
ros2 topic list # to list all the active topics
ros2 topic list -t # with topic type appended in brackets
ros2 topic list # to list all the topics
ros2 topic echo <topic_name> # to access the contents of a topic
ros2 topic info <topic_name> # to know abt pubs and subs
ros2 interface show geometry_msgs/msg/Twist # to know what type of message is sent
ros2 topic pub <topic_name> <msg_type> '<args>' # to directly publish data via terminal
E.g. ros2 topic pub /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```
Params:
```sh
ros2 param list # to see the parameters of your nodes
ros2 param get <node_name> <parameter_name> # to display the type and current value of a parameter
E.g. ros2 param get /turtlesim background_g
	Integer value is: 86
ros2 param set <node_name> <parameter_name> <value> # to change a parameter’s value at runtime
E.g. ros2 param set /turtlesim background_r 150
	Set parameter successful
ros2 param dump <node_name> # to view all the current node parameters' value
E.g. ros2 param dump /turtlesim > turtlesim.yaml # this saves the values to turtlesim.yaml
ros2 param load <node_name> <param_file> # to load parameters from file
ros2 run <package_name> <executable_name> --ros-args --params-file <file_name> # to load at starting of the node



```

