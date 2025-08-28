**Date**: 2025-08-18 **Time**: 10:44
**Status**:
**Tags**: [[ROS 2 - TFs]]
# Section 6 - Simulate your robot
## 37. Intro
**Rviz and Gazebo** are both different things. Rviz is used only for visualising your 3D files and gazebo is used for Simulation.
![[Pasted image 20250818104429.png]]

## 38. Install and run Gazebo
`sudo apt install ros-humble-ros-gz`
if you want to run it using ros,
`ros2 launch ros_gz_sim gz_sim.launch.py gz_args:=empty.sdf`

## 39. How Gazebo works with ROS

![[Pasted image 20250818124101.png]]

## 40. Add inertial tags to the urdf

![[Pasted image 20250818125619.png]]

```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">
    
    <material name="green">
        <color rgba="0 0.5 0 1" />
    </material>

    <material name="gray">
        <color rgba="0.5 0.5 0.5 1" />
    </material>
    
    <xacro:macro name="box_inertia" params="m x y z o_xyz o_rpy">
        <inertial>
            <origin xyz="${o_xyz}" rpy="${o_rpy}"/>
            <mass value="${m}"/>
            <inertia ixx="${(m / 12) * (y*y + z*z)}" ixy="0.0" ixz="0.0" 
                     iyy="${(m / 12) * (x*x + z*z)}" iyz="0.0" 
                     izz="${(m / 12) * (x*x + y*y)}"/>
        </inertial>
        
    </xacro:macro>

</robot>
```

```xml
    <link name="base_link">
        <visual>
            <geometry>
                <box size="${base_length} ${base_width} ${base_height}" />
            </geometry>
            <origin xyz="0 0 0.05" rpy="0 0 0" /> 
            <material name="green" />
        </visual>
        <xacro:box_inertia m="5.0" x="${base_length}" y="${base_width}" z="${base_height}"
                           o_xyz="0 0 0.05" o_rpy="0 0 0"/>
    </link>
```

- **Complete macros**
```xml
    <xacro:macro name="box_inertia" params="m x y z o_xyz o_rpy">
        <inertial>
            <origin xyz="${o_xyz}" rpy="${o_rpy}"/>
            <mass value="${m}"/>
            <inertia ixx="${(m / 12) * (y*y + z*z)}" ixy="0.0" ixz="0.0" 
                     iyy="${(m / 12) * (x*x + z*z)}" iyz="0.0" 
                     izz="${(m / 12) * (x*x + y*y)}"/>
        </inertial>
    </xacro:macro>

    <xacro:macro name="wheel_inertia" params="m r l o_xyz o_rpy">
        <inertial>
            <origin xyz="${o_xyz}" rpy="${o_rpy}"/>
            <mass value="${m}"/>
            <inertia ixx="${(m/12) * (3*r*r + l*l)}" ixy="0.0" ixz="0.0" 
                     iyy="${(m/12) * (3*r*r + l*l)}" iyz="0.0" 
                     izz="${(m/2)*r*r}"/>
        </inertial>
        
    </xacro:macro>

    <xacro:macro name="sphere_inertia" params="m r o_xyz o_rpy">
        <inertial>
            <origin xyz="${o_xyz}" rpy="${o_rpy}"/>
            <mass value="${m}"/>
            <inertia ixx="${(2/5)*m*r*r}" ixy="0.0" ixz="0.0" 
                     iyy="${(2/5)*m*r*r}" iyz="0.0" 
                     izz="${(2/5)*m*r*r}"/>
        </inertial>
        
    </xacro:macro>
```

- **And their usages**
```xml
<link name="base_link">
        <visual>
            <geometry>
                <box size="${base_length} ${base_width} ${base_height}" />
            </geometry>
            <origin xyz="0 0 0.05" rpy="0 0 0" /> 
            <material name="green" />
        </visual>
        <xacro:box_inertia m="5.0" x="${base_length}" y="${base_width}" z="${base_height}"
                           o_xyz="0 0 0.05" o_rpy="0 0 0"/>
    </link>

    <xacro:macro name="wheel_link" params="prefix">
        <link name="${prefix}_wheel_link">
            <visual>
                <geometry>
                    <cylinder radius="${wheel_radius}" length="${wheel_length}" />
                </geometry>
                <origin xyz="0 0 0" rpy="${pi / 2.0} 0 0" /> 
                <material name="gray" />
            </visual>   
            <xacro:wheel_inertia m="1.0" r="${wheel_radius}" l="${wheel_length}" 
                                 o_xyz="0 0 0" o_rpy="${pi / 2.0} 0 0"/>             
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
            <xacro:sphere_inertia m="0.5" r="${wheel_radius / 2.0}" 
                                  o_xyz="0 0 0" o_rpy="0 0 0"/>
        </link>
    </xacro:macro>
```


## 43. Collisions in urdf
It is used to avoid collisions.
In wheel, sphere is used because sphere causes only a single point of contact whereas if cylinder was used, there will be a line colliding.
```xml
<link name="base_link">
        <visual>
            <geometry>
                <box size="${base_length} ${base_width} ${base_height}" />
            </geometry>
            <origin xyz="0 0 0.05" rpy="0 0 0" /> 
            <material name="green" />
        </visual>
        <collision>
            <geometry>
                <box size="${base_length} ${base_width} ${base_height}"/>
            </geometry>
            <origin xyz="0 0 0.05" rpy="0 0 0"/>
        </collision>
        <xacro:box_inertia m="5.0" x="${base_length}" y="${base_width}" z="${base_height}"
                           o_xyz="0 0 0.05" o_rpy="0 0 0"/>
    </link>

    <xacro:macro name="wheel_link" params="prefix">
        <link name="${prefix}_wheel_link">
            <visual>
                <geometry>
                    <cylinder radius="${wheel_radius}" length="${wheel_length}" />
                </geometry>
                <origin xyz="0 0 0" rpy="${pi / 2.0} 0 0" /> 
                <material name="gray" />
            </visual>
            <collision>
                <geometry>
                    <sphere radius="${wheel_radius}"/>
                </geometry>
                <origin xyz="0 0 0" rpy="0 0 0"/>
            </collision>
            <xacro:wheel_inertia m="1.0" r="${wheel_radius}" l="${wheel_length}" 
                                 o_xyz="0 0 0" o_rpy="${pi / 2.0} 0 0"/>             
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
            <collision>
                <origin xyz="0 0 0" rpy="0 0 0" />
                <geometry>
                    <sphere radius="${wheel_radius / 2.0}" />
                </geometry>
            </collision>
            <xacro:sphere_inertia m="0.5" r="${wheel_radius / 2.0}" 
                                  o_xyz="0 0 0" o_rpy="0 0 0"/>
        </link>
    </xacro:macro>

```

>[!info] Remember
>1. Never forget to insert the inertial and collision tags.
>2. If you forget inertial tag, you'll not see that link
>3. If you forget collision tag, the link will go beyond the ground.

>[!important] How to Run in Gazebo?
>1. **Terminal 1:** hetansh@Hetansh-PC:~/ros2_ws/src/my_robot_description/urdf$ `ros2 run robot_state_publisher robot_state_publisher --ros-args -p robot_description:="$(xacro agv.urdf.xacro)"` 
>2. **Terminal 2:** hetansh@Hetansh-PC:~$ `ros2 launch ros_gz_sim gz_sim.launch.py gz_args:="empty.sdf -r"`
>3. **Terminal 3:** hetansh@Hetansh-PC:~$ `ros2 run ros_gz_sim create -topic robot_description`
>4. **Terminal 4:** hetansh@Hetansh-PC:~$ `ros2 run rviz2 rviz2 -d ros2_ws/src/my_robot_description/rviz/agv_urdf_config.rviz`  
****************************

>[!warning] Rviz issue
> Wheels are at the centre, is a issue due to missing plugin in gazebo.
> ==Will be fixed later== 

## 45. Launch file to start robot in gazebo
**Aim:**
![[Pasted image 20250818191148.png]]

**Solution:**
```xml
<launch>
    <let name="urdf_path" 
         value="$(find-pkg-share my_robot_description)/urdf/agv.urdf.xacro" />

    <let name="agv_urdf_config_path" 
         value="$(find-pkg-share my_robot_description)/rviz/agv_urdf_config.rviz" />
    
    <node pkg="robot_state_publisher" exec="robot_state_publisher" >
        <param name="robot_description" value="$(command 'xacro $(var urdf_path)')" />
    </node>

    <include file="$(find-pkg-share ros_gz_sim)/launch/gz_sim.launch.py" >
        <arg name="gz_args" value="empty.sdf -r"/>
    </include>

    <node pkg="ros_gz_sim" exec="create" args="-topic robot_description"/>

    <node pkg="rviz2" exec="rviz2" output="screen" args="-d $(var agv_urdf_config_path)"/>
</launch>
```

> This is used to launch using the bringup file

## 47. Gazebo plugins
Gazebo plugin file:
```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">

    <gazebo reference="castor_link">
        <mu1 value="0.01"/>
        <mu2 value="0.01"/>
    </gazebo>


    <gazebo>
        <plugin filename="gz-sim-diff-drive-system"
                name="gz::sim::systems::DiffDrive">
            <left_joint>base_left_wheel_joint</left_joint>
            <right_joint>base_right_wheel_joint</right_joint>
            <wheel_seperation>0.24</wheel_seperation>
            <wheel_radius>0.07</wheel_radius>
            <frame_id>odom</frame_id>
            <child_frame_id>base_footprint</child_frame_id>
        </plugin>
    </gazebo>

    <gazebo>
        <plugin filename="gz-sim-joint-state-publisher-system"
                name="gz::sim::systems::JointStatePublisher">
            <joint_name>base_left_wheel_joint</joint_name>
            <joint_name>base_right_wheel_joint</joint_name>
        </plugin>
    </gazebo>
</robot>
```

Castor plugin is used as it would instead introduce a drag to the robot/ more friction.

## 48. Set up Gazebo bridge
```xml
<launch>
    <let name="urdf_path" 
         value="$(find-pkg-share my_robot_description)/urdf/agv.urdf.xacro" />

    <let name="gazebo_config_path" 
         value="$(find-pkg-share my_robot_bringup)/config/gazebo_bridge.yaml" />
         
    <let name="agv_urdf_config_path" 
         value="$(find-pkg-share my_robot_description)/rviz/agv_urdf_config.rviz" />
    
    <node pkg="robot_state_publisher" exec="robot_state_publisher" >
        <param name="robot_description" value="$(command 'xacro $(var urdf_path)')" />
    </node>

    <include file="$(find-pkg-share ros_gz_sim)/launch/gz_sim.launch.py" >
        <arg name="gz_args" value="empty.sdf -r"/>
    </include>

    <node pkg="ros_gz_sim" exec="create" args="-topic robot_description"/>

    <node pkg="ros_gz_bridge" exec="parameter_bridge">
        <param name="config_file" value="$(var gazebo_config_path)"/>
    </node>

    <node pkg="rviz2" exec="rviz2" output="screen" args="-d $(var agv_urdf_config_path)"/>
</launch>

```
add a config file for the bridge
**Config File:**  (.yaml)
```yaml
- ros_topic_name: "/clock"
  gz_topic_name: "/clock"
  ros_type_name: "rosgraph_msgs/msg/Clock"
  gz_type_name: "ignition.msgs.Clock"
  direction: IGN_TO_ROS

- ros_topic_name: "/joint_states"
  gz_topic_name: "/world/empty/model/agv/joint_state"
  ros_type_name: "sensor_msgs/msg/JointState"
  gz_type_name: "ignition.msgs.Model"
  direction: IGN_TO_ROS

- ros_topic_name: "/tf"
  gz_topic_name: "/model/agv/tf"
  ros_type_name: "tf2_msgs/msg/TFMessage"
  gz_type_name: "ignition.msgs.Pose_V"
  direction: IGN_TO_ROS

- ros_topic_name: "/cmd_vel"
  gz_topic_name: "/model/agv/cmd_vel"
  ros_type_name: "geometry_msgs/msg/Twist"
  gz_type_name: "ignition.msgs.Twist"
  direction: ROS_TO_IGN
```

## 50. Create a World in Gazebo
![[Pasted image 20250820191611.png]]

>[!info] Commands
>1. `ign gazebo`
>2. `ign gazebo test_world.sdf`




## Conclusion
![[Pasted image 20250822231907.png]]


## References
https://en.wikipedia.org/wiki/List_of_moments_of_inertia
https://github.com/gazebosim/gz-sim/blob/ign-gazebo6/src/systems/joint_state_publisher
https://github.com/gazebosim/gz-sim/tree/ign-gazebo6/src/systems/diff_drive
https://app.gazebosim.org/MovAi/fuel/models
https://github.com/gazebosim/ros_gz/tree/ros2/ros_gz_bridge