**Date**: 2025-08-12 **Time**: 21:11
**Status**:
**Tags**: 
# Section 5 - Improve the URDF with Xacro

## 28. Intro

XACRO (XML Macros) is a preprocessor for URDF files in ROS2 that adds programming features like variables, math operations, and reusable macros to make robot descriptions easier to write and maintain. Instead of copying and pasting similar robot parts in regular URDF files, you can define variables (`<xacro:property name="wheel_radius" value="0.1"/>`) and create reusable components (`<xacro:macro name="wheel">`) that can be used multiple times with different parameters. XACRO files have a `.urdf.xacro` extension and must be processed with the `xacro` command to generate standard URDF files that ROS2 can use.

## 29. Make URDF compatible with Xacro

```xml
<?xml version="1.0"?>
<robot name="agv" xmlns:xacro="http://www.ros.org/wiki/xacro">
    <material name="green">
        <color rgba="0 0.5 0 1" />
```
Add the url of xacro: http://www.ros.org/wiki/xacro

*Make sure the file name is ==file_name.urdf.xacro== and update the name accordingly in the launch files.*

### To create a property:
```xml
<robot name="agv" xmlns:xacro="http://www.ros.org/wiki/xacro">

    <xacro:property name="base_length" value="0.54" />
    <xacro:property name="base_width" value="0.355" />
```
And to be used by `size="${base_length} ${base_width} 0.1"`

## 33. Create functions with Xacro Macros

```xml
    <xacro:macro name="wheel_link" params="prefix">
        <link name="${prefix}_wheel_link">
            <visual>
                <geometry>
                    <cylinder radius="${wheel_radius}" length="${wheel_length}" />
                </geometry>
                <origin xyz="0 0 0" rpy="${pi / 2.0} 0 0" /> 
                <material name="gray" />
            </visual>
        </link>
    </xacro:macro>

    <xacro:wheel_link prefix="right"/>
    <xacro:wheel_link prefix="left"/>

    <xacro:macro name="castor_link" params="prefix">
        <link name="${prefix}_castor_wheel_link">
            <visual>
                <origin xyz="0 0 0" rpy="0 0 0" />
                <geometry>
                    <sphere radius="${wheel_radius / 2.0}" />
                </geometry>
                <material name="gray" />
            </visual>
        </link>
    </xacro:macro>

    <xacro:castor_link prefix="front"/>
    <xacro:castor_link prefix="rear"/>
```

Use this format to create macros and use them to reduce the code size.



## References
