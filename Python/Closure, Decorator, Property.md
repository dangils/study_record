## First Class Function
- 프로그래밍 언어가 함수(function)을 first-class citizen으로 취급하는 것
- 함수 자체를 인자(argument)로써 다른 함수에 전달하거나 다른 함수의 결과값으로 리턴할수도 있고,    
함수를 변수에 할당하거나 데이터 구조안에 저장할 수 있는 함수   

```python

'''First Class Function
변수에 함수를 할당할 수 있을뿐만 아니라, 인자로써 다른 함수에 전달하거나, 함수의 리턴값으로도 사용
'''
def square(x):
    return x * x

f = square

print(f(5))

```

## Closure
- 클로저란 퍼스트클래스 함수를 지원하는 언어의 네임 바인딩 기술  
- 어떤 함수를 함수 자신이 가지고 있는 환경과 함께 저장한 레코드, 함수가 가진 free variable(프리변수)를 클로저가 만들어지는 당시의 값과 레퍼런스에 맵핑하여 주는 역할
  
* free variable 이란?  
파이썬에서 프리변수는 코드블럭안에서 사용은 되었지만, 그 코드블럭안에서 정의되지 않은 변수  


```python
def outer_func(): # 1
    message = 'Hi' # 3
    
    def inner_func(): # 4
        print(message) # 6
    
    #return inner_func() # 5
    return inner_func() # 함수 오브젝트를 리턴

my_func = outer_func() # 리턴값인 inner_func를 변수에 할당

#my_func() # 2

#outer_func() 

# print(outer_func())
# print(my_func())

#=========================================

def outer_func(): # 1
    message = 'Hi' # 3

    def inner_func(): # 4
        print(message)  # 6

    return inner_func # 5

my_func = outer_func() # 2

my_func() # 7
'''
✔ my_func()는 로컬 변수인 message를 참조했다. -> 클로저로 참조(클로저는 함수의 프리변수 값을 저장) 
'''

```
-----------

```python
 def outer_func():  # 1
    message = 'Hi'  # 3

    def inner_func():  # 4
        print(message)  # 6

    return inner_func  # 5

my_func = outer_func()  # 2

print(f'my_func : {my_func} \n')  # 7
print(f'dir(my_func) : {dir(my_func)} \n')  # 8
print(f'type(my_func.__closure__) : {type(my_func.__closure__)} \n')  # 9
print(f'my_func.__closure : {my_func.__closure__} \n')  # 10
print(f'my_func.__closure__[0] : {my_func.__closure__[0]} \n')  # 11
print(f'dir(my_func.__closure__[0]) : {dir(my_func.__closure__[0])} \n')  # 12
print(f'my_func.__closure__[0].cell_contents : {my_func.__closure__[0].cell_contents} \n')  # 13

'''
#7. my_func -> 함수 오브젝트 할당
#8. my_func 안에 __closure__가 숨겨져 있음
#9. __closure__는 튜플 타입
#10. __closure__에 아이탬이 하나 들어있음
#11. 첫번째 아이탬은 "cell" 이라는 문자열 오브젝트
#12. __closure__의 cell 오브젝트에 "cell_contents"라는 속성이 존재
#13. cell_contents에 데이터가 저장되어 있음

'''
my_func : <function outer_func.<locals>.inner_func at 0x0000025C2E490670> 

dir(my_func) : ['__annotations__', '__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__get__', '__getattribute__', '__globals__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__', '__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__'] 

type(my_func.__closure__) : <class 'tuple'> 

my_func.__closure : (<cell at 0x0000025C2E3E75B0: str object at 0x0000025C2E38F7B0>,) 

my_func.__closure__[0] : <cell at 0x0000025C2E3E75B0: str object at 0x0000025C2E38F7B0> 

dir(my_func.__closure__[0]) : ['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'cell_contents'] 

my_func.__closure__[0].cell_contents : Hi 
'''
```
----

### Decorator  
- 기존의 코드에 여러가지 기능을 추가하는 파이썬 구문 

```python
'''
데코레이터 함수가 wrapper 되어 실행됨
'''
def decorator_function(original_function):
    def wrapper_function():
        print(f'{original_function.__name__} function before')
       # print('{} 함수가 호출되기전 입니다.'.format(original_function.__name__))
        return original_function()

    return wrapper_function

@decorator_function # 1
def display_1():
    print('display_1 function')

@decorator_function # 2
def display_2():
    print('display_2 function')
    
# display_1 = decorator_function(display_1) # 3
# display_2 = decorator_function(display_2) # 4

display_1()
print()
display_2()

'''
display_1 function before
display_1 function

display_2 function before
display_2 function
'''
```
-----

### @property  
파이썬에서 기본 제공하는 연산자로 private한 attribute를 다룰 수 있다.

```python
class Person:
    def __init__(self, first_name, last_name, age):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, age):
        if age < 0:
            raise ValueError("Invalid age")
        self._age = age

'''
@property 를 사용했을 때 가장 큰 이점은 외부에 티 내지 않고 내부적으로 클래스의 필드 접근 방법을 바꿀 수 있다는 것
'''

hangil = Person("han",'lee',25)
print(hangil.age)

hangil.age +=1

print(hangil.age) 
#25
#26

#===============================================

# @property 활용

class Person :
    def __init__(self,first_name, last_name, age):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age

    @property
    def full_name(self):
        return self.first_name + " " + self.last_name

'''
클래스를 작성하다보면 다른 필드로 부터 값이 유추되는 읽기 전용 필드가 필요할 때가 있다. 이때 @propety를 통해 필드를 추가해줄 수 있다.
'''


```
