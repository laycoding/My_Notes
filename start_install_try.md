1. GitHub
https://github.com/playerkk/face-py-faster-rcnn


2. Requirements
Requirements for caffe and pycaffe (see: Caffe installation instructions)

GTX 950M + ubuntu+cuda8.0+cudnn5.0

3. Installation
#Clone the face Faster R-CNN repository 
Git clone –recursive https://github.com/playerkk/face-py-faster-rcnn.git
#Build the Cython modules 
cd $FRCN_ROOT/lib 
make
#Build Caffe and pycaffe 
cd $FRCN_ROOT/caffe-fast-rcnn
#cd python find requirement.txt
for req in $(cat requirements.txt); do pip install $req; done
cp Makefile.config.example Makefile.config
#(自定义：USE_CUDNN := 1 USE_OPENCV := 1 WITH_PYTHON_LAYER := 1 USE_PKG_CONFIG := 1)
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial /usr/local/share/OpenCV/3rdparty/lib/
pip install  easydict
# cudnn4.0->5.0
用最新caffe源码的以下文件替换掉faster rcnn 的对应文件 
include/caffe/layers/cudnn_relu_layer.hpp, src/caffe/layers/cudnn_relu_layer.cpp, src/caffe/layers/cudnn_relu_layer.cu
include/caffe/layers/cudnn_sigmoid_layer.hpp, src/caffe/layers/cudnn_sigmoid_layer.cpp, src/caffe/layers/cudnn_sigmoid_layer.cu
include/caffe/layers/cudnn_tanh_layer.hpp, src/caffe/layers/cudnn_tanh_layer.cpp, src/caffe/layers/cudnn_tanh_layer.cu
用caffe源码中的这个文件替换掉faster rcnn 对应文件 
include/caffe/util/cudnn.hpp
将 faster rcnn 中的 src/caffe/layers/cudnn_conv_layer.cu 文件中的所有 
cudnnConvolutionBackwardData_v3 函数名替换为 cudnnConvolutionBackwardData
cudnnConvolutionBackwardFilter_v3函数名替换为 cudnnConvolutionBackwardFilter

make -j8 && make pycaffe

下载预先训练好的VGG模型 
A pre-trained face detection model trained on the WIDER training set is available here. 
http://supermoe.cs.umass.edu/%7Ehzjiang/data/vgg16_faster_rcnn_iter_80000.caffemodel 
放置目录： 
$FRCN_ROOT/output/faster_rcnn_end2end/train/vgg16_faster_rcnn_iter_80000.caffemodel
1. 下载测试数据 
http://vis-www.cs.umass.edu/fddb/index.html 下载FDDB数据库放入$FRCN_ROOT/data目录： 
包括: 
FDDB 
FDDB/FDDB-folds 
FDDB/originalPics
