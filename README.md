## TensorRT_TX2_20FPS

# Objective 
to test one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN) on TX2 via TensorRT, aiming for 20 fps of vehicle/pedestrian detection performance.

# Motivation
1, Running SSD, YOLO2, Fast R-CNN etc. algos on Jetson TX2, with CUdnn/Caffe, most commonly achieves only 8-9 fps, according to informaton from NVDIA developer forum. 

2, NVIDI officially claims the new TensorRT 2 (under Jetpack 3.1) can accelerate the detection by 2x

Therefore the purpose for this project is to achieve around 20 fps on TX2 through tensorRT, using one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN).  


# Related resources
[1] https://myurasov.github.io/2016/11/27/ssd-tx1.html  
Claims SSD model runs at ~8.5 FPS on TX1 without using TensorRT. 

[2] TensorRT folder on TX2, after successful Jetpack 3.1 installation 
There is a sampleFastRCNN example under sample folder, which reads individual images as input

[3] http://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#setinput  
Official NVIDIA documentation on TensorRT, including the sampleFasterRCNN module.
Basically, this existing sample module takes ppm file as input, meaning in case a video file is input instead, some image processing library is needed to extract frames and convert them into ppm format.

https://developer.ridgerun.com/wiki/index.php?title=Gstreamer_pipelines_for_Tegra_X2#nvcamerasrc  
3-rd party explanation of gstreamer piplines on Jetson TX2;

http://developer2.download.nvidia.com/embedded/L4T/r24_Release_v2.0/Docs/L4T_Tegra_X1_Multimedia_User_Guide_Release_24.2.pdf?N2WLlIXCd3pm_shGSAXKkLxPRe5fO1bAjiOOTKyFFqe5Xecan7C0F7O0oWhUvcxrD828mvemWUBBYu5wb5gENNE_ZfP7z58J_GC3raZWAilfQ_JKnjXQNlAjbK5Gts4GqkT4vtnGF1uCIXGJbFTIJ2Bz-_GDdNdDaUvMgyX9S1tqWp-8rKYw-SJKb5Fh-9MsjWEnwUk
Nvidia Manual of gstreamer pipeline on Jetson

[4] https://github.com/dusty-nv/jetson-inference/blob/master/imagenet-console/imagenet-console.cpp#L68  
Jetpack example code, load .jpg file into CUDA memory

[5]
https://devtalk.nvidia.com/default/topic/1024723/detectnet-quot-video-file-quot-rather-than-a-jpg-or-camera-how-do-i-test-a-file-/  
https://github.com/dusty-nv/jetson-inference/issues/104   
One user posted a solution using using gstreamer to take a video file as input

[6]
https://yq.aliyun.com/articles/160369 
中文社区分享，含安装完JETPACK后读取本地视频文件后运行demo测试程序

[7]
http://download.csdn.net/download/zys615/9956504  
中文社区TX2开发资料共享

