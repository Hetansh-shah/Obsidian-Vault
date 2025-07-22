**Date**: 2025-07-17 **Time**: 13:59
**Status**: #child
**Tags**: [[command]] [[ROS2]] 
# Section 3 - First ROS2 Program
---
## Create a ROS 2 Workspace
First, create the workspace and include a "src" folder in it. It contains all the source code files.
```sh
mkdir ros2_ws
cd ros2_ws
mkdir src
cd ~/ros2_ws
```
After creating the workspace, you need to build it from the "ros2_ws" directory. Use this command `colcon build` for building the workspace. This creates other 3 folders alongwith "src", "build", "log", "include".

You need to source the setup.bash file everytime you run ROS. So, add it into `.bashrc` to run it everytime when the terminal is launched.
```
gedit ~/.bashrc
// when file opens, add below line at the bottom
source ~ros2_ws/install/setup.bash
```

---
## Create a Python package
To create a python package, you need to go into src folder and type:
```sh
ros2 pkg create my_py_pkg --build-type ament_python --dependencies rclpy
```
To build the package run `colcon build`.

>[!tip] Note
>To build a particular package
>`colcon build --packages-select package_name`

---
## Create a C++ package
To create a C++ package, you need to go into src folder and type:
```sh
ros2 pkg create my_cpp_pkg --build-type ament_cmake --dependencies rclcpp
```
To build the package run `colcon build`.

---
## What is ROS 2 node?
- A Node is a fundamental building blocks of a ROS2 package. Each node is designed to perform a particular task such as controlling actuators, camera, processing sensor data
- Nodes can communicate with each other through topics, services and parameters
- Subprograms in  your application, responsible for only one thing
- Combined into a graph
- Benefits:
	- Reduce code complexity
	- Fault tolerance
	- Language Agnostic (Can be written in C++ and Python)

---
## Write a minimal Python node
Go into the Python package dir.
```sh
cd ros2_ws/src/my_py_pkg/my_py_pkg/
touch my_first_node.py # to create a node file
```

```python
def main(args=None):
	rclpy.init(args-=args)
	node = Node('py_test')
	node.get_logger().info("Hello Hetansh") # to print on the terminal
	node.spin(node) # to keep the node running
	node.shutdown()
```

To run the node we can simply run the python file ==OR== we can add an executable in the ==setup.py== file in order to run it as a node from anywhere. For executable, you need to add the following in console scripts:
```python
entry_points = {
	console_scripts' : [
		"py_node = my_py_pkg.my_first_node:main",
	],
}
```

When we run the node by, `ros2 run my_py_pkg py_node`:
- `py_node` is the executable name 
- `[INFO] [1752857727.555058290] [py_test]: Hello Hetansh` : py_test is the node name.
---
## Write a Python code with OOP

>[!time] Timer Functionality
>This is used to select the interval for getting the values for a task. Lets say, monitor the temp sensor at 10Hz

### Timer Functionality
To create timer functionality, you need to create a `timer_callback(self)` class method. This method consists all the things that are to be monitored.

To use this functionality, you need to call it in `__init__(self)`  function by the command:
```python
self.create_timer(1.0, timer_callback)
# 1.0 is the frequency in seconds and the ither arguement is the callback function that is to be repeated.  
```

==You can also use a counter to keep a watch.== 
==in `__init__` you can create a variable `self.counter_ = 0`==
==then add this variable in the logger by typecasting it to str()==
==and increment the value by 1 in the timer_callback func.==

---
## Write a minimal C++ node











## References
https://www.udemy.com/course/ros2-for-beginners/learn/lecture/21305270#content