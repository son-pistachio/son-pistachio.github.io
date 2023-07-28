---
title: label Unique RGB 확인 / Image Channel 확인
date: 2023-07-28
categories: [Computer Vision]
tags: [Computer Vision, 컴퓨터비전, Deep Learning]
---
  

## label Unique RGB 확인  


#### code  
```python
import cv2
import numpy as np

def get_mask_image_rgb_values(mask_image_path):
    # 이미지를 컬러 이미지로 읽기
    mask_image = cv2.imread(mask_image_path, cv2.IMREAD_UNCHANGED)

    if mask_image is not None:
        if len(mask_image.shape) == 2:
            # 이진 이미지인 경우 컬러 이미지로 변환
            mask_image = cv2.cvtColor(mask_image, cv2.COLOR_GRAY2BGR)

        # 이미지를 1차원 배열로 변환 (높이, 너비, 채널) -> (픽셀 수, 채널)
        pixel_values = mask_image.reshape(-1, 3)

        # unique RGB 찾기
        unique_rgb_values = np.unique(pixel_values, axis=0)
        return unique_rgb_values
    else:
        print(f"Failed to read the image at '{mask_image_path}'.")
        return None

# 마스크 이미지 파일 경로
mask_image_path = "파일경로입력"
print(mask_image_path)

# 마스크 이미지의 RGB 값 확인
rgb_values = get_mask_image_rgb_values(mask_image_path)
if rgb_values is not None:
    print("RGB values in the mask image:")
    print(rgb_values)
```  
  
#### 결과 출력 예시  
```
RGB values in the mask image:
[[  0   0   0]
 [  0  51  51]
 [  0  76 130]
 [ 35 142 107]
 [ 87 103 112]
 [ 96  22 255]
 [115 135   2]
 [128  64 128]
 [156 102 102]]
```  


## Image Channel 확인  

#### code  
```python
import cv2

def get_image_channels(image_path):
    # 이미지를 컬러 이미지로 읽기
    image = cv2.imread(image_path, cv2.IMREAD_COLOR)

    if image is not None:
        # 이미지의 채널 수 확인
        channels = image.shape[2] if len(image.shape) == 3 else 1
        return channels
    else:
        print(f"Failed to read the image at '{image_path}'.")
        return None
  
# 이미지 파일 경로
image_path = "파일경로입력"
  
# 이미지의 채널 수 확인
channels = get_image_channels(image_path)
if channels is not None:
    print(f"The image has {channels} channel(s).")
```
  
#### 결과 출력 예시  
```
The image has 3 channel(s).
```
  
  
