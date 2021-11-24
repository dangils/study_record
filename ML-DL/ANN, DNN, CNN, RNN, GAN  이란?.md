## 딥러닝 학습 모델의 대표적인 5가지 [ ANN, DNN, CNN, RNN, GAN ] 

### ANN(Artificial Neural Network : ANN) 인공 신경망  
- 인공 신경망 ANN은 간략히 (Neural Network) 라고도 하는데, 인간의 뉴런이 연결된 형태를 수학적으로 모방한 모델
- 뇌에서 뉴런들이 어떤 신호, 자극 등을 받고, 그 자극이 어떠한 임계값을 넘어서면 결과 신호를 전달하는 과정에서 착안
- ANN 뉴런은 여러 입력값을 받아 일정 수준이 넘어서면 활성화되어 출력값을 내보낸다
- ANN은 뉴런들을 여러 개 쌓아, 두 개의 층(Layer)이상으로 구성되는 것이 특징이다.

<img src="https://user-images.githubusercontent.com/74512114/143187526-22283775-d77a-45d2-a4ae-f3e9b9b304d7.png" width="400" height="400"/>

### DNN(Deep Neural Network : DNN)
- ANN 이후 모델 내 은닉층(Hidden Layer)을 많이 늘려서 학습의 결과를 향상시키는 방법이 등장하였고, 이를 심층 신경망이라고 한다.
- DNN은 ANN에 비해 더 적은 수의 유닛들만으로도 복잡한 데이터를 모델링 할 수 있게 해준다.
- 컴퓨터가 스스로 분류 레이블을 만들어 내고, 공간을 왜곡하고 데이터를 구분 짓는 과정을 반복하여 최적의 구분선을 도출해 낸다.
- 많은 데이터와 반복학습, 사전 학습과 오류역전파 기법을 통해 현재 널리 사용 중 이다.

<img src ="https://user-images.githubusercontent.com/74512114/143188708-15fbdd93-9597-4d1c-8e08-0172af22182b.png" width="700" height="400"/>


### CNN 합성곱 신경망(Convolution Neural Network : CNN)
- 기존의 방식은 데이터에서 지식을 추출해 학습이 이루어졌지만, 합성곱 신경망 CNN은 데이터의 특징을 추출하여 특징들의 패턴을 파악하는 구조
- CNN 알고리즘은 Convolution과정과 Pooling과정을 통해 진행된다
- Convolution Layer와 Pooling Layer를 복합적으로 구성하여 알고리즘을 만든다.
<img src="https://user-images.githubusercontent.com/74512114/143189823-dfb014ee-d230-424b-8f5a-136a5f789c4a.png" width="700" height="200"/>

- 최근 가장 인기 있는 모델이며, 대표적인 예시로 알파고가 있다

### RNN(Recurrent Neural Network : RNN)
- 순환 신경망 RNN 알고리즘은 반복적이고 순차적인 데이터(Sequential Data)학습에 특화된 인공신경망의 한 종류로써 내부의 순환구조가 들어있다는 특징을 가지고 있다
- 순환구조를 이용하여 과거의 학습을 Weight를 통해 현재 학습에 반영
- 기존의 지속적이고 반복적이며 순차적인 데이터학습의 한계를 해결하는 알고리즘
- 
