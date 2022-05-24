# MaskR-CNN_holecover
  ## 1. Label the objects
  Using VGG Image Annotator (VIA) annotaions technique
  
  Classes: manhole, handhole
  
  ## 2. Mask R-CNN environment
  * Operating System: Ubuntu 18.04
  * GPU: NVIDIA GeForce RTX2080 Ti
  * CUDA 9.1
  * Tensorflow-gpu 1.12
  * python 3.6

  ## 3. Modify config.py `hole_project.py`
  * Modify number of classes (classes number + background)
  * Modify batch size: `IMAGES_PER_GPU`*`GPU_COUNT`
  * Modify the iterations per epoch: `STEPS_PER_EPOCH` (according to the training samples// batch size)
  * Modify the threshold of the confidence scores:`DETECTION_MIN_CONFIDENCE`
  * Modify the classes name `add_class`
  * Skip small objects
  ```python
  for k in annotations:
      for j in range(len(k['regions'])):
          x = k['regions'][j]['shape_attributes']['all_points_x']
          y = k['regions'][j]['shape_attributes']['all_points_y']
          xy_list = [[x[i],y[i]] for i in range(min(len(x),len(y)))]
          #print(k['filename'],Polygon(xy_list).area)
          if Polygon(xy_list).area < 1100: #######
            	k['regions'][j].clear()
      k['regions'] = list(filter(None, k['regions'])) ##remove the empty {}
  ```
  
  ## 4. Training
      python3 bev_project.py train --weights=coco --dataset=/home/geo/kh/Mask_RCNN-master_sign/samples/sign_data
      
  ## 5. Result
  manhole
  
  ![image](https://github.com/yichun-hub/MaskR-CNN_holecover/blob/main/result/manhole.jpg)
  
  handhole
  
  ![image](https://github.com/yichun-hub/MaskR-CNN_holecover/blob/main/result/handhole.jpg)
