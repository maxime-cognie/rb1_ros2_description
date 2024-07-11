# rb1_ros2_description
---

This is a ROS 2 package for integrating the ros2_control controller in the RB-1 robot in simulation using Gazebo.  

This implements the following controllers:  

  - `diff_drive_controller`: to control the robot motions.  
  - `effort_controllers`: to lift the robot elevator.  


### Instalation
---

#### prerequisites

 - ROS2 Galactic or higher  
 - GAZEBO  

#### Install

 1- Clone the repository inside your workspace:  
`git clone https://github.com/maxime-cognie/rb1_ros2_description.git`  

 2- Build and setup the environment:
Navigate to the root of your workspace, then use the following command:  
```bash
colcon build
source install/setup.bash
```


### getting started  
---

 - To launch the simulation and the controllers:  
`ros2 launch rb1_ros2_description rb1_ros2_xacro.launch.py`  


### Verify controller activation
---

 - To verify the controllers existence:  
```
ros2 service call /rb1_robot/controller_manager/list_hardware_interfaces controller_manager_msgs/srv/ListHardwareInterfaces {}
ros2 service call /rb1_robot/controller_manager/list_controllers controller_manager_msgs/srv/ListControllers {}
```


### Control the robot
---

#### Control the robot motion

 - To send motion command to the robot either use teleoperation with keyboard:    

`ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/rb1_robot/rb1_base_controller/cmd_vel_unstamped`

 - Or directly send command to the robot:   

`ros2 topic pub /rb1_robot/rb1_base_controller/cmd_vel_unstamped geometry_msgs/msg/Twist '{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}' -1`  
Adjust `linear` or `angular` velocity as needed.  

#### Control the robot elevator

 - To lift up or down the robot elevator:  

`ros2 topic pub /rb1_robot/effort_controller/commands std_msgs/msg/Float64MultiArray "{layout: {dim: [{label: 'effort',size: 1, stride: 1}], data_offset: 0}, data:[0.0]}" -1`  
Adjust the `data` value to lift the elevator up or down, positive effort lift it up while negative effort lift it down.  
