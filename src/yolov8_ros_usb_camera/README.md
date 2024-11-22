# Install and Run YOLOv8 Object Detection using a USB Camera with ROS Noetic on Ubuntu 20.04

This package provides a ROS wrapper for [PyTorch-YOLOv8](https://github.com/ultralytics/ultralytics) based on PyTorch-YOLOv8. 

Install and Run YOLOv8 Object Detection using a USB Camera with ROS Noetic on Ubuntu 20.04

![image.png](/yolov8_ros/media/image.png)

# Develop Environment：
- Ubuntu 20.04
- ROS Noetic
- Python >= 3.7.0
- PyTorch >= 1.7

# Prerequisites:

Pip install the ultralytics package including all [requirements](https://github.com/ultralytics/ultralytics/blob/main/requirements.txt) in a [**Python>=3.7**](https://www.python.org/) environment with [**PyTorch>=1.7**](https://pytorch.org/get-started/locally/).

```
pip install ultralytics
pip install rospkg
```

# Install YOLOv8 on ROS:

```
cd <catkin_ws>/src

git clone https://github.com/narsimlukemsaram/yolov8_ros.git

cd ..

catkin_make

```

# Setup and make Ubuntu 20.04 to identify the camera:

First, plug in the camera to the USB port.

Open a terminal, and list all cameras plugged in and detected by the system:

```
ls /dev/video*
```

Plug and unplug the Logitech C920 HD Pro camera and identify the correct /dev/video*.

In my case camera was mounted on path /dev/video2.

# Open the usb_cam_stream_publisher.launch file and change your identified camera:

Open the usb_cam_stream_publisher.launch file and change the /dev/video2 and autofocus control, as follows:

```
<!--
Example of run:
roslaunch usb_cam_stream_publisher.launch video_device:=/dev/video2 image_width:=640 image_height:=480
-->

<launch>
<arg name="video_device" default="/dev/video2" /> <!-- video2 for Logitech C920 HD Pro Camera  -->
<arg name="image_width" default="640" />
<arg name="image_height" default="480" />

<node name="usb_cam" pkg="usb_cam" type="usb_cam_node" output="screen" >
	<param name="video_device" value="$(arg video_device)" />
	<param name="image_width" value="$(arg image_width)" />
	<param name="image_height" value="$(arg image_height)"/>
	<param name="pixel_format" value="mjpeg" />
	<param name="camera_frame_id" value="usb_cam" />
	<param name="io_method" value="mmap"/>
  <param name="focus_auto" value="0" />  <!-- Disable autofocus -->
</node>
</launch>
```

Open a terminal, run:

```
roslaunch usb_cam_stream_publisher.launch video_device:=/dev/video2 image_width:=640 image_height:=480
```

# Run YOLOv8 on ROS:

Launch yolo_v8.launch file, all you should have to do is change the image topic you would like to subscribe to:

```
roslaunch yolov8_ros yolo_v8.launch
```
