# ☁Cloud_segmentation☁

## 팀정보

|팀명|운 좋은 날|
|:----:|:---------:|
|주제|인공위성 영상에서 구름 검출하기 |
|팀장|문재영|
|팀원|박용현, 정의경|
|담당 퍼실|김인유|


## 1. 프로젝트 소개

원격 탐사 분야에서 구름의 식별과 추출은 광학 위성 영상을 분석하는데 있어 매우 중요한 전처리 작업이다. 지금까지 구름을 추출하는 많은 방법이 제안되어 왔으나 대부분이 다중 분광 밴드의 개별적인 픽셀값에 의존하다 보니 다목적 실용위성 3호 / 3A 같이 제한된 수의 분광 정보를 가지는 경우 그 성능이 충분하지 않다는 경우가 많았다. 최신의 딥러닝 기술을 이용하여 구름 검출을 수행하는 알고리즘을 구축한다.


## 2. 프로젝트 목표

- 영상 내 구름의 점유율 분석
- 설원지역에서 구름 segmentation
- EDA를 통해 데이터를 분석하여 검출 모델 성능을 향상


## 3. 데이터 소개

> - 아리랑 3호 및 3A호(KOMPSAT-3A) 에서 취득한 위성영상 패치
> - R,G,B, NIR 4채널로 생성된 영상 활용
> - GSD(Ground Sample Distance) : 0.7 / 0.55 m
> - 구름의 특징에 따라 두꺼운 구름, 얇은 구름, 그림자, 청천으로 분류
> - 1024x1024 크기의 패치 단위
> - 다양한 지표 특성을 반영하여 4000장 이상


### - Data Image

|PNG File(NGR)|PNG FIle(RGB)|tif File|PNG FIle(Label)|
|:----:|:---------:|:----:|:----:|
|![CLD00001_NGR_K3A_NIA0001](https://user-images.githubusercontent.com/66707865/150467296-72ed74e0-8eab-40f1-b52d-ace2ca7b9b0e.png)|![CLD00001_RGB_K3A_NIA0001](https://user-images.githubusercontent.com/66707865/150467306-ebcecdca-7f2b-403a-b34e-2b67a112ebe7.png)|![2022-01-21 13 54 50](https://user-images.githubusercontent.com/66707865/150468446-12effd76-ea6e-4dfd-b493-1871bc3caced.jpg)|![CLD00001_MS4_K3A_NIA0001_label](https://user-images.githubusercontent.com/66707865/150469172-5a04ed35-10be-4016-b8c3-be17437509ec.png)|

> - NGR File : Blue채널을 빼서 대기의 영향이 상대적으로 적게 나타나며 녹지의 특성이 두드러지게 나타나고 결론적으로 구름 그림자가 더 두드러지게 나타남.
> - tif : 위경도 정보가 들어 있어서 지도에 표현이 가능함. 영상을 열고 데이터를 확인 하기 위해서는 위성영상 무료 전문 프로그램인 QGIS 를 사용해야 한다.
> - Label : 빨간색-짙은구름, 녹색-옅은구름, 노란색-그림자 를 의미하며 비트맵 형태인 png파일로 구성.


## 4. Baseline Model

- Dilated U-net

  ![image](https://user-images.githubusercontent.com/66707865/150466007-415f7e51-bbca-4365-a6c4-c4022c727ed8.png)
  
  Semantic Segmentation Algorithm 중 대표적인 U-Net에 기존 Convolution과 다르게 kernel 중간에 간격을 두어 더 넓은 recpetive field를 얻을 수 있는 Atrous Convolution을 추가로 적용한다.  
  
- Deeplab V3

  ![image](https://user-images.githubusercontent.com/90362552/150472271-d557395d-821d-4b64-95d5-99ea5ddc37c5.png)

  Resnet-101을 백본으로 사용을 하고 Atrous Convolution을
  병렬로 적용을 한 Atrous Spatial Pyramid Polling(ASPP)기법을 적용한다.

- HRNet v2

  <img width="754" alt="스크린샷 2022-01-21 14 27 23" src="https://user-images.githubusercontent.com/90365512/150471129-d2b876d0-df1b-40a2-a7e1-fb24981ea98d.png">

  모든 프로세스에서 high resolution representation을 보존한다.
  
  High-resolution convolution stream으로부터 시작하고 스테이지를 하나씩 넘어가면서 low resolution branch가 하나씩 확장되는 구조.
  
  n번째의 stage에서의 출력은 n개의 high-low resolution representation으로 구성되고 각 stage가 끝날때 마다 feature를 fuse하는 부분이 존재한다.
  
  high resolution을 유지하면서 multi scale fusion을 수행하므로 semantic한 관점에서 성능이 좋다.
  
  

## 5. 프로젝트 팀원 역할 분담

|이름| 담당 업무 |
|:----:|:---------:|
|문재영| 전처리, 모델 구축, EDA|
|박용현| 전처리, 모델 구축, EDA|
|정의경| 전처리, 모델 구축, EDA|

## 6. 개발 환경

> - colab pro(python 3.7.12)
> - tensorflow 2.7
> - keras 2.7
> - opencv 4.1.2
> - matplotlib 3.2.2

## 7. Reference
