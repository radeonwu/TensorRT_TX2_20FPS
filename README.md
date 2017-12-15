## TensorRT_TX2_20FPS

# Objective 
to test one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN) on TX2 via TensorRT, aiming for 20 fps of vehicle/pedestrian detection performance.

# Motivation
1, Running SSD, YOLO2, Fast R-CNN etc. algos on Jetson TX2, with CUdnn/Caffe, most commonly achieves only 8-9 fps, according to informaton from NVDIA developer forum. 

2, NVIDI officially claims the new TensorRT 2 (under Jetpack 3.1) can accelerate the detection by 2x

Therefore the purpose for this project to trying to achieve around 20 fps on TX2 through tensorRT, using one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN).  


# Related resources
[1] https://myurasov.github.io/2016/11/27/ssd-tx1.html
Claims SSD model runs at ~8.5 FPS on TX1 without using TensorRT. 

[2] TensorRT folder on TX2, after successful Jetpack 3.1 installation
There is a sampleFastRCNN example under sample folder, which reads individual images as input

[3] http://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#setinput
Official NVIDIA documentation on TensorRT, including the sampleFasterRCNN module.
Basically, this existing sample module takes ppm file as input, meaning in case a video file is input instead, some image processing library is needed to extract frames and convert them into ppm format.
