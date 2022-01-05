# YOLO (You Only Look Once)

- 가장 빠른 객체 검출 알고리즘 중 하나
- 256x256 사이즈의 이미지
- 파이썬, 텐서플로 기반 프레임워크가 아닌 C++로 구현된 코드 기준 GPU 사용 시, 초당 170 프레임(170 FPS, frames per second)
- 작은 크기의 물체를 탐지하는데는 어려움

<img src="https://miro.medium.com/max/1400/1*bSLNlG7crv-p-m4LVYYk3Q.png" width="600">

- https://pjreddie.com/darknet/yolo/
- https://www.youtube.com/watch?v=MPU2HistivI


## YOLO 계층 출력

- 마지막 계층 출력은 $w \times h \times M$ 행렬
  - $M = B \times (C + 5)$
    - B : 그리드 셀당 경계 상자 개수
    - C : 클래스 개수
  - 클래스 개수에 5를 더한 이유는 해당 값 만큼의 숫자를 예측해야함
    - $t_x$, $t_y$는 경계상자의 중심 좌표를 계산
    - $t_w$, $t_h$는 경계상자의 너비와 높이를 계산
    - $c$는 객체가 경계 상자 안에 있다고 확신하는 신뢰도
    - $p1, p2, ..., pC$는 경계상자가 클래스 1, 2, ..., C의 객체를 포함할 확률

  <img src="https://www.researchgate.net/profile/Thi_Le3/publication/337705605/figure/fig3/AS:831927326089217@1575358339500/Structure-of-one-output-cell-in-YOLO.ppm">

- Objectness Score: 바운딩 박스에 객체가 포함되어 있을 확률



## 앵커 박스(Anchor Box)

- YOLOv2에서 도입
- 사전 정의된 상자(prior box)
- 객체에 가장 근접한 앵커 박스를 맞추고 신경망을 사용해 앵커 박스의 크기를 조정하는 과정때문에 $t_x, t_y, t_w, t_h$이 필요

  <img src="https://kr.mathworks.com/help/vision/ug/ssd_detection.png">


![image](https://user-images.githubusercontent.com/74512114/148299421-e5f75f9b-1c9e-41ca-930a-b79c3fb1fa72.png)

### YOLO V5
> CNN 기반 객체 감지기로 고성능의 물체 검출을 위해 사용된다.  
> CSPNet 기반의 backbone 아키텍처로 만들어졌으며 Darknet이 아닌 PyTorch로 구현되어있다.

<br>    

아키텍쳐란?  
1.시스템 구성 및 동작 원리  
2.시스템 구성요소에 대한 설계 및 구현을 지원하는 수준을 기술  
3.구성 요소 간의 관계 및 외부환경과의 관계 묘사  
4.요구사양 및 시스템 수명주기 고려  
5.시스템의 전체적인 최적화를 목표  
✔ 최적화를 목표로 두고 시스템의 구성과 동작원리 그리고 시스템의 구성환경등을 설명 및 설계하는 청사진 또는 설계도  
✔ 기본 Computer Science 지식을 기반으로 주변환경등을 고려하여, 최상의 소프트웨어를 구성하는 방법을 연구하고 이를 바탕으로 가이드하는 역할  


<br>    
  
backbone : CSPNet
- CNN의 학습능력 강화 : 정확도를 유지하며 경량화 가능
- 메모리 cost 감소



<br>    


### YOLOv5 Preprocessing 
1. Dataset 준비   
coco dateset으로 준비 (YOLO의 pretrained 모델은 COCO dataset으로 사전학습되었다)

<br>    


2. Data type 변환
YOLOv5를 이용하기 위해 COCO type 데이터셋을 YOLO type으로 변환 시켜야 한다.  

>COCO dataset의 bounding box: 좌측상단의 x,y 좌표, + w + h 로 구성   
YOLO dataset의 bounding box : box의 중심점의 x,y + 전체이미지의 상대적 w + h  

![image](https://user-images.githubusercontent.com/74512114/148299509-f3729e6b-33e8-4aec-abad-edd8ebf12e53.png)



<br>    



3. Training  
    > $ git clone https://github.com/ultralytics/yolov5.git  
   
커스텀 데이터를 학습시키기 위해 2가 준비  
1. custom.yaml  
train: /opt/ml/detection/dataset/manifest_train.txt   
val: /opt/ml/detection/dataset/manifest_val.txt  
 
2. parameter.yaml    
lr0 : 0.0117  
lrf : 0.0855 ...  

<br>    



### YOLO의 특징  


Unified : 통합된 모델 사용  
- 다른 객체 탐지 모델 들은 전처리 모델과 인공 신경망을 결합해서 사용하지만 YOLO는 단 하나의 인공신경망에서 이를 전부 처리한다.  
이런 특징 때문에 YOLO가 다른 모델보다 간단해 보인다.

- Real-time Object Redtection : 실시간 객체 탐지 : 실시간으로 여러장의 이미지를 탐지 할 수 있다. 영상을 스트리밍 하면서 동시에 화면 상의 물체를 부드럽게 구분할 수 있다.    

<br>    

- 1-stage detector중 하나이다. 2-stage detector인 <Fast R-CNN>이 "CNN + RPN"으로, 2개의 네트워크로 구성되어 있다면,   
  1-stage dectector 인 YOLO는 단일 네트워크로 구성되어 있다. 단일 네트워크 모델이기 때문에,"end-to-end" 학습이 가능하다.


### 머신러닝에서의 물체 인식 

<br>   


머신러닝에서의 물체인식(Object Detection)은 특정 이미지에서 테두리 상자(Bounding Box)를 통해 영역을 설정하고,     
해당 영역 내 물체의 존재 유무 또는 물체의 종류를 판별하는 것을 목표로 하는 과제를 말한다.  

mAP (mean Average Precision) : 정확성
FPS (Frame Per Seconds) : 속도  

<br>   


#### YOLO 모델의 정의 
물체 인식(Object Detection)을 수행하기 위해 고안된 심층 신경망으로서, 테두리상자 조정(Bounding Box Coordinate)과 분류(Classification)를 통한 신경만 구조를 통해 동시에 실행하는 통합인식(Unified Detection)을 구현하는 것이 큰 특징

![image.png](attachment:image.png)  
  
  
  ### 용어정리  

<br>


#### 가중치 (Weight) W
- 뉴런 사이의 연결 강도, learnable parameters 라고도 불림
- 노드에 대한 중요도를 표현하는 것으로 입력 데이터에 노드 별 비중을 크거나 작게 둠으로써 곱해지는 값을 달리한다
- 노드에 입력되는 각 신호가 결과 출력에 미치는 중요도를 조절하는 매개변수(파라미터)

<br>


#### 편향 (Bias) 
- 노드의 만감도를 조정하거나 활성화를 조정하는 역할, 가중치 만으로 세밀한 조정이 되지 않은 부분에서 편향을 주어 조정 가능.  
약간의 편향 조정을 통해 연산의 결과가 함수를 활성화하는 조건에 부합하도록 할 수 있음  
- 뉴런의 활성화 조건을 조절하는 매개변수(파라미터)


<br>


#### 활성화 함수(activation function) 
- 인공 신경망에서, 입력받은 신호의 가중합을 다음 노드로 보낼지 말지를 결정하는 함수 


### MLP (Multi-Layer Perceptron)  
- 다층 인공신경망


















