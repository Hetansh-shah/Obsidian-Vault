**Date**: 2025-08-11 **Time**: 15:24
**Status**: 
**Tags**: [[ROS 2 - TFs]]
# Section 2 - TF (Transform) Overview

## Commands to install 
`sudo apt install ros-humble-urdf-tutorial`
## 8. Visualize a Robot TFs in RViz2
```sh
ros2 launch urdf_tutorial display.launch.py model:=/opt/ros/humble/share/urdf_tutorial/urdf/08-robot.urdf
```

TF is basically a joint between 2 links or structures.
## 9. Relationship between TFs, TF tree

To install TF2 tools: `sudo apt install ros-humble-tf2-tools`
To view the 5 sec data of TF,
`ros2 run tf2_tools view_frames`

## References
https://www.udemy.com/course/ros2-tf-urdf-rviz-gazebo/learn/lecture/38688964#**content