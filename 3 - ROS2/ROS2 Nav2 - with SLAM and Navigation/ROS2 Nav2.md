## Section 2 - Setup and installation of Nav2

`hetansh@Hetansh-PC:~$ sudo apt install ros-humble-navigation2 ros-humble-nav2-bringup ros-humble-turtlebot3*
`
## Section 3 - Generate a Map with SLAM
### 9. Intro
![[Pasted image 20250823000928.png]]
### 10. Make the robot move in the world
`export TURTLEBOT3_MODEL=waffle`
`source /opt/ros/humble/setup.bash`
`source ~/ros2_ws/install/setup.bash`
Add the lines in the .bashrc file

### 11. Generate and save a Map with SLAM
`ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py`
`ros2 launch turtlebot3_cartographer cartographer.launch.py`
`ros2 run nav2_map_server map_saver_cli -f maps//my_map`
![[Pasted image 20250823114516.png]]

**White->** Free Space
**Black->** Obstacle
**Gray->** Unknown Space


```yaml
image: my_map.pgm
mode: trinary
resolution: 0.05 # resolution of pixel. I pixel is of 5 cm.
origin: [-1.29, -2.42, 0] # bottom left of the map is the origin 
negate: 0
occupied_thresh: 0.65 # pixel value below 65% of 255 is considered as occupied
free_thresh: 0.25 # pixel value below 25% of 255 is considered as unoccupied
```

```pgm
P5
125 117 # 125 * 0.05 metres X 117 * 0.05 is the area
255
�������������������������������������������������������������������������������>
```

![[Pasted image 20250823123834.png]]


## Section 4 - Make a robot navigate with Nav2
### 17. A Quick Fix : ==IMPORTANT==
You need to use cyclone dds instead of fast dds
`sudo apt install ros-humble-rmw-cyclonedds-cpp` - to install cyclone DDS
add the following into .bashrc: `export RMW_IMPLEMENTATION=rnw_cyclonedds_cpp`

### 18. Make the robot navigate
`ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=maps/my_map.yaml `
use the above command to open rviz and initialise the robot position using 2D pose estimate

## Section 5 - Understand the Nav2 Stack
### 25. Parameters
You can tweek the values of the costmap by going into rqt.
![[Pasted image 20250823172232.png]]
Go into Plugins -> Configuration -> Dynamic Reconfigure
you can change values under :
- controller_server
- global_costmap
- local_costmap

### 27. TFs and important Frames
![[Pasted image 20250823174845.png]]![[Pasted image 20250823175222.png]]
map -> odom -> base_footprint

### 28. The Nav2 architecture
![[Pasted image 20250823180747.png]]

## Section 6 - Build your own world for nav in gazebo
### 36. Tip: How to fix and improve maps with Gimp
`sudo snap install gimp` - installation of gimp
open gimp by `gimp` and open the .pgm file and edit the walls and open space accordingly.

## Section 7 - Intro to adapting a custom robot for Nav2
### 42. TF/URDF
![[Pasted image 20250823222920.png]]
![[Pasted image 20250823223158.png]]

/base_scan origin should be in centre of the cylinder
refer 
-> `cd /opt/ros/humble/share/turtlebot3_description` 
-> `cd urdf`
-> `ros2 launch urdf_tutorial display.launch.py model:= turtlebot3_waffle.urdf`
![[Pasted image 20250823224424.png]]

### 43. Input/Output - Odometry, Sensors and Controller
![[Pasted image 20250823224844.png]]

![[Pasted image 20250823225923.png]]
This is also a good option if your motor has encoders

![[Pasted image 20250823233154.png]]
![[Pasted image 20250823233301.png]]

## References
https://www.mazegenerator.net/ - to generate maze