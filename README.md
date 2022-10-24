# 개발일기
## 2022년 10월 24일 월요일
### 오늘 배운 내용들(변수,조건문 위주로 공부)
#### late&final&if(조건문)&String

##### late이란?
다트에서는 late변수를 제공한다. late변수를 사용하면 non-nullable변수의 초기화를 나중에 할 수 있다. 헌데 한가지 의문점이 있다. 변수의 자료형을 nullable로 선언해도 나중에 초기화를 할 수 있으니 그냥 nullable을 쓰면 되지 왜 굳이 구글은 late라는 키워드를 만들었을까? 결론부터 말하면
late대신 nullable로 선언할 경우, 개발자가 다른 사람에게 코드의 관리를 넘겼을 때, 넘겨받은 관리자가 null이라는 값도 변수에 의미있는 값으로 오해할 수 있기 때문이다.
-           //Using null safety, incorrectly:
            class Coffee {
                String _temperature;

                void heat() { _temperature = 'hot'; }
                void chill() { _temperature = 'iced'; }

                String serve() => _temperature + ' coffee';
               }

               main() {
                var coffee = Coffee();
                coffee.heat();
                coffee.serve();
               }
    
이 코드는 에러가 난다. non nullable인 _temperature가 선언에도 초기화 되지 않고, 생성자에서도 초기화 되지 않았기 때문이다.
        
-           //Using null safey:
            class Coffee {
              late String _temperature;
              
              void heat() { _temperature = 'hot'; }
              void chill() { _temperature = 'iced'; }
              
              String serve() => _temperature + 'coffee';
             }
             
이를 방지하기 위해서 late라는 키워드를 사용하는 것이다. 구글에서는 late키워드에 대해 이렇게 말한다.
 
The late modifier lets you defer initaization, but still prohibits you from treating it like a nullable variable.

late키워드는 값의 초기화를 뒤로 미루지만, 개발지가 null을 실수로 사용하는것을 막아준다.
                      
