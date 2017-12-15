## TensorRT_TX2_20FPS

# Objective 
to test one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN) on TX2 via TensorRT, aiming for 20 fps of vehicle/pedestrian detection performance.

# Motivation
1, Running SSD, YOLO2, Fast R-CNN etc. algos on Jetson TX2, with CUdnn/Caffe, most commonly achieves only 8-9 fps, according to informaton from NVDIA developer forum. 

2, NVIDI officially claims the new TensorRT 2 (under Jetpack 3.1) can accelerate the detection by 2x

Therefore the purpose for this project to trying to achieve around 20 fps on TX2 through tensorRT, using one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN).  
