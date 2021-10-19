---
draft: false
slides: 
url_pdf: ""
title: Machine Learning project
subtitle: Face mask detection system 
date: 2021-09-19T15:16:37.591Z
summary: Face mask detection system based on yolo5
authors:
  - Le Zhang
url_video: ""
featured: false
external_link: 
url_slides: ""
tags: 
  - Other
categories:
  - Other
links: []
image:
  caption: one-stage object detection
  focal_point: Center
  filename: "https://sm.ms/image/9ltMYkcZ1mpA5SE"
url_code: "https://github.com/Magiccircuit/face-mask-detection"
---
[TOC]

## Introduction

The topic of this project is object detection. The main task is to use the model to identify the target of pictures and videos and locate and draw a frame box to display with a certain probability. On this basis, in accordance with the teacher’s request for improvement, I usethe Opencv library which calls the camera's API, and cuts the camera's real-time data stream image obtained by the camera frame by frame, achieving communication Through the real-time mask detection function of the front camera, a GUI was designed for the entire project for interaction and operation.

The object detection is based on YOLOV5.  “YOLO”, refering to “*You Only Look Once*”, is a family of **object detection** models introduced by Joseph Redmon with a 2016 publication “[You Only Look Once: Unified, Real-Time Object Detection](https://arxiv.org/pdf/1506.02640.pdf)”.



## Datasets

We use a datasets from Kaggle named  [Face Mask Detection](https://www.kaggle.com/andrewmvd/face-mask-detection) which contains 853 images and corresponding annotation files indicating whether a person is *wearing a mask correctly*, *incorrectly* or *not wearing it*. Here are some samples:

<center>
<figure>
<img src="https://i.loli.net/2021/10/19/LUtMi3bvaFQVSjh.jpg" alt="phplpE73q_jpg.rf.bd81cab9f8ff2674ce2e58278f7d37fa" width= "30%" align=left /><img src="https://i.loli.net/2021/10/19/h8KtiZLSAcnMNwg.jpg" alt="1224331650_g_400-w_g_jpg.rf.b816f49e2d84044fc997a8cbd55c347d"  width= "30%"  align=center /><img src="https://i.loli.net/2021/10/19/pzC7YTbUcgDmVNS.jpg" alt="shutterstock_1627199179_jpg.rf.8432d033a37b3d142ec4ffcede508c7d" width= "30%" align=right />
<figure>
<center>




Besides these 853 images, I also collected extra images using [Labelme](https://github.com/wkentaro/labelme) .	


The format of label in [Face Mask Detection](https://www.kaggle.com/andrewmvd/face-mask-detection) is `.xml`, the label is the four absolute coordinates  rectangular box in the image. However, since yolo model is a one-stage model, and its label must follow 'yolo' format, where the label is normalization of the x and y coordinates of the box's center and the normalization height and width of the box. All values must be in the [0-1] interval. To transform the format, I write a script as following:

```python
def convert(size, box):
    x_center = (box[0] + box[1]) / 2.0
    y_center = (box[2] + box[3]) / 2.0
    x = x_center / size[0]
    y = y_center / size[1]
    w = (box[1] - box[0]) / size[0]
    h = (box[3] - box[2]) / size[1]
    return (x, y, w, h)
```

The good news, this step is made really simple thanks to [Roboflow](https://roboflow.com/). [Roboflow](https://roboflow.com/) enables to easily *change between annotation formats*.

## Training

Before train the model,  we should change the settings of the yolov5 by creating a  `mask.yaml` in `\data` and specify the path of train set and validation set and the amount of labels as well as the label name which will be used in the target box

```python
# train and val data as 1) directory: path/images/, 2) file: path/images.txt, or 3) list: [path1/images/, path2/images/]
train: /remote-home/my/pycharm_files/mask_ml/images/train2017  # 128 images
val: /remote-home/my/pycharm_files/mask_ml/images/val2017   # 128 images

# number of classes
nc: 2

# class names
names: ['with_mask','without_mask']
```



Then we use the 

[code]: https://github.com/Magiccircuit/face-mask-detection

to train the model. 

To train the model we have to run `train.py`, which takes the following arguments:

- **img:** input image size
- **batch:** batch size
- **epochs:** number of epochs
- **data:** path to the `data.yml` file
- **cfg:** model to choose among the preexisting in 📁**models**
- **weights:** initial weights path, defaults to `yolov5s.pt` 
- **name:** renames output folder
- **device**: Wheter to train on *cpu* or *gpu.*

Finally run the code to train the model

```
python ~/github/yolov5/train.py --img 416 --batch 16 --epochs 150 --data data.yaml --cfg yolov5s.yaml --weights ‘’ --name robo4_epoch150_s --adam
```

Here are some visualized metrics :

![image-20211019211159587](https://i.loli.net/2021/10/19/ql16QvRzT3Zhsia.png)

<center>
<figure>
 <center>
<figure><img src="https://i.loli.net/2021/10/19/qot1BKZ4k6RCjm9.png" alt="image-20211019211324124" width= "40%"  /><img src="https://i.loli.net/2021/10/19/hDmMgYpx41Okwf3.png" alt="image-20211019211240876" width= "40%"  />
    <figure>
         <center>


<figure>
        <center>




The output of the model is the probability and the coordinates of the box, to visualize the result, use following script:

```python
from facenet_pytorch import MTCNN, extract_face
import matplotlib.image as mpl
import matplotlib.pyplot as plt
import os
import cv2

path = r'.\coco_images'
labels_path = r'.\coco_labels'
ims = os.listdir(path)
ignored_ones = []

for img in ims:

    im_path = path+'\\'+img
    im = mpl.imread(im_path)

    try:
        boxes, probs, points = mtcnn.detect(im, landmarks=True)
    except RuntimeError as e:
        print(f"Couldn't detect {im_path}")
        continue
        
    if boxes is not None:
        for box, prob in zip(boxes, probs):
            startX, startY, endX, endY = box.astype(int)

            color = (0,255,0) 
            cv2.putText(im, 
                        f'{prob:.1%}', 
                        (startX, startY - 10), 
                        fontFace=cv2.FONT_HERSHEY_SIMPLEX,
                        fontScale=.5, 
                        color=color,
                        thickness=2)
            cv2.rectangle(im, (startX, startY), (endX, endY), color, 2) 

        w= int(im.shape[0])
        h= int(im.shape[1])

        label = img.rstrip('.jpg')
        with open(rf'{labels_path}\{label}.txt', 'w') as f:
            for item in boxes:
                b = (startX, endX, startY, endY)
                # convert_to_darknet at 
                # https://gist.github.com/AlexanderNixon/fb741fa2e950c7e0228394027ff9dffc
                bb = convert_to_darknet((w,h), b) 
                box = ' '.join(item.astype(str))
                f.write(f"0 {box}\n")
        print(img)
        plt.imshow(im)
        plt.show()

    else:
        ignored_ones.append(im_path)
```

and finally we can get results as following:

<center>
<figure><img src="https://i.loli.net/2021/10/19/HIGdWV7SykFtRaX.jpg" alt="shutterstock_1627199179_jpg.rf.8432d033a37b3d142ec4ffcede508c7d" width= "33%"  /><img src="https://i.loli.net/2021/10/19/3O7b6VhJKR4C5rs.jpg" alt="w1240-p16x9-0e48e0098f6e832f27d8b581b33bbc72b9967a63_jpg.rf.34ed1e8f70eebdabaf43ab9d40dc1c9b" width= "33%" /><img src="https://i.loli.net/2021/10/19/ctUPsBfw4ENmTG2.jpg" alt="126202-untitled-design-13_jpg.rf.56b50d413464989bb2232448a8fbb915" width= "33%" />
    <figure>
        <center>

<figure>
<center

## Inference

To run inference, we have to call `detect.py`, and adjust the following parameters:

- **weights:** weights of the trained model
- **source:** input file/folder to run inference on, `0` for webcam
- **output:** directory to save results
- **iou-thres**: IOU threshold for NMS, defaults to `0.45`
- **conf-thres**: object confidence threshold



## GUI 

For better interactions, I design an GUI for the project, where user can upload a image or video and the results can be shown in the application, the whole project can be packed as a .exe application by `pyinstaller`, and can be used by anyone even this person has no knowledge of programming and AI. 

The main tool is `Qtdesiner`, and the UI contains 3 `push_button` and 2 `image box` to show the original picture and result picture. 

![image-20211019212159702](https://i.loli.net/2021/10/19/MQCe5w3lBucb1Ar.png)

Then we translate this .ui file to .py file, this process can be down with a python script. Finally we need to utilize the `signal` and `slot` function to connect the GUI with our program, the core class is as follows:

```python
class MyMainWindow(QMainWindow,Ui_MainWindow):
    def __init__(self,parent=None):
        super(MyMainWindow,self).__init__(parent)
        self.setupUi(self)
    def opencamera(self):
        os.system('python detect.py --source 0 --weights weights/best.pt --conf-thres 0.6')
    def openimage(self):
        imgName, imgType = QFileDialog.getOpenFileName(self, "打开图片", "", "*.jpg;;*.png;;All Files(*)")
        #print(imgName,imgType)
        os.system('python detect.py --source %s --weights weights/best.pt --conf-thres 0.6'%imgName)
        path='D:/mlfinal_pj/inference'
        folder_name=os.listdir(path)[-1]
        img_name=os.listdir(path+'/'+folder_name)[0]
        img_path=path+'/'+folder_name+'/'+img_name
        print(img_path)
        jpg = QtGui.QPixmap(imgName).scaled(self.label.width(), self.label.height())
        jpg_detect=QtGui.QPixmap(img_path).scaled(self.label.width(), self.label.height())
        self.label.setPixmap(jpg)
        self.label_2.setPixmap(jpg_detect)
    def openvedio(self):
        vedioName, vedioType = QFileDialog.getOpenFileName(self, "打开图片", "", "*.jpg;;*.png;;All Files(*)")
        print(vedioName)
        os.system('python detect.py --source %s --weights weights/best.pt --conf-thres 0.60'%vedioName)
```

 

This is the final program(left is image detection and right are video detection):

![image-20211019213731744](https://i.loli.net/2021/10/19/1BFKvtwqRIOb9pm.png)



