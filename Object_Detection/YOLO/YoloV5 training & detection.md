coco 데이터 셋 사이트

https://cocodataset.org/#home
    
custom_dataset - train - txt 
- 행 : 이미지 객체 갯수
- 0 : 각 객체의 색인
- 1 ~ : 경계 상자 중심의 x,y 좌표 , 경계 상자의 너비, 높이
[x,y 좌표는 이미지 전체에서의 비율로 표시됨 (0 ~ 1 사이 값)


#### 사전 학습 모델 확인

#### pre-trained model
- 사전 학습된 모델에 클래스를 '그냥' 추가한다는 것은 데이터세트에 레이블을 다시 지정하고 모델을 다시 학습시키는 것을 의미


#### trained model


1. CUDA ,Pytorch , YoloV5 설치 및  환경 세팅
2. yolov5-master -> data 폴더에 data set 받기(roboflow 데이터셋 활용)
3. 압축 해제한 데이터 셋의 data.yaml 확인, train:../train/images,val:../valid/images 등의 경로 변경 해줘야함
4. 데이터셋 폴더의 train, valid 경로를 복사하여 data.yaml 파일내 train, val 경로 수정



### train 명령
예시)
```python
 python train.py --img 416 --batch 16 --epochs 50 --data=FruitDataset\data.yaml --cfg=models/yolov5s.yaml --weights yolov5s.pt --name fruit_yolov5s_results --device 0

```
- --img : 학습할 사진의 크기
- --batch : 전체 데이터를 얼만큼 쪼개서 학습 할 건지를 지정
- --epochs : 전체 데이터를 몇 회 반복 학습 할 건지를 지정
- --data : 데이터셋의 경로정보를 가지고 있는 yaml 파일 지정
- --cfg : 학습에 사용할 모델의 설정파일(yaml파일)
- --weights : 학습에 사용할 모델 파일 지정 (자동으로 다운로드 됨)
- --name : 학습결과를 저장할 폴더명 
- --device 0 : GPU를 선택하여 트레이닝 실행


✔ 예시 코드에서 사진 크기는 416, 전체 데이터를 16개씩 쪼개서 학습, epochs 50번 반복(너무 크면 오버피팅, 너무 작으면 언더피팅)   
run 디렉토리 밑에 fruit_yolov5s_results로, GPU를 선택하여 학습 수행 및 결과 저장

<br> 

----

#### 결과

✔ yolov5-master - runs - train - fruit_yolov5s_results 폴더에 결과 저장     
✔ yolov5-master - runs - train - fruit_yolov5s_results - weights - pt파일 활용하여 detect 수행     
 
- best.pt (가장 좋은 epoch의 weight(best mAP - 평균 정밀도) 가 들어있음)  
- last.pt (마지막 epoch의 weight)   
- pretrain된 weight를 활용가능    
   ```ex) python train.py --img 640 --batch 16 --epochs 100 --data dpic.yaml --weights yolov5l.pt ```
   

-----     


### Mask Dataset -  detection 수행    
   

1) 데이터셋 : https://public.roboflow.com/object-detection/mask-wearing/4  [ roboflow의 maskdataset 활용 ]

2) yolov5-master - data - MaskWearing 데이터셋 저장

3) yolov5-master - data - MaskWearing - data.yaml 의 train,test,val 경로 수정

4) training 수행   

```python train.py --img 416 --batch 16 --epochs 100 --data=MaskWearing\data.yaml --cfg=models/yolov5s.yaml --weights yolov5s.pt --name mask_yolov5s_results --device 0 ```   

-> 416 사이즈 16벤치 100epochs, weights는 yolov5s 로 수행하여 runs 폴더에 make_yolov5s_results로 저장


#### Cam detection 수행

<br> 

``` python .\detect.py --weight .\runs\train\mask_yolov5s_results\weights\best.pt --img 416 --conf 0.5 --source 0```


<br> 

- weight는 train 결과인 best.pt 사용, 사이즈 416, confidence level 0.5 설정, source 0 (cam 으로 설정)   

![image](https://user-images.githubusercontent.com/74512114/148300504-b8515250-66c3-4854-8e45-782956531d3d.png)


<br> 

-----

#### Image detection 수행

```python .\detect.py --weight .\runs\train\mask_yolov5s_results\weights\best.pt --img 416 --conf 0.5 --source C:\Users\you02\yolov5-master\yolov5-master\data\MaskWearing\img_detect\test01.png```

![image](https://user-images.githubusercontent.com/74512114/148300072-de1a56af-922d-4737-9901-884ab62cd9c7.png)


<br> 

![image](https://user-images.githubusercontent.com/74512114/148300090-8f036315-b796-4eb0-b773-7807a16ad7d6.png)


<br>

----

#### Video detection 수행
- youtube 영상 mp4 파일로 detect

![image](https://user-images.githubusercontent.com/74512114/148300762-42610bd8-1e91-4a14-85ab-e826da2aceea.png)


<br>

아직 내부 파라미터를 조정하지 않아 오류가 많은 상태입니다.

### 모델 최적화 개요

<br> 

#### model tuning   

딥 러닝 모델에서의 대략적인 튜닝대상 : 

- FNN·CNN·RNN·트랜스포머 등등 어떤 계열의 모델 구조를 이용할지

- 인공 신경망의 층 수. 한 층(layer)에 들어갈 인공 뉴런의 수, convolution filter size와 filter 수

- 얼마만큼의 정도, 간격으로 학습시킬 것인지 / 모델의 파라미터 업데이트를 얼마만큼 큰 단위로 할지 결정하는 학습률 (learning rate) 

- (mini-batch size), (epoch) , 최적화 기법을 쓰고(optimizer), 손실 함수(cost function) 활성화 함수(activate function) 등

<br> 

------- 

<br> 

참조 : https://docs.microsoft.com/ko-kr/azure/machine-learning/concept-automated-ml

<br> 

#### AutoML(Automated Machine Learning)     
- 자동으로 적절한 AI가 학습되도록 도와주는 기법 -> 특정 태스크를 위한 모델 학습에 한해 사람이 주기적으로 실험에 개입하지 않고 AI가 스스로 반복 실험을 통해 성능을 개선하는 것

1) Feature Engineering : AI 모델 학습을 위해 데이터에서 중요한 특징(feature)

<br> 

2) HyperParameter 자동 탐색 :   

Grid search : 
- 최적화할 하이퍼파라미터의 값 구간을 일정 단위로 나눈 후 각 단위 조합을 테스트해 가장 높은 성능을 낸 하이퍼파라미터 조합을 선택하는 방식
- 단순하지만 최적화 대상이 되는 하이퍼 파라미터가 많다면 경우의 수가 기하급수적으로 많아져 탐색에 오랜 시간이 걸릴 수 있다. 

<br> 

Random search : 
- 랜덤하게 하이퍼파라미터의 조합을 테스트하는 방식인데 그리드 서치에 비해 비교적 빠르게 최적의 조합을 찾아낸다.  

<br>

3) AI Architecture 탐색 : AI 모델의 구조 자체를 더 효율적인 방향으로 찾아주는 아키텍처 탐색    

- 여기서 아키텍처는 모델을 이루는 구조를 말하는데, 사람이 직접 어떤 방식으로 모델 구조를 짤지 생각하지 않아도 자동 탐색을 통해 최적 구조를 찾는다.   

- 딥 러닝 모델은 인공 신경망을 활용하기 때문에 NAS(Neural Architecture search)라고 부른다.    

- NAS도 마찬가지로 대부분 메타 학습 모델과 학습 모델로 이뤄져 있어 학습 모델이 본 과제를 수행하는 AI 모델이라면 메타 학습 모델이 어떤 구조의 신경망을 만들면 좋은지 아키텍처 구성을 고민한다.  










   
   
