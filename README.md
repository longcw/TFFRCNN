# TFFRCNN

This is an experimental Tensorflow implementation of Faster RCNN, mainly based on the work of [smallcorgi](https://github.com/smallcorgi/Faster-RCNN_TF) and [rbgirshick](https://github.com/rbgirshick/py-faster-rcnn). I have re-organized the libraries under `lib` path, making each of python modules independent to each other, so you can understand, re-write the code easily.

For details about R-CNN please refer to the paper [Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks](http://arxiv.org/pdf/1506.01497v3.pdf) by Shaoqing Ren, Kaiming He, Ross Girshick, Jian Sun.

### Acknowledgments: 

1. [py-faster-rcnn](https://github.com/rbgirshick/py-faster-rcnn)

2. [Faster-RCNN_TF](https://github.com/smallcorgi/Faster-RCNN_TF)

3. [ROI pooling](https://github.com/zplizzi/tensorflow-fast-rcnn)

### Requirements: software

1. Requirements for Tensorflow (see: [Tensorflow](https://www.tensorflow.org/))

2. Python packages you might not have: `cython`, `python-opencv`, `easydict` (recommend to install: [Anaconda](https://www.continuum.io/downloads))

### Requirements: hardware

1. For training the end-to-end version of Faster R-CNN with VGG16, 3G of GPU memory is sufficient (using CUDNN)

### Installation (sufficient for the demo)

1. Clone the Faster R-CNN repository
  ```Shell
  git clone https://github.com/CharlesShang/TFFRCNN.git
  ```

2. Build the Cython modules
    ```Shell
    cd TFFRCNN/lib
    make # compile cython and roi_pooling_op, you may need to modify make.sh for your platform
    ```

### Demo

*After successfully completing [basic installation](#installation-sufficient-for-the-demo)*, you'll be ready to run the demo.

To run the demo
```Shell
cd $TFFRCNN
python ./faster_rcnn/demo.py --model model_path
```
The demo performs detection using a VGG16 network trained for detection on PASCAL VOC 2007.

### Download list

1. [VGG16 trained on ImageNet](https://drive.google.com/open?id=0ByuDEGFYmWsbNVF5eExySUtMZmM)

2. [VGG16 - FasterRCN](https://drive.google.com/open?id=0ByuDEGFYmWsbZ0EzeUlHcGFIVWM).

### Training on Pascal VOC 2007

1. Download the training, validation, test data and VOCdevkit

    ```Shell
    wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtrainval_06-Nov-2007.tar
    wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCtest_06-Nov-2007.tar
    wget http://host.robots.ox.ac.uk/pascal/VOC/voc2007/VOCdevkit_08-Jun-2007.tar
    ```

2. Extract all of these tars into one directory named `VOCdevkit`

    ```Shell
    tar xvf VOCtrainval_06-Nov-2007.tar
    tar xvf VOCtest_06-Nov-2007.tar
    tar xvf VOCdevkit_08-Jun-2007.tar
    ```

3. It should have this basic structure

    ```Shell
    $VOCdevkit/                           # development kit
    $VOCdevkit/VOCcode/                   # VOC utility code
    $VOCdevkit/VOC2007                    # image sets, annotations, etc.
    # ... and several other directories ...
    ```

4. Create symlinks for the PASCAL VOC dataset

    ```Shell
    cd $TFFRCNN/data
    ln -s $VOCdevkit VOCdevkit2007
    ```

5. Download pre-trained model [VGG16](https://drive.google.com/open?id=0ByuDEGFYmWsbNVF5eExySUtMZmM) and put it in the path `./data/pretrain_model/VGG_imagenet.npy`

6. Run training scripts 

    ```Shell
    cd $TFFRCNN
    python ./faster_rcnn/train_net.py --gpu 0 --weights ./data/pretrain_model/VGG_imagenet.npy --imdb voc_2007_trainval --iters 70000 --cfg  ./experiments/cfgs/faster_rcnn_end2end.yml --network VGGnet_train --set EXP_DIR exp_dir
    ```
