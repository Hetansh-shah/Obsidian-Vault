**Date**: 2025-08-21 **Time**: 12:39
**Status**: 
**Tags**: [[ROS 2 - TFs]]
# Section 7 - Adding a sensor in Gazebo
## 53. Add a camera to urdf
Create a new Xacro file for the camera link
```xml
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">

    <xacro:property name="camera_length" value="0.01" />
    <xacro:property name="camera_width" value="0.05" />
    <xacro:property name="camera_height" value="0.02" />
    <xacro:property name="camera_pitch" value="${pi / 12.0}" />

    <link name="camera_link">
        <visual>
            <origin xyz="0.0 0.0 0.0" rpy="0.0 0.0 0.0"/>
            <geometry>
                <box size="${camera_length} ${camera_width} ${camera_height}"/>
            </geometry>
            <material name="gray" />            
        </visual>
        <collision>
            <origin xyz="0.0 0.0 0.0" rpy="0.0 0.0 0.0"/>
            <geometry>
                <box size="${camera_length} ${camera_width} ${camera_height}"/>
            </geometry>
        </collision>     
        <xacro:box_inertia m="0.1" x="${camera_length}" y="${camera_width}" z="${camera_height}"
                           o_xyz="0 0 0" o_rpy="0 0 0"/>   
    </link>

    <joint name="base_camera_joint" type="fixed">
        <origin xyz="${base_length / 2.0} 0.0 0.25" rpy="0.0 ${camera_pitch} 0.0"/>
        <parent link="base_link"/>
        <child link="camera_link"/>
    </joint>
    
    <gazebo reference="camera_link">
        <sensor name="camera" type="camera">
            <camera>
                <horizontal_fov>1.3962634</horizontal_fov>
                <image>
                    <width>640</width>
                    <height>480</height>
                    <format>R8G8B8</format>
                </image>
                <clip>
                    <near>0.1</near>
                    <far>15</far>
                </clip>
                <noise>
                    <type>gaussian</type>
                    <mean>0.0</mean>
                    <stddev>0.007</stddev>
                </noise>
                <optical_frame_id>camera_link</optical_frame_id>
                <camera_info_topic>camera/camera_info</camera_info_topic>
            </camera>
            <always_on>1</always_on>
            <update_rate>20</update_rate>
            <visualize>true</visualize>
            <topic>camera/image_raw</topic>            
        </sensor>
    </gazebo>

</robot>
```

## 54. Add a Gazebo plugin for Camera
Add the below part in the test_world.sdf:
```xml
    <plugin name='gz::sim::systems::Physics' filename='ignition-gazebo-physics-system'/>
    <plugin name='gz::sim::systems::UserCommands' filename='ignition-gazebo-user-commands-system'/>
    <plugin name='gz::sim::systems::SceneBroadcaster' filename='ignition-gazebo-scene-broadcaster-system'/>
    <plugin name='gz::sim::systems::Contact' filename='ignition-gazebo-contact-system'/>
    <plugin name='gz::sim::systems::Sensors' filename='ignition-gazebo-sensors-system'>
      <render_engine>ogre2</render_engine>
    </plugin>
```

Added the following in gazebo_bridge.yaml
```yaml
- ros_topic_name: "/camera/camera_info"
  gz_topic_name: "/camera/camera_info"
  ros_type_name: "sensor_msgs/msg/CameraInfo"
  gz_type_name: "ignition.msgs.CameraInfo"
  direction: GZ_TO_ROS

- ros_topic_name: "/camera/image_raw"
  gz_topic_name: "/camera/image_raw"
  ros_type_name: "sensor_msgs/msg/Image"
  gz_type_name: "ignition.msgs.Image"
  direction: GZ_TO_ROS
```




## References
https://github.com/gazebosim/ros_gz/tree/ros2/ros_gz_bridge