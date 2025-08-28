**Date**: 2025-08-11 **Time**: 16:14
**Status**: 
**Tags**:
# Section 3 - Create a URDF for a Robot
## 11. Intro - What is a URDF?
URDF stands for **Unified Robot Description Format**.
- Description of all the elements in a robot
- Used to generate TFs
- XML format

## 12. First URDF file
```xml
<?xml version="1.0"?>
<robot name="my_robot">
	<material name="green">
		<color rgba="0 0.5 0 1" />
	</material>
	<link name="base_link">
		<visual>
			<geometry>
				<box size="5.4 3.6 0.1" />
			</geometry>
			<origin xyz="0 0 0.05" rpy="0 0 0" />
			<material name="green" />
		</visual>
	</link>
</robot>
```

## 14.  Combine 2 links with a Joint
```xml
<?xml version="1.0"?>
<robot name="my_robot">
	<material name="green">
		<color rgba="0 0.5 0 1" />
	</material>

	<material name="gray">
		<color rgba="0.5 0.5 0.5 1" />
	</material>
	
	<link name="base_link">
		<visual>
			<geometry>
				<box size="5.4 3.6 0.1" />
			</geometry>
			<origin xyz="0 0 0" rpy="0 0 0" />
			<material name="green" />
		</visual>
	</link>

	<link name="second_link">
		<visual>
			<geometry>
				<cylinder radius="0.1" length="0.2" />
			</geometry>
			<origin xyz="0 0 0" rpy="0 0 0" />
			<material name="gray" />
		</visual>
	</link>
</robot>
```
>[!warning] Don't do this
>1. This will cause an error as there are 2 main links.
>2. It is mandatory to have single main/parent link and others are derivatives of those.
>3. You need to create joints in order to do this.

```xml
<?xml version="1.0"?>
<robot name="my_robot">
	<material name="green">
		<color rgba="0 0.5 0 1" />
	</material>

	<material name="gray">
		<color rgba="0.5 0.5 0.5 1" />
	</material>
	
	<link name="base_link">
		<visual>
			<geometry>
				<box size="5.4 3.6 0.1" />
			</geometry>
			<origin xyz="0 0 0" rpy="0 0 0" />
			<material name="green" />
		</visual>
	</link>

	<link name="second_link">
		<visual>
			<geometry>
				<cylinder radius="0.1" length="0.2" />
			</geometry>
			<origin xyz="0 0 0" rpy="0 0 0" />
			<material name="gray" />
		</visual>
	</link>

	<joint name="base_second_joint" type="fixed">
		<parent link="base_link" />
		<child link="second_link" />
		<origin xyz="0 0 0" rpy="0 0 0" />
	</joint>
</robot>
```

## 16. Different types of joints
### Revolute
```xml
<?xml version="1.0"?>
<robot name="agv">
    <material name="green">
        <color rgba="0 0.5 0 1" />
    </material>

    <material name="gray">
        <color rgba="0.5 0.5 0.5 1" />
    </material>

    <link name="base_link">
        <visual>
            <geometry>
                <box size="0.54 0.36 0.1" />
            </geometry>
            <origin xyz="0 0 0.05" rpy="0 0 0" /> 
            <material name="green" />
        </visual>
    </link>


    <link name="second_link">
        <visual>
            <geometry>
                <cylinder radius="0.1" length="0.2" />
            </geometry>
            <origin xyz="0 0 0.1" rpy="0 0 0" /> 
            <material name="gray" />
        </visual>
    </link>

    <joint name="base_second_joint" type="revolute">
        <parent link="base_link" />
        <child link="second_link" />
        <origin xyz="0 0 0.1" rpy="0 0 0" />
        <axis xyz="0 0 1" />
        <limit lower="-1.57" upper="1.57" effort="100" velocity="100" />
    </joint>

</robot>
```

### Continuous
- can be used for wheels, (don't have limits)
```xml
	<joint name="base_second_joint" type="continuous">
        <parent link="base_link" />
        <child link="second_link" />
        <origin xyz="0 0 0.1" rpy="0 0 0" />
        <axis xyz="0 0 1" />
    </joint>
```

### Prismatic
- Can be used for translational/sliding movements
```xml
    <joint name="base_second_joint" type="prismatic">
        <parent link="base_link" />
        <child link="second_link" />
        <origin xyz="0 0 0.1" rpy="0 0 0" />
        <axis xyz="1 0 0" />
        <limit lower="-0.27" upper="0.27" effort="100" velocity="100" />
    </joint>
```






## References
https://www.udemy.com/course/ros2-tf-urdf-rviz-gazebo/learn/lecture/38689068#overview