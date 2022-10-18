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

  - main.dart
  -   void main() async {
      WidgetsFlutterBinding.ensureInitialized();
      await Firebase.initializeApp();
      runApp(MyApp());
     }
  - 다른 언어와 비슷하게 void main이 먼저 시작이 되고 
