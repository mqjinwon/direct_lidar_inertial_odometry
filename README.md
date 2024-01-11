# Direct LiDAR-Inertial Odometry: Lightweight LIO with Continuous-Time Motion Correction

This repository is for an easy-to-use library utilizing Docker.
We use the following environment.

-   docker
    -   Ubuntu 20.04
    -   ROS noetic

You can find the original instruction file [here](/INSTRUCTION.md)

We have modified `README.md` to include instructions for installing Docker images and containers.

## 1. Build docker image with Dockerfile

Using the image file, you can create a container.

```bash
cd docker
docker build -t dlio-noetic:latest .
```

## 2. Make container

Using image file, you can make container.

```bash
docker run --privileged -it \
           --volume=${DLIO_root}:/root/workspace/src \
           --volume=/tmp/.X11-unix:/tmp/.X11-unix:rw \
           --net=host \
           --ipc=host \
           --name=${docker container name} \
           --env="DISPLAY=$DISPLAY" \
           ${docker image} /bin/bash
```

You need to replace `${DLIO_root}`, `${docker container name}`, and `${docker image}` with your specific values. An example is provided below.

Example

```bash
docker run --privileged -it \
           --volume=/home/jin/Documents/direct_lidar_inertial_odometry:/root/workspace/src \
           --volume=/tmp/.X11-unix:/tmp/.X11-unix:rw \
           --net=host \
           --ipc=host \
           --name=dlio-noetic \
           --env="DISPLAY=$DISPLAY" \
           dlio-noetic:latest /bin/bash
```

Output

```bash
==============Direct Lidar Odometry Noetic Docker Env Ready================
root@jin-940XFG:~/workspace#
```

## 3. Build project

> ❗️ All processes are operated within the container terminal!

```bash
source /ros_entrypoint.sh
source /opt/ros/noetic/setup.bash
catkin init
catkin build
```

## 4. Execution

> ❗️ All processes are operated within the container terminal!

```bash
roslaunch direct_lidar_inertial_odometry dlio.launch \
  rviz:={true, false} \
  pointcloud_topic:=/robot/lidar \
  imu_topic:=/robot/imu
```

### Services

You can save .pcd and trajectory files.

Save .pcd file

```bash
rosservice call /robot/dlio_map/save_pcd LEAF_SIZE SAVE_PATH
```

## Test Data

For your convenience, we provide sample test data [here](https://ucla.box.com/shared/static/ziojd3auzp0zzcgwb1ucau9anh69xwv9.bag) (9 minutes, ~4.2GB).

```bash
roslaunch direct_lidar_inertial_odometry dlio.launch
```

```bash
rosbag play dlo_test.bag
```

### Result

## Acknowledge

We thank the authors of the [**direct lidar inertial odometry**](https://github.com/vectr-ucla/direct_lidar_inertial_odometry) open-source packages.

## Maintainer

-   mqjinwon@gmail.com
