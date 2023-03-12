# Ros Cheat Sheet

## ROS Basics:

- ROS is a middleware that allows communication between different parts of a robotic system.
- ROS uses a publish-subscribe messaging system.
- ROS nodes are independent processes that communicate with each other.
- ROS topics are named buses over which nodes communicate.
- ROS messages are the data structures used to communicate between nodes over topics.

## ROS Command Line:

- `roscd` changes the current directory to a package in the ROS workspace.
- `rosrun` runs a node from a package.
- `rostopic` list lists all available topics.
- `rostopic` echo displays the messages being sent over a topic.
- `rosmsg` show displays the structure of a message.

## ROS Nodes:

- `roscore` initializes the ROS master and should be the first command to run when starting a ROS system.
- `rosnode` list lists all running nodes.
- `rosnode` info displays information about a specific node.
- `rosnode` kill terminates a node.

## ROS Topics:

- `rostopic` pub publishes a message to a topic.
- `rostopic` hz displays the publishing rate of a topic.
- `rostopic` bw displays the bandwidth usage of a topic.
- `rosmsg` show displays the structure of a message.

## ROS Messages:

- `rosmsg` show displays the structure of a message.
- `rosmsg` info displays information about a specific message type.
- `rosmsg` list lists all available message types.

## ROS Services:

A service is a request-response communication between nodes.
- `rosservice` list lists all available services.
- `rosservice` call sends a request to a service.
- `rosservice` info displays information about a specific service.

## ROS Parameters:

Parameters are global variables that can be accessed by all nodes.
- `rosparam` set sets the value of a parameter.
- `rosparam` get retrieves the value of a parameter.
- `rosparam` list lists all available parameters.

## ROS Launch:

roslaunch is a tool for launching multiple nodes and configuring their parameters in one command.
These are just some of the most commonly used ROS commands and concepts. For more detailed information, refer to the ROS documentation.
