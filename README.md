# Multi sensorized quadcopter for PX4 Gazebo simulator 

This repository contains all necessary files to run the PX4 firmware on ROS-Gazebo simulation, usisng an IRIS based quadrotor equipped with a camera, a depth camera, a laser scanner and a sonsar sensor. 

To start the simulation clone the repository including all the submodules:
		
        $ git clone https://github.com/jocacace/Firmware --recursive
        
4. Compile and launch it!		
		
        $ cd Firmware && make posix_sitl_default gazebo -j4

    During the installation you should install some additional dependencies (follow the instruction in the compilation shell):
    
        $ sudo apt-get install python-pip
        $ sudo apt-get install python-jinja2
        $ sudo pip install numpy toml
        $ sudo apt-get install protobuf-c*

    To allow ROS/FCU communication you should install Mavros package:

        $ sudo apt-get install ros-kinetic-mavros-*
    
    Now you can install the geographic dataset

        $ sudo /opt/ros/kinetic/lib/mavros/install_geographiclib_datasets.sh


    Finally, some robots use custom sensors from hector-quadrotor stack, install it:

        $ sudo apt-get install ros-kinetic-hector-*


5. To launch the Iris version equipped with the camera, you should load the gazebo configuration. You could use the __load_sitl_conf.sh__ script placed in the sitl_gazebo folder

		$ cd Tools/sitl_gazebo && source load_sitl_conf.sh
   

### Setup the environment
In ordert to speed up the configuration process could be convienent to add the following link to your .bashrc file:

		export sitl_location='/PATH/TO/PX4/Firmware/Tools/sitl_gazebo'
		
You can now configure the environment in a fast way:
	
		cd $sitl_location
		source load_sitl_conf.sh
		cd    

### Install Gazebo for MAVLink SITL

You can refer to the official [PX4 Simulation](https://dev.px4.io/en/simulation/) documentation to properly install and run the Gazebo Mavlink simulator.

### Use a camera enabled quadrotor with Mavros
We can use Mavros package to [MAVROS](http://wiki.ros.org/mavros) ROS package to control the camera enabled IRIS quadrotor.

##### Launch the simualtion scene
After properly configured the environment, launch the simulation use this command:

		roslaunch px4 terrain_follower.launch

You can check the active topics and services to test the correct working of MAVROS bridge. 

### Main differences with the Main PX4 repo:
The main differences with the original Firmware repository lie in the configuration files:
		export sitl_location='/PATH/TO/PX4/Firmware/Tools/sitl_gazebo'

		Firmware/posix-configs
		Firmware/Tools/sitl-gazebo/models
		Firmware/Tools/sitl-gazebo/launch
### Sensors:
The best way to interact with the simulated sensors of the UAV is using ROS.
For each sensor, a related ROS topic streams the desired data. Considering
the terrain follower drone, three range finder sensors publish distance data
on the following topics:
- **/distance_forward**: the forward range sensor -> (0.2, 0.0, -0.05) with a tilt angle of 1.05 radians.
- **/distance_middle**: the central range sensor -> (0.0, 0.0, -0.05), with a tilt angle of 0.0 radians.
- **/distance_backward**: the backward range sensor -> (-0.2, 0.0, -0.05), with a tilt angle of 1.05 radians.



