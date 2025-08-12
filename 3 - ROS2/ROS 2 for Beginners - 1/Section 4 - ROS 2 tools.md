**Date**: 2025-07-23 **Time**: 11:36
**Status**: #child #ROS2
**Tags**: [[ROS2]]
# Section 4 - ROS 2 tools

## 26. Introspect your node with ros2cli

- all about sourcing your workspace using .bashrc
- `ros2 run -h`: for help
- `ros2 node list`: listing the active node
---
## 27. Rename a node at Runtime

2 nodes can run at same time with same name, but can led to unintended side effects.
so, we can rename the node at runtime
`ros2 run my_cpp_pkg cpp_node --ros-args -r __node:=abc`
- -r stands for remap | we can use --remap instead of -r
- abc is the new name of the node.
---
## 28. Colcon
Build in ros_ws/ only
by `colcon build` : to build all the packages
`colcon build --packages-select <package_name>` : for building a particular package
`colcon build --packages-select <package_name> --symlink-install` : This runs the .py file directly from the src/ folder instead of the one in install/ folder.

>[!warning] --symlink-install
>1.Valid only on Python package and not on C++

---
## 29. Rqt and Rqt_graph

**Commands:**
- `rqt`
- `rqt_graph`
To monitor what's going on in ROS

---
## 30. Discover Turtlesim

`ros2 run turtlesim turtlesim_node`
`ros2 run turtlesim turtle_teleop_key`
`ros2 run turtlesim turtlesim_node --ros-args -r __node:=my_turtle`

---
## 31. Activity 
![[Pasted image 20250723152341.png]]
Clone the same rqt graph using the existing nodes and turtlesim packages.
==**Hint**: Use remap for the task and use one Python and C++ node.==

---
## Section Conclusion
In this section you’ve discovered 2 important tools you’ll use for any ROS 2 application you develop: the ros2 cli (Command Line Interface), and rqt with all its plugins. During the course you’ll continue to practice with those tools, and you’ll discover more of them.

You also discovered the Turtlesim package, containing a simplified 2D robot, which will be very useful to apply what you learn throughout the course.

So, with the last section on nodes + the activities from this section, you already have a good foundation and you can:

- Create a custom node in your own package (Python and Cpp)
    
- Compile and run your nodes
    
- Launch and debug your nodes with different tools
    
- Run existing nodes from other packages
    

Now, the next logical step is to see how those nodes can communicate between each other. And… That’s just what’s coming in the next section on ROS 2 Topics.







---
## References
https://www.udemy.com/course/ros2-for-beginners/learn/lecture/21305562#overview