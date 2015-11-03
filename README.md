#r2d2

##Simple model of the r2d2 robot for ROS/Gazebo.

To run (simulate) the model:

`roslaunch r2d2_bringup bringup.launch`

To control the model:

`rqt`

In rqt, 
    
`Perspectives->Import-><~/catkin_ws/src/r2d2/r2d2_control/rqt/r2d2.perspective>`

To run only in rviz:

`roslaunch r2d2_description r2d2_publisher.launch`

To control via joystick (teleop) install https://github.com/ros-teleop/teleop_twist_joy

```
cd ~/catkin_ws/src
git clone https://github.com/ros-teleop/teleop_twist_joy.git
cd ..
catkin_make
```

Change it appropriately (launch file to use the xbox joy instead of ps3 and config file to send proper maximums) and run:

`roslaunch teleop_twist_joy teleop.launch`


To build a map, run the model, run the teleop control and start saving messages in a bag:

`rosbag record -a`

Then, walk the robot around to scan the environment. After that, stop everything and (http://wiki.ros.org/slam_gmapping/Tutorials/MappingFromLoggedData):

```
roscore
rosparam set use_sim_time true
rosrun gmapping slam_gmapping scan:=/r2d2/laser/scan _odom_frame:=/r2d2/odom
rosbag play --clock <name of the bag the you created above >
```

Wait for rosbag to finish and exit. Save your new map to disk using map_saver from the map_server package: 

`rosrun map_server map_saver`


