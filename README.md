# UR5 docker images

It provides Docker images for the [Universal Robots ROS2 Driver](https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver)

## Download a ready-to-use UR5 docker images

### Using GUI

Go to the [Actions](https://github.com/razr/ur5/actions) tab, find the latest [UR5 driver](https://github.com/razr/ur5/actions/workflows/ur5driver.yaml) or [UR5 MoveIt](https://github.com/razr/ur5/actions/workflows/ur5moveit.yaml) build, go inside the latest workflow run, download, and unzip artifacts called ```UR5 driver docker image``` or ```UR5 MoveIt docker image```. Thess artifacts contain archived Docker images.

### Using command line

Go to the [```Developer settings```](https://github.com/settings/tokens) and generate a token to access the repo via Github API. Use this token in conjunction with your Github name to retrieve build artifacts.

```bash
$ token=<my_token>
# retrieve all artifacts
$ curl -i -u <my github name>:$token -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/razr/ur5/actions/artifacts | grep archive_download_url
      "archive_download_url": "https://api.github.com/repos/razr/ur5/actions/artifacts/100185602/zip",

# download the latest one for the UR5 driver
$ curl -u <my github name>:$token -L -H "Accept: application/vnd.github.v3+json"  https://api.github.com/repos/razr/ur5/actions/artifacts/100185602/zip --output ros.galactic.ur5driver.zip

# download the latest one for the UR5 MoveIt
$ curl -u <my github name>:$token -L -H "Accept: application/vnd.github.v3+json"  https://api.github.com/repos/razr/ur5/actions/artifacts/132431312/zip --output ros.galactic.ur5moveit.zip
```

## Deploy

### UR5 driver

```bash
$ unzip ros.galactic.ur5driver.zip
$ docker load -i ros.galactic.ur5driver.tar.gz
```

### MoveIt

```bash
$ unzip ros.galactic.ur5moveit.zip
$ docker load -i ros.galactic.ur5moveit.tar.gz
```

## Launch

### UR5 driver

```bash
# with a faked hardware
$ docker run --net=host ros:galactic.ur5driver bash -c "ros2 launch ur_bringup ur_control.launch.py ur_type:=ur5e robot_ip:=xxx.xxx.xxx.xxx use_fake_hardware:=true launch_rviz:=false"

# ur5
$ docker run --net=host ros:galactic.ur5driver bash -c "ros2 launch ur_bringup ur_control.launch.py ur_type:=ur5e robot_ip:=xxx.xxx.xxx.xxx launch_rviz:=false"
```

```bash
$ docker run --net=host ros:galactic.ur5driver bash -c "ros2 control list_controllers"
force_torque_sensor_broadcaster[ur_controllers/ForceTorqueStateBroadcaster] active
io_and_status_controller[ur_controllers/GPIOController] active
speed_scaling_state_broadcaster[ur_controllers/SpeedScalingStateBroadcaster] active
joint_trajectory_controller[joint_trajectory_controller/JointTrajectoryController] active
joint_state_broadcaster[joint_state_broadcaster/JointStateBroadcaster] active
```

### MoveIt

```bash
docker run -ti -v "/tmp/.X11-unix:/tmp/.X11-unix" -e DISPLAY=$DISPLAY -e QT_GRAPHICSSYSTEM=native ros:galactic.ur5moveit bash -c "ros2 launch ur_bringup ur_moveit.launch.py ur_type:=ur5e robot_ip:=128.224.200.101 use_fake_hardware:=false launch_rviz:=true"
...
[move_group-1] Loading 'move_group/ApplyPlanningSceneService'...
[move_group-1] Loading 'move_group/ClearOctomapService'...
[move_group-1] Loading 'move_group/MoveGroupCartesianPathService'...
[move_group-1] Loading 'move_group/MoveGroupExecuteTrajectoryAction'...
[move_group-1] Loading 'move_group/MoveGroupGetPlanningSceneService'...
[move_group-1] Loading 'move_group/MoveGroupKinematicsService'...
[move_group-1] Loading 'move_group/MoveGroupMoveAction'...
[move_group-1] Loading 'move_group/MoveGroupPlanService'...
[move_group-1] Loading 'move_group/MoveGroupQueryPlannersService'...
[move_group-1] Loading 'move_group/MoveGroupStateValidationService'...
[move_group-1]
[move_group-1] You can start planning now!
[move_group-1]
```
