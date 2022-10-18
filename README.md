#  개발일기
## 2022년 10월 18일 화요일
### provider&Container book앱 분석

#### 오늘 배운 내용들 

##### Container&Row&Column
- _Container는 한 개의 자식을 갖는 레이아웃 위젯입니다.

-     Container({
        Key key,
        this.alignment,
        this.padding,
        Color color,
        Decoration decoration,
        this.foregroundDecoration,
        double width,
        double height,
        BoxConstraints constraints,
        this.margin,
        this.transform,
        this.child,
       })
 
-       padding은 Container 내부의 공간을 의미하고 Color는 배경색을 의미합니다. margin은 Container 외부의 공간을 의미합니다.

- _Row는 "행" 이라고 한다. 가로 방향 집합을 의미한다.
- _Column는 "열"이라고 한다. 세로 방향 집합을 의미한다.

#### provider는 하나의 상태조각의 압축(encapsulates)된 객체이자 상태의 변화를 감시하는 역할을 가지고 있다. StateLessWidget(상태가 없는) StateFulWidget(상태가 있는)


####  Provider.of<FirebaseAuthProvider>(context, listen: false);  부모의 FirebaseAuthProvider를 찾을려면

- My app 
- provider 는  Materialapp 부모노드  자식노드에게 provider를 연결할 수 있다. 
- MaterialApp 
- LoginScreen
- body
- LoginButton 
- Provider.of<FirebaseAuthProvider>(context, listen: false);

#### final loginField = Provider.of<LoginFieldModel>(context, listen: false); 부모의 LoginFielModel를 찾을려면 

- My app 
- MaterialApp
- LoginScreen
- 부모의 provider LoginFielModel 
- return ChangeNotifierProvider(   create: (_) => LoginFielModel 
- body
- LoginButton
- final loginField = Provider.of<LoginFieldModel>(context, listen: false);

  #### ChangeNotifierProvider를 통해 변화에 대해 구독한다. (1개만 구독이 가능하다. 여러 개를 구독하기 위해서는 Muitprovider로 감싼 후 사용해야된다.)
  - builder: (context) => MultiProvider(providers: [
  -     ChangeNotifierProvider(create: (context) => FirebaseAuthProvider()),
  -           ChangeNotifierProvider(create: (context) => LoginFieldModel()),
  
 #### book앱으로 main.dart에서 login.screen.dart까지 동작원리 

  - main.dart 처음 실행됬을때 main함수가 시작이 되고 
  -     void main() async {
          WidgetsFlutterBinding.ensureInitialized();
          await Firebase.initializeApp();
          runApp(MyApp());
      }

  - Myapp statelessWidget 위젯이 실행이 되면서 텍스트 가 나오게됨 ('book app') 그리고 SplashScreen으로 이동이됨
  -     class MyApp extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            return ChangeNotifierProvider(create: (context) => FirebaseAuthProvider(),
            child: MaterialApp(
              title: 'Book App',
              home: SplashScreen(),
            ));
          }
        }
  
  - 상태가 있는 SplashScreen을 선언과 동시에 생성을 한다.
  -     class SplashScreen extends StatefulWidget {
          @override
          _SplashScreenState createState() => _SplashScreenState();
        }
  
  - 먼저 실행되는 initState 는 2초라는 moveScreen이 띄워진다.
  -       @override
          void initState() {
            super.initState();
            Timer(Duration(seconds: 2), () {
              moveScreen();
            });
          }
  
  - moveScreen이 실행되고 다음 텍스쳐가 띄워진다.(bear story book)
  -     @override
        Widget build(BuildContext context) {
          return Scaffold(
            body: Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(
                    'bear story book',
                    style: TextStyle(fontSize: 20),
                  ),
                  Text(
                    '나만의 도서 : 나만의 도서목록 앱',
                    style: TextStyle(fontSize: 20),
                  ),
                ],
              ),
            ),
          );

  - 로그인이 true가 되었을때 ListScreen으로 이동
  -     void moveScreen() async {
          checkLogin().then((isLogin) {
            if (isLogin) {
              Navigator.of(context).pushReplacement(
                MaterialPageRoute(
                  builder: (context) => ListScreen(),
                ),
              );
  - 로그인이 false가 되었을때 LoginScreen으로 이동
  -     Navigator.of(context).pushReplacement(
          MaterialPageRoute(
            builder: (context) => MultiProvider(providers: [
              ChangeNotifierProvider(create: (context) => LoginFieldModel()),
            ], child: LoginScreen(),),
          ),
        );

  - 로그인 스크린 위젯안에 뼈대를 만들고 이메일&비밀번호&로그인버튼&패딩&회원가입버튼을 인풋을 한다.
  -     class LoginScreen extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            return ChangeNotifierProvider(
              create: (_) => LoginFieldModel(),
              child: Scaffold(
                appBar: AppBar(
                  title: Text("로그인 화면"),
                ),
                body: Column(
                  children: [
                    EmailInput(),
                    PasswordInput(),
                    LoginButton(),
                    Padding(
                      padding: EdgeInsets.all(8.0),
                      child: Divider(thickness: 1),
                    ),
                    RegisterButton(),
                  ],
                ),
              ),
            );
          }
        } 
      
  - 이메일 위젯 생성&인증
  -     class EmailInput extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final loginField = Provider.of<LoginFieldModel>(context, listen: false);
            return Container(
              padding: EdgeInsets.fromLTRB(20, 5, 20, 5),
              child: TextField(
                onChanged: (email) {
                  loginField.setEmail(email);
                },
                keyboardType: TextInputType.emailAddress,
                decoration: InputDecoration(
                  labelText: '이메일',
                  helperText: '',
                ),
              ),
            );
          }
        }

  - 비밀번호 &인증
  -     class PasswordInput extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final loginField = Provider.of<LoginFieldModel>(context, listen: false);
            return Container(
              padding: EdgeInsets.fromLTRB(20, 5, 20, 5),
              child: TextField(
                onChanged: (password) {
                  loginField.setPassword(password);
                },
                obscureText: true,
                decoration: InputDecoration(
                  labelText: '비밀번호',
                  helperText: '',
                ),
              ),
            );
          }
        }
  
  - 로그인 버튼 생성&인증
  -     class LoginButton extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final authClient =
              Provider.of<FirebaseAuthProvider>(context, listen: false);
            final loginField = Provider.of<LoginFieldModel>(context, listen: false);
            return Container(
              width: MediaQuery.of(context).size.width * 0.85,
              height: MediaQuery.of(context).size.height * 0.05,
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(30.0),
                  ),
                ),
  - 로그인 인증이 ture 일때 아래에 스낵바가 생기고 ListScreen으로 이동
  -     onPressed: () async {
          await authClient
              .loginWithEmail(loginField.email, loginField.password)
              .then((loginStatus) {
            if (loginStatus == AuthStatus.loginSuccess) {
              ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text(authClient.user!.email! + '님 환영합니다!')),
                );
              Navigator.push(context,
                  MaterialPageRoute(builder: (context) => ListScreen()));
  
  - 로그인 인증이 false 일때 아래에 스낵바만 생김
  -     ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text('로그인에 실패했습니다. 다시 시도해주세요.')),
                );
             }
          });
        },
        child: Text('로그인'),
   
  - 회원가입버튼 생성&인증 로그인 버튼이 눌르면 push가 뒤고 회원가입을 위해 RegisterScreen으로 이동
  -     class RegisterButton extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final theme = Theme.of(context);
            return TextButton(
              onPressed: () {
                Navigator.push(context,
                  MaterialPageRoute(builder: (context) => RegisterScreen()));
              },
              child: Text(
                '이메일로 간단하게 회원가입 하기',
                style: TextStyle(color: theme.primaryColor),
              ),
            );
          }
        }

- 회원가입 RegisterScreen ( register.dart ) 뼈대 
-     class RegisterScreen extends StatelessWidget {
        @override
        Widget build(BuildContext context) {
           return ChangeNotifierProvider(
            create: (_) => RegisterFieldModel(),
            child: Scaffold(
               appBar: AppBar(
                  title: Text("회원가입 화면"),
               ),
               body: Column(
                children: [
                  EmailInput(),
                  PasswordInput(),
                  PasswordConfirmInput(),
                  RegisterButton(),
                ],
              ),
            ),
          );
        }
      }
  
  - 회원가입 이메일 가입&생성
  -     class EmailInput extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final registerField =
            Provider.of<RegisterFieldModel>(context, listen: false);
            return Container(
              padding: EdgeInsets.fromLTRB(20, 5, 20, 5),
              child: TextField(
                onChanged: (email) {
                  registerField.setEmail(email);
                },
                keyboardType: TextInputType.emailAddress,
                decoration: InputDecoration(
                  labelText: '이메일',
                  helperText: '',
                ),
               ),
              );
            }
           }
  
  - 회원가입 패스워드 가입&생성
  -     class PasswordInput extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final registerField =
            Provider.of<RegisterFieldModel>(context, listen: false);
            return Container(
              padding: EdgeInsets.fromLTRB(20, 5, 20, 5),
              child: TextField(
                onChanged: (password) {
                registerField.setPassword(password);
                },
                obscureText: true,
                decoration: InputDecoration(
                  labelText: '비밀번호',
                  helperText: '',
                ),
              ),
            );
          }
        }
  
  - 패스워드 확인 가입&생성
  -     class PasswordConfirmInput extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final registerField = Provider.of<RegisterFieldModel>(context);
            return Container(
              padding: EdgeInsets.fromLTRB(20, 5, 20, 5),
              child: TextField(
                onChanged: (password) {
                registerField.setPasswordConfirm(password);
              },
              obscureText: true,
              decoration: InputDecoration(
                labelText: '비밀번호 확인',
                helperText: '',
                errorText: registerField.password != registerField.passwordConfirm
                    ? '비밀번호가 일치하지 않습니다.'
                    : null,
                ),
              ),
            );
          }
        }
  
  - 회원가입 버튼 생성
  -     class RegisterButton extends StatelessWidget {
          @override
          Widget build(BuildContext context) {
            final authClient =
            Provider.of<FirebaseAuthProvider>(context, listen: false);
            final registerField =
            Provider.of<RegisterFieldModel>(context, listen: false);
            return Container(
              width: MediaQuery.of(context).size.width * 0.85,
              height: MediaQuery.of(context).size.height * 0.05,
              child: ElevatedButton(
                style: ElevatedButton.styleFrom(
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(30.0),
                  ),
                ),
    
  
  - 회원가입을 할때 이메일과 패스워드가 turn일때 스낵바가 생기고 텍스트 ('회원가입이 완료되었습니다!') 가 하단에 나온다.
  -      onPressed: () async {
          await authClient
              .registerWithEmail(registerField.email, registerField.password)
              .then((registerStatus) {
            if (registerStatus == AuthStatus.registerSuccess) {
              ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text('회원가입이 완료되었습니다!')),
                );
              Navigator.pop(context);
  
  - 회원가입을 할때 이메일과 패스워드 둘중 하나가 false일때 스낵바가 생기고 텍스트 (회원 가입을 실패했습니다. 다시 시도해주세요.')가 하단에 나오면서 회원가입화면으로 돌아감
  -                   ScaffoldMessenger.of(context)
                ..hideCurrentSnackBar()
                ..showSnackBar(
                  SnackBar(content: Text('회원가입을 실패했습니다. 다시 시도해주세요.')),
                );
            }
          });
        },
        child: Text('회원가입'),
