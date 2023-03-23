# Ros Cheat Sheet

## Table of Contents
- [ROS Basics](#rosbasics)
- [Installing ROS](#installingros)
  - [Linux](#installingroslinux)
  - [Windows](#installingroswindows)
- [ROS Command Line](#roscmdline)
- [ROS Nodes](#rosnodes)
- [ROS Topics](#rostopics)
- [ROS Messages](#rosmessages)
- [ROS Parameters](#rosparameters)
- [ROS Launch](#roslaunch)
- [Create ROS project](#createrosproject)
  - [Create ROS workspace](#createrosworkspace)
  - [Create ROS package](#createrospackage)
  - [Most used dependencies](#mostuseddependencies)
  - [Add files to package](#addfilestopackage)
  - [Build package](#buildpackage)
  - [Source workspace](#sourceworkspace)
  - [Test package](#testpackage)

<a name="rosbasics"></a>
## ROS Basics

- ROS is a middleware that allows communication between different parts of a robotic system.
- ROS uses a publish-subscribe messaging system.
- ROS nodes are independent processes that communicate with each other.
- ROS topics are named buses over which nodes communicate.
- ROS messages are the data structures used to communicate between nodes over topics.

<a name="installingros"></a>
## Installing ROS
<a name="installingroslinux"></a>
### Installing ROS on Linux
1- Choose your version of ROS (e.g., Kinetic, Melodic, Noetic)
2- Configure Ubuntu repositories:
```
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
```
3- Set up your sources.list:
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
4- Set up your keys:
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
```
5- Install ROS:
```
sudo apt-get update
sudo apt-get install ros-<version>-desktop-full
```
Replace `<version>` with the ROS version you want to install.
6- Initialize ROS:
```
sudo rosdep init
rosdep update
echo "source /opt/ros/<version>/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
Replace `<version>` with the ROS version you installed.
And that's it! You should now have ROS installed on your Linux machine. Remember to test your installation by running `roscore`.

<a name="installingroswindows"></a>
### Installing ROS on windows
1- First install visual studio 2019 or higher (make sure you install C++ development tools)
2- Ros noetic or higher is advised.
3- Install chocolatey:
- In the Start Menu, find the "x64 Native Tools Command Prompt for VS 2019" item.
- Right Click, select More then "Run as Administrator"
- Copy the following command line:
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
- Paste it into the command window.
- Approve any prompts
- Once it has completed, close the command prompt to complete the install.

4- install git: First verify that git is installed by typing:
```
git --version
```
If you have an error then you need to install git:
```
choco upgrade git -y
```
Verify again and it should work
```
git --version
```
5- Binary Package Installation
To get things started, install the recommended desktop_full metapackage. A Metapackage is a collection of other packages. The Desktop-Full metapackage refers to a number of other packages needed to build, run, debug and visualize a robot.

If you have closed it, open the Visual Studio Command Prompt as Administrator as described above.
- first we activate Powershell:
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass
```
Then we install ros. Here is an example for installing ros noetic :
```
mkdir c:\opt\chocolatey
set ChocolateyInstall=c:\opt\chocolatey
choco source add -n=ros-win -s="https://aka.ms/ros/public" --priority=1
choco upgrade ros-noetic-desktop_full -y --execution-timeout=0
```
You are almost there!

You may need to reboot your system. Make sure to read the output text. If reboot is required, reboot the system and reopen x64 Native Tools Command Prompt for VS 2019 .

We need to source ros by executing:
```
C:\opt\ros\noetic\x64\setup.bat
```
Now you are ready to use ros. You can launch roscore for example:
```
roscore
```



<a name="roscmdline"></a>
## ROS Command Line

- `roscd` changes the current directory to a package in the ROS workspace.
- `rosrun` runs a node from a package.
- `rostopic` list lists all available topics.
- `rostopic` echo displays the messages being sent over a topic.
- `rosmsg` show displays the structure of a message.

<a name="rosnodes"></a>
## ROS Nodes:

- `roscore` initializes the ROS master and should be the first command to run when starting a ROS system.
- `rosnode` list lists all running nodes.
- `rosnode` info displays information about a specific node.
- `rosnode` kill terminates a node.

<a name="rostopics"></a>
## ROS Topics:

- `rostopic` pub publishes a message to a topic.
- `rostopic` hz displays the publishing rate of a topic.
- `rostopic` bw displays the bandwidth usage of a topic.
- `rosmsg` show displays the structure of a message.

<a name="rosmessages"></a>
## ROS Messages:

- `rosmsg` show displays the structure of a message.
- `rosmsg` info displays information about a specific message type.
- `rosmsg` list lists all available message types.

<a name="rosservices"></a>
## ROS Services:

A service is a request-response communication between nodes.
- `rosservice` list lists all available services.
- `rosservice` call sends a request to a service.
- `rosservice` info displays information about a specific service.

<a name="rosparameters"></a>
## ROS Parameters:

Parameters are global variables that can be accessed by all nodes.
- `rosparam` set sets the value of a parameter.
- `rosparam` get retrieves the value of a parameter.
- `rosparam` list lists all available parameters.

<a name="roslaunch"></a>
## ROS Launch:

roslaunch is a tool for launching multiple nodes and configuring their parameters in one command.
These are just some of the most commonly used ROS commands and concepts. For more detailed information, refer to the ROS documentation.



<a name="createrosproject"></a>
## Create a ros project
<a name="createrosworkspace"></a>
1- Create a new ROS workspace:
```
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
```
This will create a new ROS workspace in your home directory and compile the packages.
<a name="createrospackage"></a>
2- Create a new ROS package:
```
cd ~/catkin_ws/src
catkin_create_pkg <package_name> [dependencies]
```
Replace `<package_name>` with the name of your package and `[dependencies]` with any dependencies your package may have.

<a name="mostuseddependencies"></a>
### Most used dependencies:
- roscpp: This package provides the basic C++ implementation of ROS. It allows you to create ROS nodes and interact with the ROS system.

- rospy: This package provides the Python implementation of ROS. It allows you to create ROS nodes and interact with the ROS system using Python.

- rviz: This package provides a 3D visualization tool for ROS. It allows you to visualize the state of your robot and the environment in which it operates.

- message_generation: This package provides tools for generating ROS message and service code from .msg and .srv files.

- geometry_msgs: This package provides messages for representing geometric primitives, such as points, vectors, and poses.

- sensor_msgs: This package provides messages for representing sensor data, such as images, laser scans, and IMU measurements.

- tf: This package provides tools for working with coordinate frames and transformations in ROS. It allows you to keep track of the positions and orientations of objects in your robot's environment.

- actionlib: This package provides tools for defining and executing long-running actions in ROS. It allows you to write complex behavior sequences, such as moving a robot arm to a specific position.

- std_msgs: This package provides a set of standard message types that can be used to represent common data types in ROS. Some of the message types provided by std_msgs include:

  std_msgs/Int8: An 8-bit signed integer
  std_msgs/Int16: A 16-bit signed integer
  std_msgs/Int32: A 32-bit signed integer
  std_msgs/Int64: A 64-bit signed integer
  std_msgs/Float32: A 32-bit floating point number
  std_msgs/Float64: A 64-bit floating point number
  std_msgs/String: A string of characters
  std_msgs/Bool: A boolean value (true or false)
  You can use these message types to define the format of messages that your ROS nodes will send and receive.

<a name="addfilestopackage"></a>
3- Add files to your package:
```
cd ~/catkin_ws/src/<package_name>
```
You can add nodes, launch files, and other resources to your package here.
<a name="buildpackage"></a>
4- Build your package:
```
cd ~/catkin_ws/
catkin_make
```
This will build your package and make it available to ROS.
<a name="sourceworkspace"></a>
5- Source your workspace:
```
source ~/catkin_ws/devel/setup.bash
```
This will set up your workspace environment variables so that ROS can find your package.
<a name="testpackage"></a>
6- Test your package:
```
roscore &
rosrun <package_name> <node_name>
```
