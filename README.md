# 개발일기
## 2022년 10월 24일 월요일
### 오늘 배운 내용들
#### late&final 
##### late이란?
-다트에서는 late변수를 제공한다. late변수를 사용하면 non-nullable변수의 초기화를 나중에 할 수 있다. 헌데 한가지 의문점이 있다. 변수의 자료형을 nullable로 선언해도 나중에 초기화를 할 수 있으니 그냥 nullable을 쓰면 되지 왜 굳이 구글은 late라는 키워드를 만들었을까? 결론부터 말하면
 late대신 nullable로 선언할 경우, 개발자가 다른 사람에게 코드의 관리를 넘겼을 때, 넘겨받은 관리자가 null이라는 값도 변수에 의미있는 값으로 오해할 수 있기 때문이다.
