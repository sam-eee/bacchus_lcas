# bacchus_lcas
A repository for contributions to the BACCHUS H2020 project

=======
## 1 - Getting the system

### From source
1. Go to `~/catkin_ws/src` and clone the repo: `git clone https://github.com/LCAS/bacchus_lcas.git`
1. While inside `~/catkin_ws/src` , install all dependencies for the packages with: `rosdep install --from-paths . -i`
1. Go to ~/catkin_ws and compile: `catkin_make` or `catkin build`

### From installed packages
1. Enable ROS and L-CAS Ubuntu repositories: https://github.com/LCAS/rosdistro/wiki#using-the-l-cas-repository
1. install `sudo apt-get install ros-melodic-bacchus-gazebo`

## 2 - Launch the system with:
`roslaunch bacchus_gazebo vineyard_demo.launch`

To change the world you can modify the "world_name" parameter inside the launch file.

### (optional)  Manual teleoperation of thorvald
At this stage, you should be able to control manually the robot with the keyboard.
To do so run:
`rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=/thorvald_001/nav_vel`


## 3 - To send a sequence of goals:
Go to `~/catkin_ws/src/bacchus_lcas/bacchus_move_base/scripts` and run:

`python send_goals.py _goals_seq_file:=../config/goals/vineyard_demo_goals.txt`

The `goals_seq_file` has to be a txt containing 3 columns `[x,y,yaw]`. They define the sequence of goals to be send to the robot.

Or simply send a navigation goal via `rviz`.


## Steps taken to get to this point

Added cpp files to src directory.

Added h files to include directory.

Added cfg folder with GNSSConfig.cfg file.

Added hector_gazebo_plugins, gazebo_plugins, dynamic_reconfigure to dependencies in CMakeLists.txt and package.xml

Added below to CMakeLists.txt

`find_package(catkin REQUIRED dynamic_reconfigure)
generate_dynamic_reconfigure_options(
  cfg/GNSS.cfg
)`


Added below to CMakeLists.txt

`link_directories(${GAZEBO_LIBRARY_DIRS})
add_library(gazebo_ros_gps src/gazebo_ros_gps.cc)`

Added below code to sensors.xarco and vineyard_stage0.world

` <plugin name="gps_controller" filename="libgazebo_ros_gps.so">
    <alwayson>true</alwayson>
    <updaterate>1.0</updaterate>
    <bodyname>base_link</bodyname>
    <topicname>/fix</topicname>
    <velocitytopicname>/fix_velocity</velocitytopicname>
    <drift>5.0 5.0 5.0</drift>
    <gaussiannoise>0.1 0.1 0.1</gaussiannoise>
    <velocitydrift>0 0 0</velocitydrift>
    <velocitygaussiannoise>0.1 0.1 0.1</velocitygaussiannoise>
</plugin> `


Used following commands in src directory

`cmake ../`

`make`

`export GAZEBO_PLUGIN_PATH=${GAZEBO_PLUGIN_PATH}:~/bacchus_ws/devel/lib`

Also used catkin_make in ws directory.

When launching vineyard_demo.launch fails.

To find error use `gzserver ~/bacchus_ws/src/bacchus_lcas/bacchus_gazebo/worlds/vineyard_stage0.world --verbose`
