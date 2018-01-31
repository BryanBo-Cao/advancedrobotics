# Advanced Robotics Fall 2015
## Stewart Platform Fork

Code repository and issue tracker for the Fall 2015 of "Advanced Robotics" (Independent Study group)

http://correll.cs.colorado.edu/?page_id=3748

Demo: https://www.youtube.com/watch?v=Tb0sU9bNYIY

# Stewart Platform

This repo contains the files necessary to build and control a Stewart platform.

# Getting Started

This code uses:

- Ubuntu 14.04

- ROS Indigo

### Setup the ROS Environment 

Install ROS indigo if you haven't already: http://wiki.ros.org/indigo/Installation/Ubuntu

Setup environment variables:
```
		source /opt/ros/indigo/setup.bash
```

Setup the workspace:
```
		mkdir -p ~/stewart_ws/src
		cd ~/stewart_ws/src
		catkin_init_workspace
```
### Install ROS Dependencies

```
sudo apt-get install ros-indigo-rosserial-arduino ros-indigo-rosserial ros-indigo-rosserial-server
```

### Clone this repo into the catkin workspace

```
cd ~/stewart_ws/src
git clone <https://address>
mv advancedrobotics/ stewart_platform/
```

### Build this workspace

```
cd ~/stewart_ws/src/stewart_platform
mkdir include
catkin build
source ~/stewart_ws/devel/setup.bash
sudo cp ~/stewart_ws/src/stewart_platform/dev_rules/*.rules /etc/udev/rules.d/
roscd stewart_platform/
```

### Setup the environment variables to be sourced on startup

```
sudo gedit ~/.bashrc
```

Add these 2 lines at the bottom:
```
source /opt/ros/indigo/setup.bash
source ~/stewart_ws/devel/setup.bash
```

# Running the Stewart Platform

Install Dynamixel Dependencies:
```
sudo apt-get install ros-indigo-dynamixel-motor
```

Make sure roscore is running. If not, run this command in a separate terminal:
```
roscore
```

In a new terminal, bring up the dynamixel servos:
```
roslaunch stewart_platform controller_manager.launch
```

## In a new terminal(s):
### Stewart Platform Test
You should see the motors move to the min range, max range, and then a neutral position when running this test. If they don't move, but you don't have any errors, try restarting everything. If you have errors, good luck!
```
rosrun stewart_platform stewart_ik_test
```

### Communication with Sparkfun Razor 9DOF IMU

```
rosrun stewart_platform stewart_accel_test
```

A [Razor](https://github.com/ptrbrtz/razor-9dof-ahrs/wiki/Tutorial) tutorial. Use this tutorial to calibrate the Razor.

**NOTE** Yaw will not be correct if the magnetometer is not set up properly.

You can view the data in a serial monitor tool, such as Arduino or PuTTy. 
#### To use Arduino:
Install arduino from webpage, not distro.

`rosserial_arduino` Tutorials are [here](http://wiki.ros.org/rosserial_arduino/Tutorials)

Start Arduino

Change the Serial Port to the one that the Sparkfun Razor is plugged into by navigating to and changing Tools -> Serial Port -> /dev/ttyUSB#

Navigate to Tools -> Serial Monitor

Change the baud rate to 57600

You should see scrolling data that changes when you move the IMU around and that looks similar to:
```
#YPR=-157.65,2.94,-0.14
#YPR=-157.67,2.95,-0.15
#YPR=-157.69,2.95,-0.14
#YPR=-157.72,2.96,-0.13
...
```


## Windows Dynamixel Manager
download the [RoboPlus software](http://en.robotis.com/BlueAD/board.php?bbs_id=downloads&mode=view&bbs_no=1132559&page=1&key=&keyword=&sort=&scate=SOFTWARE) from dynamixel

see [this page](http://learn.trossenrobotics.com/34-blog/140-ftdi-2-12-00-notice-robotis-usb2dynamixel-cm-530-and-ln-101-october-2014.html) for driver installation problems

Use the Dynamixel wizard to set up the servos. 2, 4, 6 need to be in the reverse direction mode. The files in this package assume the communication baud rate is (0x34) 57600.


# Hardware Build Notes

* Remember to wire up the servos before installing them
* Add a laser cut hole to bottom of servo stands for easer cable routing
* Ensure servo horns are aligned to the zero position.
* Make all bars the same length and command servos to same position before installing the upper plate.
