---
title: 深度学习-如何安装pytorch/yolov5
date: 2023-10-19
tags:
- pytorch
- tensorflow
- yolov5
- 深度学习
categories:
- 深度学习
aside: false
---

## 1.创建虚拟环境，pytorch和tensorflow安装
每个项目所需库不一样，比如使用pytorch框架：
->打开 Anaconda prompt
->更换源，不然很慢
->创建环境`conda create --name pytorch python=3.7`
->进入环境`conda activate pytorch`
->下载pytorch：`conda install pytorch torchvision cpuonly`（删除-c pytorch，这样就可以使用镜像了,这里是下载cpu版本的，gpu根据自己配置修改）。tensorflow: `conda install tensorflow`
->完成了，可以去pycharm创建新项目，然后打开python interpreter，选择previously，点击add interpreter，再去设置conda路径（看需求选择，我选择conda），然后interpreter选择创建的虚拟环境pytorch\tensorflow，然后ok，创建，最后就可以使用了
->如果要安装其他库，可以在 Anaconda prompt激活pytorch环境，然后conda install 包名。此时不用再添加镜像，因为前面已经修改了

## 2.anaconda创建新的python环境时，报错'An unexpected error has occurred. Conda has prepared the above report'
梯子没有关闭，关闭vpn即可，很多报错都与梯子有关，必须先关闭

## 3.在虚拟环境中安装第三方库
找到存放路径: ~/anaconda3/envs/pytorch/lib/python3.9/site-packages
比如安装opencv: 
``` bash
$ pip install --target=~/anaconda3/envs/pytorch/lib/python3.9/site-packages opencv-python -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

## 4.ERROR: No matching distribution found for ultralytics>=8.0.100
`pip install ultralytics --default-timeout=100 -i https://pypi.tuna.tsinghua.edu.cn/simple`


## 5.yolov5实战
->安装ananconda：下载 Anaconda3-2022.05-Linux-aarch64.sh；`bash Anaconda3-2022.05-Linux-aarch64.sh` 有时-u；加载新的PATH环境变量source ~/.bashrc ；
->创建yolo环境：`conda create -n yolo python=3.8`；激活`conda activate yolo`
->安装yolov5:`git clone https://github.com/ultralytics/yolov5.git`；添加依赖：`pip3 install -U -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple`
->测试：`conda activate yolo`->`python detect.py --source data/images/zidane.jpg`
->测试电脑摄像头：找到detect.py文件，修改以下代码中的source：改成0则为调用电脑自带的摄像头；进入我们创建的yolo环境，输入python detect.py即可运行。
->将YOLOV5代码加入到ORB-SLAM2中

yolov5测试视频
->在data文件夹下创建video文件夹
->修改detect.py中的source：将/data/images修改为data/video

yolov5训练自己的数据：（准备好自己的数据集->借用权重文件训练自己的数据集->测试自己的验证集
->在yolov5同目录下创建数据集datasets，继续创建文件夹face做人脸识别
->在face下创建images数据集存放图片，可以写代码划分tarin和val，也可以自己随机划分
->在face下创建labels数据集存放标签，可以使用labeling去画框然后保存为txt文件
->在yolov5/data下创建yaml文件，可以参考coco128.yaml
->开始训练，进入pytorch环境，通过命令修改train.py的参数：`python train.py --weights yolov5s.pt --data data/face.yaml --workers 1 --batch-size 8` 
->训练完成后，将runs/exp/weights下的模型（best.pt）复制在yolov5文件夹下
->开始测试`python detect.py --weights best.pt --source ../datasets/face/images/val `
