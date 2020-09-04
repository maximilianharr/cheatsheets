# ROS 2 CheatSheet

The [Robot Operating System (ROS)](https://index.ros.org/doc/ros2/) is a set of software libraries and tools for building robot applications. From drivers to state-of-the-art algorithms, and with powerful developer tools, ROS has what you need for your next robotics project. And it’s all open source.

---

## Bagiles
[ROS2 Bag](https://index.ros.org/doc/ros2/Tutorials/Ros2bag/Recording-And-Playing-Back-Data/#ros2bag)  
[ROS2 Github](https://github.com/ros2/rosbag2)  
ros2 bag is a command line tool for recording data published on topics in your system. New in ROS2, the bagfile is stored as sqlite3 database.
```bash
ros2 bag info ${BAG_NAME}
ros2 bag play ${BAG_NAME}
ros2 bag record -o ${BAG_NAME} ${TOPIC1} ${TOPIC2}
```

--- 

## Communication (Topics, Services, Action)
### Actions
[ROS2 Actions](https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Actions/#ros2actions)  
Actions are like services that allow you to execute long running tasks, provide regular feedback, and are cancelable. They consist of three parts: a goal, a result, and feedback. 
```bash 
ros2 action info ${ACTION}
ros2 action list -t
ros2 action send_goal ${ACTION_NAME} ${ACTION_TYPE} ${VALUE}
```
examples:  
```bash
ros2 action send_goal /turtle1/rotate_absolute turtlesim/action/RotateAbsolute "{theta: 1.57}"
```
### Messages / Interface
```bash
ros2 interface show ${MSG}
```
examples:
```bash
ros2 interface show geometry_msgs/msg/Twist
```
### Services
[ROS2 Services](https://index.ros.org/doc/ros2/Tutorials/Services/Understanding-ROS2-Services/#ros2services)  
While topics allow nodes to subscribe to data streams and get continual updates, services only provide data when they are specifically called by a client.
```bash
ros2 service call ${SERVICE} ${SERVICE_TYPE} ${ARGS}
ros2 service find ${SERVICE}
ros2 service list
```
examples:
```bash 
ros2 service call /clear std_srvs/srv/Empty  
ros2 service call /spawn turtlesim/srv/Spawn "{x: 2, y: 2, theta: 0.2, name: ''}"  
```
### Topics
[ROS2 Topics](https://index.ros.org/doc/ros2/Tutorials/Topics/Understanding-ROS2-Topics/#ros2topics)  
```bash
ros2 topic echo ${TOPIC}
ros2 topic hz ${TOPIC}
ros2 topic info ${TOPIC}
ros2 topic list -t
ros2 topic pub ${TOPIC} ${MSG} '${ARGS}'
```
examples:
```bash
ros2 topic pub --rate 1 /turtle1/cmd_vel geometry_msgs/msg/Twist "{linear: {x: 2.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 1.8}}"
```
---

## Graphical RQT
[ROS2 RQT](https://index.ros.org/doc/ros2/Tutorials/Turtlesim/Introducing-Turtlesim/#turtlesim)  
```bash
rqt
```

### RQT console
[ROS2 RQT Console](https://index.ros.org/doc/ros2/Tutorials/Rqt-Console/Using-Rqt-Console/#rqt-console)
rqt_console is a GUI tool used to introspect log messages in ROS 2.
```bash
ros2 run rqt_console rqt_console
```
Only show warn msgs of node
```bash
ros2 run ${PKG} ${NODE} --ros-args --log-level WARN
```

### RQT graph
![RQT Graph](../doc/rqt_graph.png)
<img src="../doc/rqt_graph.png" alt="RQT Graph"/>
```bash
rqt_graph
```

--- 

## Nodes
[ROS2 Nodes](https://index.ros.org/doc/ros2/Tutorials/Understanding-ROS2-Nodes/#ros2nodes)  
```bash
ros2 node info ${NODE}
ros2 node list -t
```

## Parameters
[ROS2 Parameters](https://index.ros.org/doc/ros2/Tutorials/Parameters/Understanding-ROS2-Parameters/#ros2params)  
A parameter is a configuration value of a node. All parameters are dynamically reconfigurable, and built off of ROS 2 services.
```bash
ros2 param dump ${NODE}
ros2 param get ${NODE} ${PARAM}
ros2 param list
ros2 param set ${NODE} ${PARAM} ${VALUE}
```
examples:
```bash
ros2 param get /turtlesim use_sim_time  
```
### Run Node with parameter file (ros2 param dump)
```bash
ros2 run ${PKG} ${EXECUTABLE} --ros-args --params-file ${PARAM_FILE}
```

### Remapping
Remapping allows you to reassign default node properties (e.g. name, topic, service)
```bash
ros2 run ${PKG} ${EXECUTABLE} --ros-args --remap __node:=${NEW_NAME}
```

---

## Launching (Run, Launch)
### Launch
[ROS2 Launchfiles](https://index.ros.org/doc/ros2/Tutorials/Launch-Files/Creating-Launch-Files/#ros2launch)
```bash
ros2 launch ${PKG} ${LAUNCH_FILE}
```
### Run
```bash
ros2 run ${PKG} ${EXECUTABLE}
```

---

## Workspace
[ROS2 Workspace](https://index.ros.org/doc/ros2/Tutorials/Workspace/Creating-A-Workspace/#ros2workspace)
[ROS2 Packages](https://index.ros.org/doc/ros2/Tutorials/Creating-Your-First-ROS2-Package/#createpkg)
### New Workspace
```bash
cd ${WS_ROOT}
colcon build
colcon build --packages-select ${PKG}
```
### New Package
```bash
cd ${WS_ROOT}/src
ros2 pkg create --build-type ament_cmake --node-name ${NODE} ${PKG}
ros2 pkg create --build-type ament_python --node-name ${NODE} ${PKG}
cd ${WS_ROOT}
colcon build
```
### Source Overlay
Caution: Sourcing an overlay in the same terminal where you built, or likewise building where an overlay is sourced, may create complex issues.
```bash
cd ${WS_ROOT}
. install/local_setup.bash
```

### Resolve Dependencies
```bash
rosdep update
rosdep install -i --from-path src --rosdistro ${ROS_DISTRO} -y
```

---

## Tutorials
[ROS2 tutorials](https://index.ros.org/doc/ros2/Tutorials/)

### Nodes
```bash
ros2 run turtlesim turtlesim_node
ros2 run turtlesim turtle_teleop_key
ros2 bag record /turtlesim1/turtle1/cmd_vel
```
### Workspace
```bash
cd /media/docker/workspace/jumbuck/ros2
. install/setup.bash 
ros2 run python_pkg python_node
```
### New Package Initialization
After a new node has been added
```bash
cd ${WS_ROOT}
rosdep update
rosdep install -i --from-path src --rosdistro foxy -y
colcon build --packages-select ${PKG}
. install/setup.bash
```