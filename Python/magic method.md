### 파이썬 magic method

1. 매직 메소드란  
클래스안에 정의할 수 있는 스페셜 메소드이며, 클래스를 int,str,list등의 파이썬 빌트인 타입(built-in type)과 같은 작동을 하게 해준다.

2. __init__이란
초기화 메서드라고 불리며 객체가 생성될 때 여러가지 성질을 갖게 하는 기능

```python

# Food 객체생성시 name,price 성질을 갖게 한다
class Food(object):
  def __init__(self, name, price):
    self.name = name
    self.price = price
    
food_1 = Food('아이스크림',3000)
print(food_1)  # <__main__.Food object at 주소>

# __str__ 메소드를 사용하지 않고 객체를 프린트하면 객체의 주소만 불러오게 된다.


# __str__ : 객체 정보를 문자열로 반환

class Food(object):
    def __init__(self, name, price):
        self.name = name
        self.price = price
        
    def __str__(self):
        return '아이템: {}, 가격: {}'.format(self.name, self.price)

food_1 = Food('아이스크림', 3000)

# 인스턴스 출력
print(food_1) # 아이템: 아이스크림, 가격: 3000
```
