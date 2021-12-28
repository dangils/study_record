## 파이썬 with ~ as 구문
with as는 파일 열고 > 쓰고 > 닫고를 알아서 자동으로 해준다


### 사용법  
* with [expression] as [변수명]

```python
with open('textfile.txt', 'r') as file:
    contents = file.read()
    

# 위 구문과 동일한 내용
file = open('textfile.txt', 'r')
contents = file.read()
file.close()

```

> 두번째 코드를 실행한다고 할때 만약에 content = file.read() 에서 에러가 난다면 file 을 닫을 수 없다.  
>  그러나 with 문을 사용하면 에러가 나더라도 file 을 자동으로 닫아준다.


#### with을 class에 응용하기 (파이썬 Magic Mathod)
- open 함수 이외의 사용이 끝난 객체를 종료시킨다
- with 구문의 표현식을 open함수 이외에 사용자가 임의로 정의한 클래스 객체로 정의, 사용시 class 내부 메소드에 __enter__(),__exit__() 메소드에 대한 내용을 입력해야 한다. 
- 객체가 실핼될 때 __enter__()가 자동으로 호출, 종료 될때 __exit__()를 호출

```python
class Withest:
  #초기화 메서드 정의
  def __init__(self):
    self.temp = "초기화 메서드"
  
  #객체가 호출될 때 자동으로 실행
  def __enter__(self):
    print(self.temp)
    return self # 반환값이 있어야 Variable을 블록내에서 사용할 수 있다
  
  def printText(self):
    print("메소드 호출 테스트")
   
   #with 구문 통해 객체가 종료될 때 자동으로 실행
   def __exit__(self, exc_type, exc_val, exc_tb): 
    #exc_type, exc_val, exc_tb는 with 문을 빠져나오기 이전에 예외가 발생했을 때를 나타내는 정보
    
    #종료 구문 설정하기(세션의 종료 등)
    print("with 통해 종료")
# 객체 생성 및 with 구문 실행
if __name__ = "__main__":
  withclass = Withest()
  
  with withclass as wc:
    wc.printText()

```
결과 :  
초기화 메서드  
메소드 호출 테스트  
with 통해 종료  



