## SOLID 원칙 
객체지향 설계 원칙
<br> 

1) Single Responsibility Principle  
- 하나의 클래스는 하나의 역할만을 수행
<br> 

2) Open - close Principle
- 확장(상속)에는 열려있고, 수정에는 닫혀 있어야 함
<br> 

3) Liskov Substitution Principle  
- 자식이 부모의 자리에 항상 교체될 수 있어야 한다  
- 부모의 property / method 모두를 자식이 가지고 있어야 함
<br> 

4) Interface Segregation Principle
- 인터페이스가 잘 분리되어 클래스가 꼭 필요한 인터페이스만 구현이 되어있음
<br> 

5) Dependency Inversion Property
- 상위 모듈이 하위 모둘에 의존하면 안됨
- 메타 클래스 / 추상화 클래스에 의존(필요하면), 추상화 자체는 세부 사항에 의존하면 안됨

<br> 

## Design Pattern
잘 설계된 구조의 형식적 정의. 다양한 디자인 패턴으로 서로 다른 문제를 해결 가능

<br> 

#### 디자인 패턴이 필요한 이유
- 중복되는 코드 개발을 하고 싶지 않을 때 "바퀴를 다시 발명하지 마라"
- 업무의 분리를 위해, 개념을 압축해 효율적인 커뮤니케이션 가능
- 유지보수 용이 / 객체지향 기술 향상 -> OOP 기술의 문제점에 대한 해결책 제시

<br> 

![image](https://user-images.githubusercontent.com/74512114/147792109-dfc936a4-095a-4db1-82d4-bd42f919d79d.png)

<br> 

#### 디자인 패턴의 구조
- 맥락(context) :  
문제가 발생하는 여러 상황을 기술한다. 즉, 패턴이 적용될 수 있는 상황을 나타낸다. 경우에 따라서는 패턴이 유용하지 못한 상황을 나타내기도 한다.

<br> 

- 문제(problem) :    
패턴이 적용되어 해결될 필요가 있는 여러 디자인 이슈들을 기술한다. 이때, 여러 제약 사항과 영향력도 문제 해결을 위해 고려해야 한다.

<br> 

- 해결(solution) :   
문제를 해결하도록 설계를 구성하는 요소들과 그 요소들 사이의 관계, 책임, 협력 관계를 기술한다.  
해결은 반드시 구체적인 구현 방법이나 언어에 의존적이지 않으며 다양한 상황에 적용할 수 있는 일종의 템플릿이다. 




https://realzero0.github.io/study/2017/06/12/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%EC%A0%95%EB%A6%AC.html     

https://blog.naver.com/PostView.naver?blogId=dsz08082&logNo=222177413963&parentCategoryNo=18&categoryNo=81&viewDate=&isShowPopularPosts=false&from=postView     

https://salix97.tistory.com/201      


https://www.fun-coding.org/PL&OOP2-2.html [파이썬 OOP]    




