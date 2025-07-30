**Date**: 2025-07-29 **Time**: 10:03
**Status**: #ROS2
**Tags**: [[ROS2]]
# Section 7 - ROS 2 Interfaces
## 63. Intro
ROS 2 interfaces are the messages and services you use with your topics and services.

For now you’ve only used existing interfaces so you could focus on learning how topics and services work.

Now, it’s time to learn how to use your own message/service types!

At the end of this section you’ll be able to:

- Create and build your own interfaces with msg and srv definitions
    
- Use those interfaces with your topics and services
---
## 65. Create & Build Custom Interfaces Msg

1. Create a Different package which contains all the interfaces. E.g.: ==my_robot_interfaces== of ==ament_cmake== type. By default it will e cmake only.
2. Remove the /src and /include and create a /msg directory.
3. Customize the package.xml and CmakeLists.txt in order to create custom interfaces.
	package.xml:
	```xml
	<?xml version="1.0"?>
	<?xml-model href="http://download.ros.org/schema/package_format3.xsd"  chematypens="http://www.w3.org/2001/XMLSchema"?>
	<package format="3">
	  <name>my_robot_interfaces</name>
	  <version>0.0.0</version>
	  <description>TODO: Package description</description>
	  <maintainer email="user@todo.todo">user</maintainer>
	  <license>TODO: License declaration</license>
	
	  <buildtool_depend>ament_cmake</buildtool_depend>
	
	  <buildtool_depend>rosidl_default_generators</buildtool_depend>
	  <exec_depend>rosidl_default_runtime</exec_depend>
	  <member_of_group>rosidl_interface_packages</member_of_group>
	
	  <test_depend>ament_lint_auto</test_depend>
	  <test_depend>ament_lint_common</test_depend>
	
	  <export>
	    <build_type>ament_cmake</build_type>
	  </export>
	</package>
	```
	
	Only the following 3 lines are needed to be added:
	```xml
	  <buildtool_depend>rosidl_default_generators</buildtool_depend>
	  <exec_depend>rosidl_default_runtime</exec_depend>
	  <member_of_group>rosidl_interface_packages</member_of_group>
	```
	
	CMakeLists:
	```c
	cmake_minimum_required(VERSION 3.5)
	project(my_robot_interfaces)
	
	# Default to C++14
	if(NOT CMAKE_CXX_STANDARD)
	  set(CMAKE_CXX_STANDARD 14)
	endif()
	
	if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	  add_compile_options(-Wall -Wextra -Wpedantic)
	endif()
	
	find_package(ament_cmake REQUIRED)
	find_package(rosidl_default_generators REQUIRED)
	
	rosidl_generate_interfaces(${PROJECT_NAME}
	  "your custom interfaces will be here"
	  "one per line"
	  "no comma for separating lines"
	 )
	
	ament_export_dependencies(rosidl_default_runtime)
	
	ament_package()
	```
	
	Only the following lines are needed to be added:
	```c
	find_package(rosidl_default_generators REQUIRED)
	
	rosidl_generate_interfaces(${PROJECT_NAME}
	  "your custom interfaces will be here"
	  "one per line"
	  "no comma for separating lines"
	 )
	
	ament_export_dependencies(rosidl_default_runtime)
	```
4. Now create the .msg file in the /msg directory. 
   `$ cd ~/ros2_ws/src/my_robot_interfaces/msg/
   `$ touch HardwareStatus.msg`
   >[!info] Note
>- Use CamelCase for the name of the interface. Ex: “MotorTemperature”.
>- Don’t add “Msg” or “Interface” in the name, this will add redundancy.
>- Use the .msg extension.

Add the path in the CMakeLists, "msg/HardwareStatus.msg"
5. Build it. 
6. check it by:
   `ros2 interface show my_robot_interfaces/msg/HardwareStatus`



## 68. Create & Build Custom Interfaces Srv
1. Create a Different package which contains all the interfaces. E.g.: ==my_robot_interfaces== of ==ament_cmake== type. By default it will e cmake only.
2. Remove the /src and /include and create a /msg directory.
3. Customize the package.xml and CmakeLists.txt in order to create custom interfaces.
	package.xml:
	```xml
	<?xml version="1.0"?>
	<?xml-model href="http://download.ros.org/schema/package_format3.xsd"  chematypens="http://www.w3.org/2001/XMLSchema"?>
	<package format="3">
	  <name>my_robot_interfaces</name>
	  <version>0.0.0</version>
	  <description>TODO: Package description</description>
	  <maintainer email="user@todo.todo">user</maintainer>
	  <license>TODO: License declaration</license>
	
	  <buildtool_depend>ament_cmake</buildtool_depend>
	
	  <buildtool_depend>rosidl_default_generators</buildtool_depend>
	  <exec_depend>rosidl_default_runtime</exec_depend>
	  <member_of_group>rosidl_interface_packages</member_of_group>
	
	  <test_depend>ament_lint_auto</test_depend>
	  <test_depend>ament_lint_common</test_depend>
	
	  <export>
	    <build_type>ament_cmake</build_type>
	  </export>
	</package>
	```
	
	Only the following 3 lines are needed to be added:
	```xml
	  <buildtool_depend>rosidl_default_generators</buildtool_depend>
	  <exec_depend>rosidl_default_runtime</exec_depend>
	  <member_of_group>rosidl_interface_packages</member_of_group>
	```
	
	CMakeLists:
	```c
	cmake_minimum_required(VERSION 3.5)
	project(my_robot_interfaces)
	
	# Default to C++14
	if(NOT CMAKE_CXX_STANDARD)
	  set(CMAKE_CXX_STANDARD 14)
	endif()
	
	if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
	  add_compile_options(-Wall -Wextra -Wpedantic)
	endif()
	
	find_package(ament_cmake REQUIRED)
	find_package(rosidl_default_generators REQUIRED)
	
	rosidl_generate_interfaces(${PROJECT_NAME}
	  "your custom interfaces will be here"
	  "one per line"
	  "no comma for separating lines"
	  "msg/HardwareStatus.msg"
	  "srv/ComputeRectangleArea.srv"
	 )
	
	ament_export_dependencies(rosidl_default_runtime)
	
	ament_package()
	```
	
	Only the following lines are needed to be added:
	```c
	find_package(rosidl_default_generators REQUIRED)
	
	rosidl_generate_interfaces(${PROJECT_NAME}
	  "your custom interfaces will be here"
	  "one per line"
	  "no comma for separating lines"
	  "msg/HardwareStatus.msg"
	  "srv/ComputeRectangleArea.srv"
	 )
	
	ament_export_dependencies(rosidl_default_runtime)
	```
4. Now create the .srv file in the /srv directory. 
   `$ cd ~/ros2_ws/src/my_robot_interfaces/srv/
   `$ touch ComputeRectangleArea.srv`
   >[!info] Note
>- Use CamelCase for the name of the interface. Ex: “MotorTemperature”.
>- Don’t add “Srv” or “Interface” in the name, this will add redundancy.
>- Use the .msg extension.

>[!important] Content of .srv file:
>1. An empty service only has `---` in it.
>2. A working srv will have inputs, then outputs seperated by `---`.
>

Add the path in the CMakeLists, "srv/ComputeRectangleArea.srv"
5. Build it. 
6. check it by:
   `ros2 interface show my_robot_interfaces/srv/ComputeRectangleArea.srv`

---
## 69. Introspect interfaces with ROS 2 command line


```bash
[[command]]
ros2 interface list # lists all the interfaces sourced
ros2 interface package <package_name> # lists all the interfaces of that package
ros2 interface show <interface path> # shows the information about the particular interface


```
---
## 74. Conclusion
In this section you have seen how to create your own ROS 2 interfaces, for Topics and Services.

Topics and Services are the communication layer. Interfaces are the actual content that you send.

To recap, here’s how to create a custom interface:

- Create a new package only for your msg and srv definitions.
    
- Setup the package (CMakeLists.txt and package.xml)
    
- Create a msg/ and srv/ folders, place your custom msg definitions and srv definitions here.
    

Once you’ve setup your package, adding a new interface is really simple:

- Add a new file in the right folder: msg/ or srv/
    
- Add one line into CMakeLists.txt
    
- Compile with “colcon build”
    
- And don’t forget to source your ROS 2 workspace when you want to use those messages!
    

Here’s what you can use inside a msg or srv definition:

- Any primitive type defined by ROS 2 (most common ones: int64, float64, bool, string, and array of those)
    
- Any message you’ve already created in this package.
    
- Any message from another package. In this case don’t forget to add a dependency for the other package in both package.xml and CMakeLists.txt.
    

And now, when you compile the definitions, new interfaces will be created, along with headers/modules ready to be included in your C++ or Python nodes.

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2022-04-18_07-57-05-c0ccb12404b31599c1fd96bc5dbfd8ab.png)

  

You’re almost ready to start your own ROS 2 application! In the next section you’ll see how to add settings to your nodes at run time with ROS 2 parameters.







## References
https://www.udemy.com/course/ros2-for-beginners/learn/lecture/21306040#overview
https://roboticsbackend.com/ros2-create-custom-message/