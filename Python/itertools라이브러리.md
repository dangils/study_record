## itertools : 효율적인 루핑을 위한 이터레이터를 만드는 함수 라이브러리


```python
'''
combinations(iterable, r) : interable에서 원소 갯수가 r개인 조합 뽑기
'''

from itertools import combinations

ar = [1,2,3]

for i in combinations(ar,2):
    print(i)
    
(1, 2)
(1, 3)
(2, 3)

'''
combinations_with_replacement : iterable에서 원소 개수 r개인 중복 조합 뽑기
'''
from itertools import combinations_with_replacement

ar = ['a','b','c']

for i in combinations_with_replacement(ar,2):
    print(i)
    
    
('a', 'a')
('a', 'b')
('a', 'c')
('b', 'b')
('b', 'c')
('c', 'c')


'''
permutations(iterable,r=None) : iterable에서 원소 개수 r개인 순열 뽑기
'''

from itertools import permutations

ar = ['a','b','c']

for i in permutations(ar): #r을 지정하지 않거나 r=None으로 하면 최대 길이의 순열이 리턴된다
    print(i)
    

('a', 'b', 'c')
('a', 'c', 'b')
('b', 'a', 'c')
('b', 'c', 'a')
('c', 'a', 'b')
('c', 'b', 'a')


'''
product(*iterables, repeat=1) : *iterables의 모든 쌍을 지어 리턴(데카르트곱 리턴)
- product는 다른 함수와 달리 인자로 여러 iterable을 넣어줄 수 있고, 그 친구들간의 모든 짝을 지어서 리턴한다.
'''

from itertools import product
ar1 = ['a','b']
ar2 = [1,2]

for i in product(ar1,ar2,repeat=1): # ar1과 ar2의 모든 쌍을 지어 리턴한다. repeat=1 생략 가능
    print(i)
    
for i in product(ar1, repeat = 3): # product(ar1,ar1,ar1,repeat = 1) 과 동일
    print(i)
    
    
 ('a', 1)
('a', 2)
('b', 1)
('b', 2)
('a', 'a', 'a')
('a', 'a', 'b')
('a', 'b', 'a')
('a', 'b', 'b')
('b', 'a', 'a')
('b', 'a', 'b')
('b', 'b', 'a')
('b', 'b', 'b')

    
    
    
    
    
```
