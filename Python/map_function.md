## 파이썬의 Map 함수  

형태 : map(function, irerable)
첫 번째 매개변수로는 함수가 오고,   
두 번째 매개변수로는 반복 가능한 자료형(리스트,튜플 등)이 옵니다.
>map 함수의 반환 값은 map 객체 이기 때문에 해당 자료형을 list 혹은 tuple로 형 변환 시켜주어야 합니다.

함수의 동작 : 두 번째 인자로 들어온 반복 가능한 자료형(리스트나 튜플)을 첫 번째 인자로 들어온 함수에 하나씩 집어 넣어 수행

``` python
# 리스트에 값을 하나씩 더해서 새로운 리스트를 만드는 작업 
myList = [1, 2, 3, 4, 5]   

# for 반복문 이용  
result1 = [] 
for val in myList:
  result1.append(val+1)
print(f'result1: {result1}')

# map 함수 이용
def add_one(n):
  return n+1
result2 = list(map(add_one,myList)) # map 반환을 list로 변환
print(f'result2 : {result2}')

```
map 함수의 첫번째 인자를 자동적으로 두번째 인자(리스트)에 적용 시켜 map 객체를 반환,  
반환된 객체를 list로 형 변환하여 사용

