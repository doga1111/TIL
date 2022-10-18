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
