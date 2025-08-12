**Date**: 2025-08-12 **Time**: 17:00
**Status**:
**Tags**: 
# Section 4 - Broadcast TFs with the Robot State Publisher

## 22. Run the robot state publisher
`ros2 run robot_state_publisher robot_state_publisher --ros-args -p robot_description:="$(xacro agv.urdf)"` [[command]]

`ros2 run joint_state_publisher_gui joint_state_publisher_gui` [[command]]

`ros2 run rviz2 rviz2` [[command]] - to see the robo


## 24. Launch File for URDF
```xml
<launch>
    <let name="urdf_path" 
         value="$(find-pkg-share my_robot_description)/urdf/agv.urdf" />
    
    <node pkg="robot_state_publisher" exec="robot_state_publisher" >
        <param name="robot_description" value="$(command 'xacro $(var urdf_path)')" />
    </node>

    <node pkg="joint_state_publisher_gui" exec="joint_state_publisher_gui" />

    <node pkg="rviz2" exec="rviz2" />
</launch>
```

## 26. Add Rviz config in launch file
```xml
<launch>
    <let name="urdf_path" 
         value="$(find-pkg-share my_robot_description)/urdf/agv.urdf" />
    
    <node pkg="robot_state_publisher" exec="robot_state_publisher" >
        <param name="robot_description" value="$(command 'xacro $(var urdf_path)')" />
    </node>

    <node pkg="joint_state_publisher_gui" exec="joint_state_publisher_gui" />

    <node pkg="rviz2" exec="rviz2" output="screen" args="-d $(find-pkg-share my_robot_description)/rviz/agv_urdf_config.rviz"/>
</launch>
```




## References
https://www.udemy.com/course/ros2-tf-urdf-rviz-gazebo/learn/lecture/38689068#overview