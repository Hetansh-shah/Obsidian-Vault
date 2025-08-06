**Date**: 2025-08-05 **Time**: 11:40
**Status**:
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








---
## References
