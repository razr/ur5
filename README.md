# UR5 docker images

It provides Docker images for the [Universal Robots ROS2 Driver](https://github.com/UniversalRobots/Universal_Robots_ROS2_Driver)

## Download a ready-to-use UR5 docker images

### Using GUI

Go to the ```Action``` tab, find the latest ```ROS``` build, go inside the latest workflow run, download, and unzip artifacts called ```UR5 driver docker image```. This archive contains archived Docker image.

### Using command line

Go to the [```Developer settings```](https://github.com/settings/tokens) and generate a token to access the repo via Github API. Use this token in conjunction with your Github name to retrieve build artifacts.

```bash
$ token=<my_token>
# retrieve all artifacts
$ curl -i -u <my github name>:$token -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/razr/ur5/actions/artifacts | grep archive_download_url
      "archive_download_url": "https://api.github.com/repos/razr/ur5/actions/artifacts/100185602/zip",

# download the latest one
$ curl -u <my github name>:$token -L -H "Accept: application/vnd.github.v3+json"  https://api.github.com/repos/razr/ur5/actions/artifacts/100185602/zip --output ros.galactic.ur5driver.zip
```

## Deploy

```bash
$ unzip ros.galactic.ur5driver.zip
$ docker load -i ros.galactic.ur5driver.tar.gz
```

## Use

```bash
$ docker run --net=host -p63352:63352 ros:galactic.ur5driver ros2 launch ur_bringup ur_control.launch.py ur_type:=ur5e robot_ip:=xxx.xxx.xxx.xxx use_fake_hardware:=true launch_rviz:=false
```
