# pgm_map_creator
Create pgm map from Gazebo world file for ROS localization

## Environment
Tested on Ubuntu 18.04, ROS Melodic, Gazebo 9.0, Boost 1.65 

## Usage

### Add the package to your workspace
0. Create a catkin workspace
1. Clone the package to the src folder
2. Add the following definition to top of /usr/include/boost/gil/extension/io/png_io_private.hpp:  
`#define int_p_NULL (int*)NULL`
3. in pgm_map_creator/src/collision_map_creator.cc make the following changes due to changes in Gazebo physics::World class method names:  

line 35: Change `getName()` to `Name()`  
line 91: `GetPhysicsEngine()` to `Physics()`  

4. To avoid protobuf compiler errors, in /catkin_ws/src/pgm_map_creator/msgs/CMakeLists.txt:  

change this  
```
set (msgs  
  collision_map_request.proto  
  ${PROTOBUF_IMPORT_DIRS}/vector2d.proto  
  ${PROTOBUF_IMPORT_DIRS}/header.proto  
  ${PROTOBUF_IMPORT_DIRS}/time.proto  
)  
```
to  
```
set (msgs  
  collision_map_request.proto  
)  
```
5. `catkin_make` and source `devel/setup.bash`

### Add the map and insert the plugin
1. Add your world file to world folder
2. Add this line at the end of the world file, before `</world>` tag:
`<plugin filename="libcollision_map_creator.so" name="collision_map_creator"/>`

### Create the pgm map file
1. Open a terminal, run gzerver with the map file
`gzserver src/pgm_map_creator/world/<map file>`
2. Open another terminal, launch the request_publisher node
`roslaunch pgm_map_creator request_publisher.launch`
3. Wait for the plugin to generate map. It will be located in the map folder

## Map Properties
Currently, please update the argument value in launch/request_publisher.launch file.

## Acknowledgements
[Gazebo Custom Messages](http://gazebosim.org/wiki/Tutorials/1.9/custom_messages)
[Gazebo Perfect Map Generator](https://github.com/koenlek/ros_lemtomap/tree/154c782cf8feb9112bc928e33a59728ca2192489/st_gazebo_perfect_map_generator)

