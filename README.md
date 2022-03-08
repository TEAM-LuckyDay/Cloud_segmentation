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


- Data Image

|PNG File(NGR)|PNG FIle(RGB)|tif File|PNG FIle(Label)|
|:----:|:---------:|:----:|:----:|
|![CLD00001_NGR_K3A_NIA0001](https://user-images.githubusercontent.com/66707865/150467296-72ed74e0-8eab-40f1-b52d-ace2ca7b9b0e.png)|![CLD00001_RGB_K3A_NIA0001](https://user-images.githubusercontent.com/66707865/150467306-ebcecdca-7f2b-403a-b34e-2b67a112ebe7.png)|![2022-01-21 13 54 50](https://user-images.githubusercontent.com/66707865/150468446-12effd76-ea6e-4dfd-b493-1871bc3caced.jpg)|![CLD00001_MS4_K3A_NIA0001_label](https://user-images.githubusercontent.com/66707865/150469172-5a04ed35-10be-4016-b8c3-be17437509ec.png)|

> - NGR File : Blue채널을 빼서 대기의 영향이 상대적으로 적게 나타나며 녹지의 특성이 두드러지게 나타나고 결론적으로 구름 그림자가 더 두드러지게 나타남.
> - tif : 위경도 정보가 들어 있어서 지도에 표현이 가능함. 영상을 열고 데이터를 확인 하기 위해서는 위성영상 무료 전문 프로그램인 QGIS 를 사용해야 한다.
> - Label : 빨간색-짙은구름, 녹색-옅은구름, 노란색-그림자 를 의미하며 비트맵 형태인 png파일로 구성.
> - 출처 : https://aihub.or.kr/aidata/7982


## 4. Baseline Model

- Dilated U-net

  ![image](https://user-images.githubusercontent.com/66707865/157150234-7a1137a6-e2eb-45da-8e6b-3d486b60956e.png)
  
  Semantic Segmentation Algorithm 중 대표적인 U-Net에 기존 Convolution과 다르게 kernel 중간에 간격을 두어 더 넓은 recpetive field를 얻을 수 있는 Atrous Convolution을 추가로 적용한다.  
  

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

- 오한, 김보람, 김진형, “다목적실용위성 3호/3A호 영상을 위한 딥러닝기반 구름 검출 알고리즘”, 한국항공우주학회 학술발표회 초록집, pp128-130, 2020

- Olaf Ronneberger, Philipp Fischer, Thomas Brox, "U-Net: Convolutional Networks for Biomedical Image Segmentation",  arXiv:1505.04597v1, 2015.

- Liang-Chieh Chen, George Papandreou, Florian Schroff, Hartwig Adam, "Rethinking Atrous Convolution for Semantic Image Segmentation", arXiv:1706.05587v3, 2017.

- Ke Sun, Yang Zhao, Borui Jiang, Tianheng Cheng, Bin Xiao, Dong Liu, Yadong Mu, Xinggang Wang, Wenyu Liu, Jingdong Wang, "High-Resolution Representations for Labeling Pixels and Regions", arXiv:1904.04514v1, 2019.



