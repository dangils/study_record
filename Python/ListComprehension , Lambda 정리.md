## List Comprehension
> list 변수 내부에 for 구문을 이용해서 데이터 생성  
> 머신러닝 데이터 조작시 자주 사용하는 기법

```python
list_data=[ x**2 for x in range(5)] 
# [0, 1, 4, 9, 16]

list2 = [x for x in range(1,11,2)]
# [1, 3, 5, 7, 9]

raw_data = [[1,10],[2,20],[3,30],[4,40]]

raw1 = [x[0] for x in raw_data]
# [1, 2, 3, 4]

raw2 = [x[1] for x in raw_data]
# [10, 20, 30, 40]

```

## Lambda 함수
> 함수를 만들 때 def를 사용하지 않고 한 줄 형태로 작성 하는 것  
> 머신러닝에서 수치 미분(Numerical Derivative)과 활성화 함수(Activation Function) 등을 수학에서의 함수 표기법 f(x), f(x,y)로 나타낼때 사용

```python
f=lambda x : x+100

for i in range(3):
    print(f(i))
  
100
101
102
```
→ lambda를 대표하는 함수 이름(f), 입력 변수(x) 그리고 이를 대체(substitution)할 수 있는 표현식(Expression)으로 정의 할 수 있다.
