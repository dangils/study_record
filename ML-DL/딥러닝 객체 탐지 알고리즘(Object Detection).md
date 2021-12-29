## 딥러닝 객체탐지 알고리즘의 이해
<br>  

### Object Detection(객체 탐지) 란?
이미지에서 관심 객체를 배경과 구분해 식별하는 자동화 기법으로, computer vision 기술의 하위 집합 


![image](https://user-images.githubusercontent.com/74512114/147708164-6b6f492f-0ad0-4425-8bf0-00bdc82a8ca8.png)

<br>    
  
### 객체 탐지 알고리즘
 - 영역 제안 (Region Proposal)   
 객체를 포함할 가능성이 높은 영역을 선택적 탐색(Selective Search)같은 컴퓨터 비전 기술을 활용하거나 딥러닝 기반의 영역 제안 네트워크(RPN : Region Proposal Network)를 통해 선택,  
 후보군의 bounding box 세트를 취합하여 회귀 모델과 분류 모델의 수를 공식화해 객체를 탐지. ex) Faster R-CNN, R-FCN 등.  
 Two-Stage Methods라고 불리는 이 알고리즘들은 높은 정확도를 제공하지만 단일 단계(Single-Stage Methods)보다는 처리 속도가 느리다.  
 
 <br>  
 > Regression(회귀) : 회귀분석은 데이터 변수들간에 함수관계를 파악하여 통계적 추론을 하는 기술
 
<br>   
    
- 정해진 위치와 정해진 크기의 객체만 찾음  
이 위치와 크기들은 대부분의 시나리오에 적용할 수 있도록 전략적으로 선택된다. 이 카테고리의 알고리즘은 보통 원본 이미지를 고정된 사이즈 그리드 영역으로 나누는데, 각 영역에 대해 형태와 크기가 미리 결정된 객체의 고정 갯수를 예측한다. Single-Stage Methods 라고 불리며 YOLO, SSD, RetinaNet 같은 알고리즘이 포함된다. Two-Stage Methods 방식보다는 정확도가 떨어지지만 빠른 처리가 가능하다. 이 유형은 보통 실시간 탐지를 요구하는데 많이 활용된다.


<br>  

### YOLO(You Only Look Once) :    
원본 이미지를 동일한 크기의 그리드로 나눈다. 각 그리드에 대해 그리드 중앙을 중심으로 미리 정의된 형태(predefined shape)으로 지정된 경계박스의 갯수를 예측하고 이를 바탕으로 신뢰도를 계산한다.  이미지에 객체가 포함되어 있는지, 또는 배경만 단독으로 있는지에 대한 여부가 포함되고, 높은 객체 신뢰도를 가진 위치를 선택해 객체 카테고리를 파악한다.   

![image](https://user-images.githubusercontent.com/74512114/147708201-1a5bf2e1-0cf7-41b3-b47c-42046ec54277.png)


<br>   

![image](https://user-images.githubusercontent.com/74512114/147708215-124220c9-3177-4b6e-b805-facf1965e9e3.png)

✔ 이미지 그리드를 resize한 후 single convolution을 동작(CNN 동작)    
-> output의 결과를 NMS를 통해 걸러내 객체를 탐지 (NMS : CNN의 MaxPooling같은 느낌. 정확히는 현쟈 픽셀을 기준으로 주변의 픽셀과 비교했을 때 최대값인 경우 그대로, 아닐 경우 억제) 
- Bounding-box와 Class probability를 하나의 문제로 간주하여 객체의 종류와 위치를 한번에 예측. 
- 이미지를 일정 크기의 그리드로 나눠 각 그리드에 대한 Bounding-box를 예측합니다.
- Bounding-box의 confidence score와 그리드셀의 class score의 값으로 학습하게 됩니다. 
- 간단한 처리과정으로 속도가 매우 빠르지만 작은 객체에 대해서는 상대적으로 정확도가 낮습니다.

<br>  
   
#### One-Stage Detector
one-stage detector는 Classification, Regional Proposal을 동시에 수행하여 결과를 얻는다.   
![image](https://user-images.githubusercontent.com/74512114/147708231-f5f30aa6-588e-4f60-817e-00c9e3d45082.png)

<br>  

#### Two-Stage Detector  
Two-stage Detector는 Classification,Regional Proposal을 순차적으로 수행하여 결과를 얻는 방법  
![image](https://user-images.githubusercontent.com/74512114/147708238-e688af2e-3a04-4757-973f-ec78ded9f418.png)

<br>  


### YOLO의 특징 
1. 빠른 탐지 (실시간에 적합) : 회귀문제로 인식하기 때문에 복잡한 파이프라인이 필요하지 않다. 간단한 신경망 학습을 통해 예측하는 것으로 기본적으로 45FPS, fast - yolo는 150FPS 이상이다.  
2. 이미지를 전체적으로 파악 : yolo는 train과 test 시간중에 이미지 전체를 보기 때문에 클래스의 외관뿐만 아니라 상황에 따른 정보도 암묵적으로 Encoding한다. 따라서 Fast R-CNN와 비교했을 때 Background Error [ex) 강아지 뿐만 아니라 뒷배경도 인식하는 에러]가 반 이하로 낮다.

3. yolo는 일반화를 배운다 : 일반적인 이미지로 train하고 예술작품으로 test를 돌렸을 때 DPM(Deformable Parts Model)과 R-CNN보다 성능이 좋다. 이유는 YOLO가 일반화가 잘 되기 때문에 새로운 분야나 예상치 못한 이미지가 들어왔을 때 예측을더 잘할 수 있다.
