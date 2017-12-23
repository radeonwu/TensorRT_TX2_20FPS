The FasterRCNN sample uses the dataset from here:
https://people.eecs.berkeley.edu/~rbg/faster-rcnn-data/faster_rcnn_models.tgz

The dataset needs to be placed into the data/faster-rcnn directory.

The commands to do this on linux are as follows:

cd <TensorRT directory>
cd data
wget https://dl.dropboxusercontent.com/s/o6ii098bu51d139/faster_rcnn_models.tgz?dl=0 -O faster-rcnn.tgz
tar zxvf faster-rcnn.tgz -C faster-rcnn --strip-components=1 --exclude=ZF_*
