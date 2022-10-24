# 개발일기
## 2022년 10월 24일 월요일
### 오늘 배운 내용들(변수,조건문 위주로 공부)
#### late, final&const, if(조건문)

#### late이란?
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
                      
#### final&const 이란?
final은 런타임(run-time)에, const는 컴파일타임(compile-time)에 초기화(initialize) 된다.
그러므로 final과 const는 최종 값을 갖는 변수 이지만 사용되는 상황이 다르다. 
값이 컴파일 단계에서 결정될 경우 const/런타임 단계에서 결정될 경우 final을 사용한다.

- _final&const 공통점
- 선언과 초기화가 동시에 발생 
  ex) finalconst name = 'Bob';

- 초기화된 값은 변경 불가능 
  ex) finalconst name = 'Bob';
  name = 'Alice'; //Error: a final variable can only be set once. / Error: Constant variables can't be assigned a value

- _final%const 차이점 
- final
  클래스의 인스턴스를 할당 가능
  값이 객체(Object)인 경우, 안의 요소는 변경 가능 
 
- const
  클래스의 인서튼스를 할당 불가
  값이 객체(Object)인 경우, 안의 요소도 변경 불가능 

#### if(조건문)

- if문
  if문은 주어진 조건에 따라서 실행할 문장이 다를 때 사용한다.
  if문은 단순 if문과 if~else을 사용한다.

-           if (loginStatus == AuthStatus.loginSuccess) {
              ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text(authClient.user!.email! + '님 환영합니다!')),
                );
              Navigator.push(context,
                  MaterialPageRoute(builder: (context) => ListScreen()));
            } else {
              ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text('로그인에 실패했습니다. 다시 시도해주세요.')),
                );
               }
            });
          },
          child: Text('로그인'),
 
 - if (loginStatus == AuthStatus.loginSuccess) 두개의 값이 일치(true)일때 
 
 -          ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text(authClient.user!.email! + '님 환영합니다!')),
                );
              Navigator.push(context,
                  MaterialPageRoute(builder: (context) => ListScreen()));
                  
 스낵바가 생성 되면서 ListScreen으로 이동
                  
 - if (loginStatus == AuthStatus.loginSuccess) 두개의 값이 실패(false)일때 

 -          ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text('로그인에 실패했습니다. 다시 시도해주세요.')),
                );
               }
            });
          },
          child: Text('로그인'),
          
스낵바가 생성 되면서 로그인으로 스크린으로 다시 이동

        
