## TensorRT_TX2_20FPS

# Objective 
to test one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN) on TX2 via TensorRT, aiming for 20 fps of vehicle/pedestrian detection performance.

# Motivation
1, Running SSD, YOLO2, Fast R-CNN etc. algos on Jetson TX2, with CUdnn/Caffe, most commonly achieves only 8-9 fps, according to informaton from NVDIA developer forum. 

2, NVIDI officially claims the new TensorRT 2 (under Jetpack 3.1) can accelerate the detection by 2x

Therefore the purpose for this project is to achieve around 20 fps on TX2 through tensorRT, using one of the real-time object detection algos (SSD, YOLO2 or Fast R-CNN).  


# Related resources
[1.a] https://myurasov.github.io/2016/11/27/ssd-tx1.html  
steps for SSD install on TX1; claims SSD model runs at ~8.5 FPS on TX1 without using TensorRT. 

[1.b] https://devtalk.nvidia.com/default/topic/1021356/caffe-ssd-on-tx2-cudnn_status_internal_error/
NVIDIA steps for SSD install on TX2.

[2] TensorRT folder on TX2, after successful Jetpack 3.1 installation   
There is a sampleFastRCNN example under sample folder, which reads individual images as input

[3] http://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#setinput  
Official NVIDIA documentation on TensorRT, including the sampleFasterRCNN module.
Basically, this existing sample module takes ppm file as input, meaning in case a video file is input instead, some image processing library is needed to extract frames and convert them into ppm format.

https://devtalk.nvidia.com/default/topic/1023905/jetson-tx2/no-result-when-using-tensorrt-sample-fasterrcnn-with-other-images/3
Q&A of loading jpg file for sampleFasterRCNN

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

# Googlenet sample on TX2
(a) Cited from [5]    
~/tegra_multimedia_api/samples/backend$ ./backend 1 ../../data/Video/sample_outdoor_car_1080p_10fps.h264 H264 --trt-deployfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.prototxt --trt-modelfile ../../data/Model/GoogleNet_one_class/GoogleNet_modified_oneClass_halfHD.caffemodel --trt-forcefp32 0 --trt-proc-interval 1 -fps 10

Test result: the above commands runs pretty smooth on TX2, although detection result not the best. 

# Technics for Jetson TX2
(1) Two ways to change TX2's running clocks/mode    

1.1 maximize clock speed     
  $ sudo /home/ubuntu/jetson_clocks.sh to maximize the speed    
show all the options by running,     
  $ sudo /home/ubuntu/jetson_clocks.sh --help   
to save default clock speed into a file, run    
  $ sudo /home/ubuntu/jetson_clocks.sh --store <file name>    
to restore the default clock speed, run    
  $ sudo /home/ubuntu/jetson_clocks.sh --restore <file name> 

refer to the link below for details,    
    https://devtalk.nvidia.com/default/topic/999915/how-do-you-switch-between-max-q-and-max-p/

1.2 switch running mode
  by running    
    $ sudo nvpmodel -m [mode]   
  query which mode is currently being used by running   
    $ sudo nvpmodel -q --verbose    
  
  refer to the link below for details,    
    http://www.jetsonhacks.com/2017/03/25/nvpmodel-nvidia-jetson-tx2-development-kit/

(2) AF01 video convert
from avi -> H264    
  $ avconv -i af01_779.avi -c:v libx264 -c:a copy -s 1920x1080 -r 25 af01_779HD.h264    
so that the sample "back" program in TX2 can recognize.  

(3) gstreamer playback on TX2
The following pipeline settings can playback .h264 AF01 video on TX2, which is converted from .avi format   
    $ gst-launch-1.0 filesrc location=af01_779.h264 ! h264parse ! omxh264dec ! nveglglessink -e

(4) a few examples about grabbing frame from video file using Gstreamer   
4.1   using pad/probe mechanism,    
    gst_pad_add_probe()     
https://gstreamer.freedesktop.org/documentation/application-development/advanced/pipeline-manipulation.html#section-spoof-appsink

4.2   using buffer/sample+buffer      
4.2-a for gst-0.xx version,     
    gst_app_sink_pull_buffer()   
https://coherentmusings.wordpress.com/2014/07/09/extracting-frame-from-a-gstreamer-pipeline-and-displaying-it-with-opencv/

4.2-b for gst-1.0 version       
    gst_sample_unref(sample);   
    sample = gst_app_sink_pull_sample(GST_APP_SINK(sink));    
https://github.com/opencv/opencv/blob/master/modules/videoio/src/cap_gstreamer.cpp

(5) Error while compiling customized sampleFasterRCNN.cpp program   
undefined reference to `cudaRGBToRGBA`    
https://stackoverflow.com/questions/35886052/cuda-c-linking-error-undefined-reference-to    
Suggested to add -lcuda -lcudart in cmake file

Example codes of Gstreamer video inputs 
https://cgit.freedesktop.org/gstreamer/gst-plugins-base/tree/tests/examples/snapshot/snapshot.c   

Gstreamer official site, handy elements   
https://gstreamer.freedesktop.org/documentation/tutorials/basic/handy-elements.html   
including a convenient launch cmd, $ gst-launch-1.0 filesrc location=f:\\media\\sintel\\sintel_trailer-480p.webm ! decodebin ! autovideosink   

and the following page gives an example casting from autovideosink to appsink,    
http://gstreamer-devel.966125.n4.nabble.com/gst-element-seek-simple-returns-false-using-GST-FORMAT-BYTES-format-td4679798.html

example 
https://stackoverflow.com/questions/27419113/playing-a-raw-video-using-gst-launch
including such an example pipeline, video framerate
$ gst-launch-1.0 filesrc location=/home/user/Videos/out.yuv ! videoparse width=1920 height=816 framerate=24/1 format=2 ! autovideoconvert ! autovideosink
