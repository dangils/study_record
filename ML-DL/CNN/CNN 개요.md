## CNN(Convolution Neural Network) 구조
이미지를 인식하기위해 패턴을 찾는데 특히 유용하다. 데이터에서 직접 학습하고 패턴을 사용해 이미지를 분류한다. 특징을 수동으로 추출할 필요가 없다.  
이미지의 공간정보를 유지한채 학습을 하게 한다.  

<br> 

![image](https://user-images.githubusercontent.com/74512114/148873738-44c7b51d-434d-4527-bc28-da8da5eff71f.png)

![image](https://user-images.githubusercontent.com/74512114/148873752-25fb3bec-6743-45f4-835c-af8689a32a14.png)

<br>  

### Convolution Layer (합성곱 계층)

<br> 

#### Filter :
Filter를 통해 특징을 추출할 수 있고 이 추출한 특징을 Featuer map이라고 한다.필터는 데이터에 의한 학습 진행시 특징을 인식하고 필터를 만들어 낸다. 그리고 필터를 적용해 얻은 결과를 Feature Map 이라고 한다

<br> 

#### Stride : 필터를 원본이미지에 컨볼루션할 때, 얼마만큼의 간격으로 진행할지에 대한 값을 stride라고 한다.

<br> 

#### padding : 필터를 적용할 때 결과값이 작아야 되는데, 이러한 현상을 막기위해 input 계층 주위로 특정한 값(ex. 0 )으로 둘러쌓는 것.     이를 통해 결과값이 작아지는 것을 막는 특징이 유실되는 것을 막고 오버피팅을 방지

<br> 

### Activation Function(활성화 함수)   
Feature map에 특징이 있으면 픽셀은 큰값, 특징이 없는 픽셀은 0에 가까운 값이 담겨있다. 이 값이 정량적인 값으로 나오기 때문에, 이 값을 특징이 "있다 없다"의 비선형 값으로 바꿔주는 과정이 필요한데, 이것이바로 Actiavation Function 이다.    
(노드에 들어오는 값들에 대해 곧바로 다음 레이어로 전달하지않고 주로 비선형 함수를 통과시킨 후 전달한다. 이때 사용하는 함수 -> activation function)

> ReLu를 주로 사용하는 이유  
> 뉴럴 네트워크에서는 신경망이 깊어질수록 학습이 어렵다는 문제점이 있다. 이를 보완하기 위해 Back propagation(역전파)라는 방법을 사용하는데,이는 계산한 값을 재활용하여 다시 계산하는 것을 말한다. sigmoid 함수의 경우 레이어가 깊어지면 역전파(*값을 뒤에서 앞으로 전달할 때 희석되는 현상)가 제대로 작동하지 않기 때문에 ReLu를 사용한다.

<br>   

> 선형함수가 아닌 비선형 함수를 사용하는 이유  
> 선형함수를 사용할 시 층을 깊게 하는 의미가 줄어든다. 선형함수 f(x) = ax를 3층 네트워크로 쌓았다고 할 때 y(x) = f(f(f(x)))가 된다. 이는 사실 y(k) = cx 와 같다.(* c=a^3)   
이는 은닉층의 의미가 없어지므로 비선형 함수를 활성함수로 선택해야 한다.   
 
<br>   

![image](https://user-images.githubusercontent.com/74512114/148873771-6fdebab8-f6a5-4197-aca5-915b3f95ae86.png)
  
<br>   



