### Harris 코너 검출
Harris 코너 검출은 Sobel 필터로 edge를 찾아낸 다음, Gradiant 변화량을 측정해서 x축, y축으로 동시에 급격하게 변화한 지점을 코너로 판단

```python
import cv2
import numpy as np
img = cv2.imread('C:/Users/you02/YTS_AI_study/python/prac/siling_test/test01.png')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# Harris corner 검출
corner = cv2.cornerHarris(gray,2,3,0.05)
img[corner > 0.01 * corner.max()] = [0,255,0]

corner_norm = cv2.normalize(corner, None, 0, 255, cv2.NORM_MINMAX, cv2.CV_8U)

cv2.imshow('Harris',img)
cv2.imshow('corner',corner_norm)
cv2.waitKey()
cv2.destroyAllWindows()
```

![image](https://user-images.githubusercontent.com/74512114/149733082-a64c8bd4-856d-450e-a3f2-42310e46fb5b.png)

<hr>

### FAST 코너 검출
- 본래 명칭은 Feature from Accelerated Segment Test이며, 이름과 같이 feature를 검출하는 속도를 개선한 것이다.
- 코너 판단 여부는 pixel을 중심으로 원을 그려서 임계값을 기준으로 특정 개수 이상이 밝거나 어두움이 연속될 때를 기준으로 결정된다.
- FAST 검출기를 생성하는 cv2.FastFeatureDetector_create()는 임계값, [코너 억제 여부, edge 검출 패턴]을 인자로 사용
-  선명한 edge를 찾기 위해서 코너 억제 여부의 default는 True로 설정된다. edge 검출 패턴은 해당 pixel 주변의 16개에 대해 9개의 pixel이 임계를 만족하는지에 대해 비교하는 cv2.FastFeatureDetector_TYPE_9_16이 default로 적용


```python
import cv2
import numpy as np

img = cv2.imread('C:/Users/you02/YTS_AI_study/python/prac/siling_test/test02.png')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# FAST 검출기 생성
fast = cv2.FastFeatureDetector_create(50)
#  cv2.FastFeatureDetector_create()는 임계값, [코너 억제 여부, edge 검출 패턴]을 인자로 사용

# keypoint 검출 및 그리기
point = fast.detect(gray, None)
draw = cv2.drawKeypoints(img, point, None)
cv2.imshow('FAST',draw)

cv2.waitKey()
cv2.destroyAllWindows()
```

![image](https://user-images.githubusercontent.com/74512114/149733246-4926bbcf-37c8-4ff8-b770-a761b29642b2.png)

<hr>

### SimpleBlobDetector
- BLOB이란 Binary Large Object의 줄임말로, 이진 이미지의 연결된 pixel 그룹을 의미한다. 
- 작은 객체는 Noise로 판단해서 무시하고, 특정 크기 이상의 객체에만 관심을 가진다. 
- cv2.SimpleBlobDetector_create()로 BLOB 검출기를 생성한다. 
- 아무 인자 없이 BLOB 검출을 할 수도 있지만, cv2.SimpleBlobDetector_Params()로 필요한 옵션만 골라서 검출할 수도 있다.

```python

'''
옵션 인자 없이 BLOB검출
'''

import cv2
import numpy as np

img = cv2.imread('C:/Users/you02/YTS_AI_study/python/prac/siling_test/test02.png')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)


# # BLOB 검출기 생성=============
sbd = cv2.SimpleBlobDetector_create()

point = sbd.detect(gray, None)
draw = cv2.drawKeypoints(img, point, None, (0,0,255), flags = cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)

cv2.imshow('SBD', draw)
#=============================


cv2.waitKey()
cv2.destroyAllWindows()

```
![image](https://user-images.githubusercontent.com/74512114/149733349-be4fa0bb-bdad-4516-b37e-4014aaea7831.png)


```python
'''
옵션 인자 설정
'''
import cv2
import numpy as np

img = cv2.imread('C:/Users/you02/YTS_AI_study/python/prac/siling_test/test02.png')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

sbd = cv2.SimpleBlobDetector_create()
point = sbd.detect(gray, None)

# params 추가
params = cv2.SimpleBlobDetector_Params()
params.maxThreshold = 200
params.filterByConvexity = False

sbd = cv2.SimpleBlobDetector_create(params)
point = sbd.detect(gray, None)
draw = cv2.drawKeypoints(img, point, None, (0,255,0), flags = cv2.DRAW_MATCHES_FLAGS_DRAW_RICH_KEYPOINTS)
cv2.imshow('SBD', draw)

cv2.waitKey()
cv2.destroyAllWindows()


```
![image](https://user-images.githubusercontent.com/74512114/149733395-da976b7d-d852-4dfa-959a-19eca2753616.png)

![image](https://user-images.githubusercontent.com/74512114/149733406-f4723c40-b438-40f9-9e0c-af36bac7c4cd.png)

![image](https://user-images.githubusercontent.com/74512114/149733425-22a2ead2-3a16-41b6-a56e-ef39ee78ab02.png)
