#  개발일기
## 2022년 10월 17일 화요일
### provider& book앱 분석


#### provider는 하나의 상태조각의 압축(encapsulates)된 객체이자 상태의 변화를 감시하는 역할을 가지고 있다. StateLessWidget(상태가 없는) StateFulWidget(상태가 있는)


#####  Provider.of<FirebaseAuthProvider>(context, listen: false);  부모의 FirebaseAuthProvider를 찾을려면

- My app 
- provider 는  Materialapp 부모노드  자식노드에게 provider를 연결할 수 있다. 
- MaterialApp 
- LoginScreen
- body
- LoginButton 
- Provider.of<FirebaseAuthProvider>(context, listen: false);

##### final loginField = Provider.of<LoginFieldModel>(context, listen: false); 부모의 LoginFielModel를 찾을려면 

- My app 
- MaterialApp
- LoginScreen
- 부모의 provider LoginFielModel 
- return ChangeNotifierProvider(   create: (_) => LoginFielModel 
- body
- LoginButton
- final loginField = Provider.of<LoginFieldModel>(context, listen: false);

  ##### ChangeNotifierProvider를 통해 변화에 대해 구독한다. (1개만 구독이 가능하다. 여러 개를 구독하기 위해서는 Muitprovider로 감싼 후 사용해야된다.)
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
      
  -   
