### generator
- 제너레이터는 반복자(iterator)와 같은 루프의 작용을 컨트롤하기 위해 쓰여지는 특별한 함수 또는 루틴
- 제너레이터는 배열이나 리스트를 리턴하는 함수와 비슷하며, 호출을 할 수 있는 파라메터를 가지고 있고, 연속적인 값들을 만들어 낸다.
- 하지만 한번에 모든 값을 포함한 배열을 만들어서 리턴하는 대신 yield 구문을 이용해 한번 호출될 때마다 하나의 값만을 리턴하고, 이러한 이유로 일반 반복자에 비해 아주 작은 메모리를 필요로 한다.  
- yield 값 : yield를 사용하면 함수는 제너레이터가 되며 yield에는 값(변수)를 지정

```python
def number_generator():
    yield 0
    yield 1
    yield 2
    
for i in number_generator():
    print(i)
    
g = number_generator()

print(dir(g))

```

------

```python
'''
List 예제
'''
from __future__ import division
import os
import psutil
import random
import time

names = ['최용호', '지길정', '진영욱', '김세훈', '오세훈', '김민우']
majors = ['컴퓨터 공학', '국문학', '영문학', '수학', '정치']

process = psutil.Process(os.getpid())
mem_before = process.memory_info().rss / 1024 / 1024


def people_list(num_people):
    result = []
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(majors)
        }
        result.append(person)
    return result


def people_generator(num_people):
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(majors)
        }
        yield person

t1 = time.time()
people = people_list(1000000)  # 1 people_list를 호출
t2 = time.time()
mem_after = process.memory_info().rss / 1024 / 1024
total_time = t2 - t1

print('시작 전 메모리 사용량: {} MB'.format(mem_before))
print('종료 후 메모리 사용량: {} MB'.format(mem_after))
print('총 소요된 시간: {:.6f} 초'.format(total_time))

'''
시작 전 메모리 사용량: 64.33203125 MB
종료 후 메모리 사용량: 334.73828125 MB
총 소요된 시간: 0.812184 초
'''


'''
Generator 예제
'''
from __future__ import division
import os
import psutil
import random
import time

names = ['최용호', '지길정', '진영욱', '김세훈', '오세훈', '김민우']
majors = ['컴퓨터 공학', '국문학', '영문학', '수학', '정치']

process = psutil.Process(os.getpid())
mem_before = process.memory_info().rss / 1024 / 1024


def people_list(num_people):
    result = []
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(majors)
        }
        result.append(person)
    return result


def people_generator(num_people):
    for i in range(num_people):
        person = {
            'id': i,
            'name': random.choice(names),
            'major': random.choice(majors)
        }
        yield person

t1 = time.time()
people = people_generator(1000000)  # 1 people_generator를 호출
t2 = time.time()
mem_after = process.memory_info().rss / 1024 / 1024
total_time = t2 - t1

print('시작 전 메모리 사용량: {} MB'.format(mem_before))
print('종료 후 메모리 사용량: {} MB'.format(mem_after))
print('총 소요된 시간: {:.6f} 초'.format(total_time))

'''
시작 전 메모리 사용량: 64.359375 MB
종료 후 메모리 사용량: 64.359375 MB
총 소요된 시간: 0.000000 초
'''


```

✔ 이로써 알 수 있는 사실은 실행 시간 보다 메모리 소비를 줄여야 하는 경우라면 제너레이터를 사용해야하고,리소스 보다 실행 시간을 줄여야하는 경우라면 리스트를 사용해야한다고 볼 수 있겠습니다.    
하지만 위의 데이터 보다 훨씬 더 많은 양의 데이터를 병행으로 동시에 처리해야 한다고 했을 때 약간의 시간을 줄이기 보다는 한정적인 리소스를 효율적으로 사용해야 하는 경우가 대부분이라고 생각합니다.
