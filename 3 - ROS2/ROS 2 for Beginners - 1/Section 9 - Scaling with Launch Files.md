**Date**: 2025-08-05 **Time**: 11:40
**Status**: #ROS2 
**Tags**: [[ROS2]]
# Section 9 - Scaling with Launch Files

## 86. Intro
So, you now have many nodes in your packages. When you start them, you can rename them, rename the topics, services, and set parameters.

That’s a lot of things! Now, imagine you have to start 10 nodes, each with a different configuration. Using the terminal is not something that scales well.

In this section we’ll see how to solve that problem with launch files.

At the end of this section you will be able to start all your nodes and parameters from one single ROS 2 Launch File.

What you’ll do in this section:

- Understand what launch files are and when to use them.
    
- How to create, install, and start a launch file.
    
- Install YAML files and load them inside your launch files.
    
- Discover how to start your nodes inside namespaces.
    
- And practice more on your one with another activity.
---
## 88. Create and install a Launch file (XML) 

> ==XML== stands for Extensible Markup Language

You should create the launch file by:
going to the src directory and create a package named ==E.g. abc_bringup==
- remove include and src.
- create launch directory.
- Inside CMakeLists.txt, add,
```c
install(DIRECTORY
	launch
	DESTINATION share/${PROJECT_NAME}/
)
```

The launch file i.e. `number_app.launch.xml` must contain:
```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher"/>
	<node pkg="my_py_pkg" exec="number_counter"/>
</launch>
```
---
## 90. Remapping in a launch file
Remapping or renaming of nodes can be done in the launch file by:
```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher"/>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
```

To remap a topic from a Launch file:
```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher">
		<remap from="/number" to="/my_number"/>
	</node>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
``` 
---
## 91. Loading Parameters in a Launch file
```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher">
		<remap from="/number" to="/my_number"/>
		<param name="number" value="3"/>
		<param name="time_period" value="1.5"/>
	</node>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
```

You can create a config folder in the bringup directory in order to store the params yaml files.
and Add the config folder in the install section of CMakeLists.txt. 
```c
install(DIRECTORY
	launch config
	DESTINATION share/${PROJECT_NAME}/
)
```

To load from a yaml file:
```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher">
		<remap from="/number" to="/my_number"/>
		%% <param name="number" value="3"/> commented
		<param name="time_period" value="1.5"/> %% commented
		<param from="$(find-pkg-share my_robot_bringup)/config/number_app.launch.xml"/>
	</node>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
```
---
## 92. Adding namespace
Namespaces in ROS2 are hierarchical naming structures that organize and separate nodes, topics, services, and parameters. They work like folders in a file system, creating distinct scopes for ROS2 entities.
### How Namespaces Work
Namespaces use forward slashes (/) to create hierarchical paths:
- `/robot1/camera/image`
- `/robot2/camera/image`
- `/global/status`
### Why Namespaces Are Used
**Multi-robot systems**: Run identical nodes for different robots without naming conflicts **Component organization**: Group related functionality together **Parameter isolation**: Keep configuration separate between different parts of the system **Remapping flexibility**: Easily redirect connections between nodes

`ros2 run my_package my_node --ros-args --remap __ns:=/robot1` or
`ros2 run my_package my_node --ros-args -r __ns:=/robot1`

```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher"> namespace="/abc"
		<remap from="/number" to="/my_number"/>
	</node>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
```
Namespace is added as /abc. Now the node will run as ==/abc/number_publisher==.

```xml
<launch>
	<node pkg="my_py_pkg" exec="number_publisher" name="my_number_publisher"> namespace="/abc"
		<remap from="number" to="my_number"/>
	</node>
	<node pkg="my_py_pkg" exec="number_counter" name="my_number_counter"/>
</launch>
```
When the node runs, it will publish onto ==/number== only and not on ==/abc/number==. It is because when we add a slash (/) in front of topic it will search the exact '/number' but if we write only ==number== it will see whichever 'number' is present and remap it.

---
## 95. Conclusion
In this section you have discovered Launch Files.

With a launch file, you can start your entire application with only one command line, in one terminal. You can add any number of nodes and fully configure them. That will make your application fully customizable in no time.

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2022-04-18_08-15-46-9b60901c70f2fbfea8377c828d45350d.png)

Recap:
Setup for launch files:
- Create a new package <robot_name>_bringup (best practice).
    
- Create a launch/ folder at the root of the package.
    
- Configure CMakeLists.txt to install files from this launch/ folder.
    
- Create any number of files you want inside the launch/ folder, ending with .launch.py.
    
Run a launch file:
- After you’ve written your file, use “colcon build” to install the file.
    
- Don’t forget to source your environment
    
- Start the launch file with “ros2 launch <package> <name_of_the_file>
    


Download the complete code for this section (this is the code from all previous sections + the current one).
Now, you have everything you need to create a complete application, and scale it without any problem.