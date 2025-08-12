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
## References
