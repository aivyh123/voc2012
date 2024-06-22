# README

## 概述
使用YOLOv5预训练模型及深度学习框架，对Pascal VOC2012数据集实现目标检测。

## 下载VOC2012数据集
```
http://host.robots.ox.ac.uk/pascal/VOC/voc2012/
```

只解压`Annotations`和`JPEGImages`这两个文件夹。  
`Annotations`存放标注用的.xml文件，一个.xml对应一张图片。  
`JPEGImages`存放原始图片。  
`ImageSets`存放配置文件，指示训练和测试中使用哪些图片，需自己编写合适的，故不解压原有的。

解压至项目根目录下的`my_VOC2012`目录中，注意`JPEGImages`目录需改名为`images`。

## GitHub拉取YOLOv5源码
```
https://github.com/ultralytics/yolov5.git
```

## 项目配置conda环境
新建conda环境
```bash
conda create -n YOLOv5_for_VOC2012 python=3.8
```
启动
```bash
conda activate YOLOv5_for_VOC2012
```
先配置GPU训练环境（根据本机的CUDA配置）
```bash
pip install torch==1.13.0+cu117 torchvision==0.14.0+cu117 torchaudio==0.13.0 --extra-index-url https://download.pytorch.org/whl/cu117
```
再根据YOLOv5项目自带的`requirements.txt`下载依赖包
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt
```

## 数据集划分
编写`my_split.py`用于划分训练、验证、测试集，代码测试时可以只选一小部分图像。  
输出的.txt文件记录了数据集的划分情况，存放于`./my_VOC2012/ImageSets/Main`下。

## 标注文件格式转换
编写`my_xml2label.py`用于将VOC提供的.xml格式的标注转为适合YOLOv5框架使用的标注格式。  
其针对.txt文件中指示的每一张图像，将其对应的.xml标注转换并保存在`./my_VOC2012/labels`目录下。

**注意`my_xml2label.py`的第70行包含绝对路径，需修改为本机的相应路径。**

## 创建数据集配置文件
根据VOC2012数据集的情况，在`./data`目录下创建`myvoc.yaml`配置文件。  
配置文件中描述了分类的类别数量、各类别名称，以及记录了数据集划分情况的.txt文件的路径（需使用`my_xml2label.py`生成的位于`my_VOC2012`目录下的.txt文件）。

## 下载预训练模型
从
```
https://github.com/ultralytics/yolov5/releases/tag/v5.0
```
下载，这里选用了`yolov5m.pt`，存放在`./weights`目录下。

## 修改模型配置文件
根据选用的模型创建或修改相应的模型配置文件，模型配置文件位于`./models`目录下。  
由于使用的`yolov5m.pt`权重文件已有相应的`yolov5m.yaml`，故仅需修改。  
仅需修改Parameters中的nc一项，改为20与模型的分类数量相同。

## 开始训练
运行项目根目录下的`train.py`启动训练。
```bash
python train.py --batch 4 --epoch 300 --data ./data/myvoc.yaml --cfg ./models/yolov5m.yaml --weights ./weights/yolov5m.pt
```
需要训练的epoch数量以及各文件的路径根据实际情况填写。
