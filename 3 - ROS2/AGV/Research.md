
**Date**: 2025-07-19 **Time**: 14:22
**Status**: #baby 
**Tags**: 
# Research

SLAM - Simultaneous Localization and Mapping
- Feature SLAM - Occurs on the basis of features or landmark objects.
- Grid SLAM - Occurs by dividing the world into ==grids==, which can be Occupied or Unoccupied.

Slam Toolbox is a Grid based approach.


### Coordinate frames
- Base_link - Frame attached to the robot.
- odom - It is the origin point that is in the environment (world origin)
- map - "World/Global origin"


### Odometry
There can be a drift in the values because of accumulation of small errors over time. Therefore, we use multiple sensors in order to get precise odom values.
- IMU: 
	- **Accelerometer** can be used to get velocity and position by integrating and double integrating the values.
	- **Gyroscope** can be used in order to calculate the orientation or the angular velocity from the values of Roll, Pitch and Yaw.
- Visionary Data
- Encoders
You can use **Kalman Filters** in order to get higher accuracy by integrating these and get precise values.
- **Extended Kalman Filter (EKF)** or **Complementary Filter** is commonly used.
- These combine encoder, IMU, and possibly GPS or LiDAR data.
- Helps correct drift and improves overall localization.







 
## References
https://www.ros.org/reps/rep-0105.html