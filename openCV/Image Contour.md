### Contour  
contour는 등고선이라는 뜻으로 같은 값을 가진 곳을 연결한 선이다.     
이미지에서 contours는 동일한 색 또는 동일한 강도를 가지고 있는 영역의 경계선을 연결한 선으로 물체 모형 분석이나 객체 감지 및 인식 알고리즘에 주로 사용하는 툴이다.

1. 정확도를 높이기 위해 주로 Binary 이미지를 사용한다. 그래서 Contours를 찾기 전에 threshold나 Canny 경계선 탐지 등을 이미지에 먼저 적용한다.    
2. cv2.drawContours() 함수는 원본 이미지를 직접 수정하기 때문에, 원본 이미지를 보존하려면 copy() 함수를 사용해야 한다.      
3. openCV에서 Contours를 찾는 것은 검정색 배경에서 하얀색 대상을 찾는 것과 비슷하므로 찾고자 하는 대상은 흰색, 배경은 검은색으로 하는 것이 좋다.     

<br>

- findContours [소스 이미지 , 등고선 검색 모드(contour 제공 방식) , 등고선 근사 방법]
- cv2.CHAIN_APPROX_SIMPLE : 수직선, 수평서느 대각선에 대해 끝점만 저장

<br>

#### findContours 의 2번째 인자(contour search 방법) 
- cv2.RETR_LIST : 이미지에서 발견한 모든 Contour들을 계층에 상관하지 않고 나열, 계층 구조를 생성하지 않음
- cv2.RETR_TREE : 모든 Contour들의 관계를 명확히 해서 리턴 (계층 구조를 생성)
- cv2.RETR_EXTERNAL : 외곽 윤곽선만 검출하며, 계층 구조를 구성하지 않음
- cv2.RETR_CCOMP : 모든 윤고가선을 검출하며, 계층 구조는 2단계로 구성


#### 근사화 방법

- cv2.CHAIN_APPROX_NONE : 윤곽점들의 모든 점을 반환합니다.
- cv2.CHAIN_APPROX_SIMPLE : 윤곽점들 단순화 수평, 수직 및 대각선 요소를 압축하고 끝점만 남겨 둡니다.
- cv2.CHAIN_APPROX_TC89_L1 : 프리먼 체인 코드에서의 윤곽선으로 적용합니다.
- cv2.CHAIN_APPROX_TC89_KCOS : 프리먼 체인 코드에서의 윤곽선으로 적용합니다.

 

각각인자 값에 따른 hierarchy(계층) 결과      


cv2.RETR_LIST 인 경우      

hierarchy =      
[[[ 1 -1 -1 -1]              
    [ 2  0 -1 -1]                 
    [ 3  1 -1 -1]              
    [ 4  2 -1 -1]        
    [ 5  3 -1 -1]                     
    [-1  4 -1 -1]]]               


cv2.RETR_TREE 인 경우      
hierarchy =       
[[[ 5 -1  1 -1]        
    [-1 -1  2  0]            
    [-1 -1  3  1]            
    [-1 -1  4  2]             
    [-1 -1 -1  3]         
    [-1  0 -1 -1]]]             


```python
import cv2
import matplotlib.pyplot as plt

'''
✔ 객체의 윤곽선을 이용한 무게중심(중심점) 산출
 
참조 : 
    https://dsbook.tistory.com/220?category=802614 , 
    https://076923.github.io/posts/Python-opencv-21/

Contours : 동일한 색 또는 동일한 픽셀값(강도, intensity)을 가지고 있는 영역의 경계선 정보

cv2.findContours() : contour 정보와 Contour 계층구조(hierarchy) 출력 (흑백이미지, 이진화된 이미지에만 적용가능) 


'''

img = cv2.imread('test12.PNG')
img2 = img.copy()
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(img_gray, 127, 255, cv2.THRESH_BINARY_INV)
'''
윤곽선(컨투어)를 검출하는 주된 요소는 하얀색의 객체를 검출
그러므로 배경은 검은색이며 검출하려는 물체는 하얀색의 성질을 띄게끔 변형
이진화 처리 후, 반전시켜 검출하려는 물체를 하얀색의 성질을 띄도록 변환

threshold(img, threshold_value, value, flag) -> 픽셀값이 127보다 크면 ,0(검은색) 작으면 value(255(흰색))으로 할당
'''


contours, hierarchy = cv2.findContours(thresh, cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
'''
cv2.RETR_EXTERNAL : 외곽 윤곽선만 검출하며, 계층 구조를 구성하지 않습니다.
cv2.CHAIN_APPROX_SIMPLE : 윤곽점들 단순화 수평, 수직 및 대각선 요소를 압축하고 끝점만 남겨 둡니다.
'''

cv2.drawContours(img, contours, -1, (255,0, 0), 1)
# 윤곽석 검출



# 무게중심(중심점) 구하기 moment
'''
폐곡선 객체의 무게중심, 객체의 면적등의 특성을 계산

공간 모멘트 
m00, m01, m10, m11, m20, m02, m30, m21, m12, m03

중심 모멘트
mu20, mu11, mu02, mu30, mu21, mu12, mu03

평준화된 중심 모멘트 
nu20, nu11, nu02, nu30, nu21, nu03

여기서 m00은 폐곡선의 면적을 뜻한다.

'''
c0 = contours[0]
M = cv2.moments(c0)
print(M.items())

plt.title('Contour')
plt.imshow(img)
plt.axis('off')

img_copy = img.copy()
cx = M['m10']/M['m00']
cy = M['m01']/M['m00']
cv2.circle(img_copy, (int(cx),int(cy)), 2, (255, 0, 0), -1)

print(f'중심 좌표 : {cx:.3f} , {cy:.3f}')

plt.imshow(img_copy)
plt.title("Contour Center")
plt.axis("off")
plt.show()

```
![image](https://user-images.githubusercontent.com/74512114/149887917-4619154f-018d-4cbb-bbc1-853a16b1f609.png)

<hr> 

```python


import cv2
import matplotlib.pyplot as plt

'''
✔ 객체 외각을 접하는 외접원의 중심점 산출
(컨투어 라인을 완전히 포함하는 가장 작은 원)

참조 : https://datascienceschool.net/03%20machine%20learning/03.02.03%20%EC%9D%B4%EB%AF%B8%EC%A7%80%20%EC%BB%A8%ED%88%AC%EC%96%B4.html
'''

img = cv2.imread('test05.PNG')
imgray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

ret, thr = cv2.threshold(imgray, 127, 255, cv2.THRESH_BINARY)
#픽셀값이 급격히 변하는 구간 이진화

contours, hierarchy = cv2.findContours(thr, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)
'''
cv2.findContours : 이미지의 컨투어 정보, 컨투어의 상하구조/계층(hierachy) 정보를 출력
cv2.RETR_TREE : 모든 컨투어 라인을 찾음
cv2.CHAIN_APPROX_SIMPLE : 윤곽점들 단순화 수평, 수직 및 대각선 요소를 압축하고 끝점 남김
'''

# print(contours)
# print()
# print(hierarchy)
con = contours[1]

(x, y), r = cv2.minEnclosingCircle(con)
print(x,y)
cv2.circle(img, (int(x), int(y)), int(r), (0, 0, 255), 1)

img_copy = img.copy()

cv2.circle(img_copy, (int(x), int(y)), 1, (0, 0, 255), -1)
print(f'중심 좌표 : {x:.4f} , {y:.4f}')
print(x,y)

plt.imshow(img_copy)
plt.title('EnclosingCircle')
plt.axis("off")
plt.show()

```
![image](https://user-images.githubusercontent.com/74512114/149888019-6cf67d2c-663e-46db-a096-d8951c212c07.png)


