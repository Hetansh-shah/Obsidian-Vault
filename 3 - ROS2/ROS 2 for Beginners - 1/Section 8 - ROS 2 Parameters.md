**Date**: 2025-07-31 **Time**: 10:51
**Status**: #ROS2 
**Tags**: [[ROS2]]
# Section 8 - ROS 2 Parameters

## 77. Using ROS 2 Parameter
It can be ==created== by:
`self.declare_parameter("<variable_name>", <default_value>)`
it can be ==called== by:
`self.number_ = self.get_parameter("<variable_name>").value`

can be used in terminal by:
`ros2 run <package> <executable> --ros-args -p <param1>:=<value> -p <param2>:=<value>`

---
## 79. Turtlesim params
`ros2 param get </node name> <param>`

---
## 80. YAML Parameter Files
Syntax of it should be:
```yaml
/number_publisher:  # node name
  ros__parameters:  # 2 space indent and "ros__parameters:" is compulsory
    number: 3       # params
    time_period: 0.7
```
To call it:
`ros2 run my_py_pkg number_publisher --ros-args --params-file ~/file_location.yaml`

---
## 84. Parameter callback
This thing is used when you change the parameter during the runtime of a node. We just initialize the param value at the init() phase.

> We can change the params during runtime by `ros2 param set </node name> <param name> <value>`

**Therefore, we need to add a param callback in order to update the new parameters.**
It is done by the command `add_post_set_parameters_callback()`.

You need to include:
```python
from rclpy.parameter import Parameter


	# after declaring parameters...
	self.add_post_set_parameters_callback(self.parameters_callback)



	def parameters_callback(self, params: list[Parameter]):
		for param in params:
			if param == 'number': # number is a parameter name
				self.number_ = param.value
```
---
## 85. Conclusion
In this section you have discovered ROS 2 parameters.

With parameters you don’t need to modify + re-compile your code for each different set of configuration. Just write your code once, and choose your settings at run-time.

Using parameters is one of the first steps to make your application more scalable.

![](https://img-c.udemycdn.com/redactor/raw/article_lecture/2022-04-18_07-58-29-8c66c1af0f8916838b6d228c816f2eaa.png)
 

To handle parameters:

- Don’t forget to declare any parameter before you even try to use it.
    
- The default value’s data type will be the data type for the parameter.
    
- When you start your node, set values for your parameters (if you don’t, default values will be used).
    
- In your node’s code, get the parameters’ values and use them.
    

You can also store many parameters for one or several nodes, inside a YAML param file. Then, all you need to do is load the YAML file when you start the nodes.



---
## References
https://www.udemy.com/course/ros2-for-beginners/learn/lecture/21306350#overview