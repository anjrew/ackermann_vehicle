# About this package

In Robotic Operating System (ROS) and robot simulator Gazebo, there is no official package to simulate the Ackerman motion. The existing open-source github was build over many sources and it is outdated. Furthermore, the package could not be run in the latest version of ROS. It gave the compilation errors due to the non-compatibility of versions.

We manage to update our Github repo with the latest version that works with the current version of ROS based on the previous research works. Melodic and Noetic were tested by our team. 

The repaired version was uploaded to our Github. You can download it and improvise it if you want. Please provide a pull request just in case you have an improved version of the code. 

## This package is updated from original source to simulate the motion of the Ackerman drive in Gazebo

Since most of the previous repo used previous version of ROS, I faced compilation error in ROS Noetic version running Ubuntu 20.04. 
Some of the launch file issues due to the newer ROS version affected the launch file. The issue can be seen [>here<](https://answers.ros.org/question/122021/xacro-problem-invalid-param-tag-cannot-load-command-parameter-robot_description/)


## Tested version: 

1. ROS Noetic running Ubuntu 20.04
2. ROS Melodic running Ubuntu 18.04

If you face running error in Python (for ROS Melodic) you can try to change the heading of the Python to Python3 in Python files. 

Change from this:

```python
#!/usr/bin/env python
```

To this:

```python
#!/usr/bin/env python3
```

## Ackermann_vehicle (updated with ROS Noetic)

ROS packages for simulating a vehicle with Ackermann steering

## Installation (Noetic)

```bash
cd ~/catkin_ws/src
git clone https://github.com/aizzat/ackermann_vehicle/
sudo apt install ros-noetic-ackermann-msgs
cd ~/catkin_ws
rosdep install --from-paths src --ignore-src -r -y
catkin_make
```

## Installation (Melodic)

```bash
cd ~/catkin_ws/src
git clone https://github.com/aizzat/ackermann_vehicle/
sudo apt install ros-melodic-ackermann-msgs
cd ~/catkin_ws
rosdep install --from-paths src --ignore-src -r -y
catkin_make
```

## Running

```bash
roslaunch ackermann_vehicle_gazebo ackermann_vehicle_noetic.launch
```

## To test the steering command:

```bash
#go to terminal to this python directory:
#/ackermann_vehicle/ackermann_vehicle_navigation/scripts/
# /ackermann_vehicle/ackermann_vehicle_navigation/scripts$
# Run the following commands:
./cmd_vel_to_ackermann_drive.py
```

This python file will wait for /cmd_vel inputs. You can test it with another terminal. For example:

```bash
rostopic pub -r 10 /cmd_vel geometry_msgs/Twist  '{linear:  {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0,y: 0.0,z: 0.2}}'
```

![Test run steering terminal](images/testrunackermann.jpg)

## Video - Please click on the image

[![Watch the video](https://img.youtube.com/vi/nZZEMrxxz2o/maxresdefault.jpg)](https://youtu.be/nZZEMrxxz2o)


## Troubleshooting

With the following error on launching the Gazebo:

```bash
Traceback (most recent call last):
  File "/opt/ros/noetic/lib/controller_manager/spawner", line 220, in <module>
    if __name__ == '__main__': main()
  File "/opt/ros/noetic/lib/controller_manager/spawner", line 189, in main
    name_yaml = yaml.load(open(name))
TypeError: load() missing 1 required positional argument: 'Loader'
```

This error is due to a change in the PyYAML library. In newer versions, you need to specify a Loader when calling yaml.load() for security reasons.
To fix this, we need to modify the controller spawner script. Here's how we can do that:

Locate the spawner script:

```bash
sudo nano /opt/ros/noetic/lib/controller_manager/spawner
```

Find the line that says:

```python
name_yaml = yaml.load(open(name))
```

Change it to:

```python
name_yaml = yaml.safe_load(open(name))
```

Save and exit the editor.