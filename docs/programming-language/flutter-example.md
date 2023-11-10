---
layout: default
title: Flutter 주요 예제
parent: Flutter
grand_parent: Programming Language
permalink: /docs/programming-language/flutter-example/
nav_order: 20
---

# Flutter 주요 예제
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}


---

{: .important-title }
> 플러터를 학습하면서 주요한 내용을 정리하였으며 주요 예제 및 코드는 `코딩쉐프` 님의 [유튜브](https://youtu.be/StvbitxUKSo) 동영상을 참고로 하였습니다. 상세한 설명에 감사하다는 말씀을 드리고 싶습니다.




### 기본 레이아웃

![example](/assets/images/flutterex/flutterex1.png)

```python

import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'BBANTO',
      home: Grade(),
    );
  }
}

class Grade extends StatelessWidget {
  const Grade({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.amber[800],
      appBar: AppBar(
        title: Text('BBANTO'),
        backgroundColor: Colors.amber[700],
        centerTitle: true,
        elevation: 0.0,
      ),
      body: Padding(
        padding: EdgeInsets.fromLTRB(30.0, 40.0, 0.0, 0.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Center(
              child: CircleAvatar(
                backgroundImage: AssetImage(
                  'assets/bc9.png'
                ),
                radius: 60.0,

              ),
            ),
            Divider(
              height: 60.0,
              color: Colors.grey[850],
              thickness: 0.5,
              endIndent: 30.0,
            ),
            Text('NAME',
               style: TextStyle(
                 color: Colors.white,
                 letterSpacing: 2.0
               )
            ),
            SizedBox(
              height: 10.0,
            ),
            Text('BBANTO',
            style: TextStyle(
              color: Colors.white,
              letterSpacing: 2.0,
              fontSize: 28.0,
              fontWeight: FontWeight.bold
            ),),
            SizedBox(
              height: 30,
            ),
            Text('BBANTOPOWER LEVEL',
                style: TextStyle(
                    color: Colors.white,
                    letterSpacing: 2.0
                )
            ),
            SizedBox(
              height: 10.0,
            ),
            Text('14',
              style: TextStyle(
                  color: Colors.white,
                  letterSpacing: 2.0,
                  fontSize: 28.0,
                  fontWeight: FontWeight.bold
              ),
            ),
          SizedBox(
            height: 30.0,
          ),
          Row(
            children: [
              Icon(Icons.check_circle_outline),
              SizedBox(
                width: 10.0,
              ),
              Text('using lightsaber',
              style: TextStyle(
                fontSize: 16.0,
                letterSpacing: 1.0,
              ),),
            ],
          ),
            Row(
              children: [
                Icon(Icons.check_circle_outline),
                SizedBox(
                  width: 10.0,
                ),
                Text('face hero tatoo',
                  style: TextStyle(
                    fontSize: 16.0,
                    letterSpacing: 1.0,
                  ),),
              ],
            ),
            Row(
              children: [
                Icon(Icons.check_circle_outline),
                SizedBox(
                  width: 10.0,
                ),
                Text('fire flagem',
                  style: TextStyle(
                    fontSize: 16.0,
                    letterSpacing: 1.0,
                  ),),
              ],
            ),
            Center(
              child: CircleAvatar(
                backgroundImage: AssetImage(
                  'assets/bcw.jpg'
                ),
                radius: 40.0,
                backgroundColor: Colors.amber[800],
              ),
            )
          ],
        ),
      ),
    );
  }
}



```


### App Bar 메뉴

![example](/assets/images/flutterex/flutterex2.png)

```python
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'AppBar',
      theme: ThemeData(
        primarySwatch: Colors.red
      ),
      home: MyPage(),
    );
  }
}

class MyPage extends StatelessWidget {
  const MyPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Appbar icon menu'),
        centerTitle: true,
        elevation: 0.0,
        leading: IconButton(
          icon: Icon(Icons.menu),
          onPressed: () {
            print('menu button clicked');
          },
        ),
        actions: [

          IconButton(
            icon: Icon(Icons.shopping_cart),
            onPressed: () {
              print('shopping cart button clicked');
            },
          ),
          IconButton(
            icon: Icon(Icons.search),
            onPressed: () {
              print('search button clicked');
            },
          )
        ],
      ),
    );
  }
}
```



### Drawer 메뉴

![example](/assets/images/flutterex/flutterex3.png)

```python
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'AppBar',
      theme: ThemeData(
        primarySwatch: Colors.red
      ),
      home: MyPage(),
    );
  }
}

class MyPage extends StatelessWidget {
  const MyPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Appbar icon menu'),
        centerTitle: true,
        elevation: 0.0,

        actions: [
          IconButton(
            icon: Icon(Icons.shopping_cart),
            onPressed: () {
              print('shopping cart button clicked');
            },
          ),
          IconButton(
            icon: Icon(Icons.search),
            onPressed: () {
              print('search button clicked');
            },
          )
        ],
      ),
      drawer: Drawer(
        child: ListView(
          padding: EdgeInsets.zero,
          children: [
            UserAccountsDrawerHeader(
              currentAccountPicture: CircleAvatar(
                backgroundImage: AssetImage('assets/bc9.png'),
                backgroundColor: Colors.white,
              ),
              otherAccountsPictures: [
                CircleAvatar(
                  backgroundImage: AssetImage('assets/bcw.jpg'),
                  backgroundColor: Colors.white,
                ),
                // CircleAvatar(
                //   backgroundImage: AssetImage('assets/bcw.jpg'),
                //   backgroundColor: Colors.white,
                // )
              ],
              accountName: Text('BAANTO'),
              accountEmail: Text('aaa@gmail.com'),
              onDetailsPressed: (){
                print('arrow is clicked..');
              },
              decoration: BoxDecoration(
                color: Colors.red[200],
                borderRadius: BorderRadius.only(
                  bottomLeft: Radius.circular(40.0),
                  bottomRight: Radius.circular(40.0),
                )
              ),
            ),

            ListTile(
              leading: Icon(Icons.home,
              color: Colors.grey[800]),
              title: Text('Home'),
              onTap: () {
                print('Home is clicked..');
              },
              trailing: Icon(Icons.add),

            ),
            ListTile(
              leading: Icon(Icons.settings,
                  color: Colors.grey[800]),
              title: Text('Setting'),
              onTap: () {
                print('Setting is clicked..');
              },
              trailing: Icon(Icons.add),

            ),
            ListTile(
              leading: Icon(Icons.question_answer,
                  color: Colors.grey[800]),
              title: Text('Q&A'),
              onTap: () {
                print('Q&A is clicked..');
              },
              trailing: Icon(Icons.add),

            )
          ],
        ),
      ),
    );
  }
}
```


### Navigator 페이지 이동

![example](/assets/images/flutterex/flutterex4.png)

```python
import 'package:flutter/material.dart';
import 'package:fluttertoast/fluttertoast.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'SnackBar',
      theme: ThemeData(primarySwatch: Colors.blue),
      home: FirstPage(),
    );
  }
}

class FirstPage extends StatelessWidget {
  const FirstPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context2) {
    return Scaffold(
      appBar: AppBar(
        title: Text('frist page'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Go to the second page'),
          onPressed: () {
            Navigator.push(
                context2, MaterialPageRoute(builder: (_) => SecondPage()));
          },
        ),
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext ctx) {
    return Scaffold(
      appBar: AppBar(
        title: Text('second page'),
      ),
      body: Center(
        child: RaisedButton(
          child: Text('Go to the first page'),
          onPressed: () {
            Navigator.pop(ctx);
          },
        ),
      ),
    );
  }
}

```



### Multi 페이지 구성 및 이동

![example](/assets/images/flutterex/flutterex5.png)

`main.dart`
```python

import 'package:coding_chef1st/ScreenB.dart';
import 'package:coding_chef1st/ScreenC.dart';
import 'package:flutter/material.dart';
import 'package:coding_chef1st/ScreenA.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/',
      routes: {
        '/' : (context) => ScreenA(),
        '/b': (context) => ScreenB(),
        '/c': (context) => ScreenC()
      },
    );
  }
}

```

`ScreenA.dart`
```python
import 'package:flutter/material.dart';

class ScreenA extends StatelessWidget {
  const ScreenA({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ScreenA')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            RaisedButton(
              color: Colors.red,
              child: Text('Go to Screen B'),
              onPressed: (){
                Navigator.pushNamed(context, '/b');
              },
            ),
            RaisedButton(
              color: Colors.red,
              child: Text('Go to Screen C'),
              onPressed: (){
                Navigator.pushNamed(context, '/c');
              },
            )
          ],
        )
      ),
    );
  }
}

```

`ScreenB.dart`
```python
import 'package:flutter/material.dart';

class ScreenB extends StatelessWidget {
  const ScreenB({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ScreenB')),
      body: Center(
        child: Text(
          'ScreenB',
          style: TextStyle(fontSize: 24.0),
        ),
      ),
    );
  }
}

```

`ScreenC.dart`
```python
import 'package:flutter/material.dart';

class ScreenC extends StatelessWidget {
  const ScreenC({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('ScreenC')),
      body: Center(
        child: Text(
          'ScreenC',
          style: TextStyle(fontSize: 24.0),
        ),
      ),
    );
  }
}

```



### 스낵바 표시 및 스낵바 버튼 페이지 이동

![example](/assets/images/flutterex/flutterex6.png)

```python
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(primarySwatch: Colors.blue),
      home: MyPage(),
    );
  }
}

class MyPage extends StatelessWidget {
  const MyPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Scaffold Messenaer')),
      body: HomeBody(),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          ScaffoldMessenger.of(context).showSnackBar(SnackBar(
            content: Text('Like a new snalckslfdsf'),
            duration: Duration(seconds: 5),
            action: SnackBarAction(
              label: 'Undo',
              onPressed: () {
                Navigator.push(context,
                    MaterialPageRoute(builder: (context) => ThirdPage()));
              },
            ),
          ));
        },
        child: Icon(Icons.thumb_up),
      ),
    );
  }
}

class HomeBody extends StatelessWidget {
  const HomeBody({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Center(
      child: ElevatedButton(
        child: Text('Go to second page'),
        onPressed: () {
          Navigator.push(
              context, MaterialPageRoute(builder: (context) => SecondPage()));
        },
      ),
    );
  }
}

class SecondPage extends StatelessWidget {
  const SecondPage({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        appBar: AppBar(title: Text('Second Page')),
        body: Center(
          child: Text(
            "종하요가 추가되었습니다",
            style: TextStyle(fontSize: 20.0, color: Colors.redAccent),
          ),
        ));
  }
}

class ThirdPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ScaffoldMessenger(
      child: Scaffold(
          appBar: AppBar(title: Text('third Page')),
          body: Builder(builder: (context) {
            return Center(
              child: Column(
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(
                    "종하요를 취소하시겠습니다.?",
                    style: TextStyle(fontSize: 20.0, color: Colors.yellow),
                  ),
                  ElevatedButton(
                      onPressed: () {
                        ScaffoldMessenger.of(context).showSnackBar(SnackBar(
                          content: Text("좋아요가 취소되었습니다."),
                          duration: Duration(seconds: 3),
                        ));
                      },
                      child: Text("취소하기"))
                ],
              ),
            );
          })),
    );
  }
}

```

### 버튼 종류 및 표현

![example](/assets/images/flutterex/flutterex7.png)

```python
import 'package:flutter/material.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      theme: ThemeData(primarySwatch: Colors.blue),
      home: MyButtons(),
    );
  }
}

class MyButtons extends StatelessWidget {
  const MyButtons({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Buttons'),
        centerTitle: true,
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            TextButton(
              // onPressed: () {
              //   print("text button clicked");
              // },
              onLongPress: () {
                print("text button clicked");
              },
              child: Text('TextButton', style: TextStyle(fontSize: 20.0)),
              style: TextButton.styleFrom(
                primary: Colors.red,
                // backgroundColor: Colors.blue
              ),
              onPressed: () {},
            ),
            ElevatedButton(
              onPressed: null,
              child: Text('ElevatedButton'),
              style: ElevatedButton.styleFrom(
                  primary: Colors.orangeAccent,
                  shape: RoundedRectangleBorder(
                    borderRadius: BorderRadius.circular(15.0),
                  ),
                  elevation: 0.0),
            ),
            OutlinedButton(
              onPressed: () {
                print('OutLIned Button');
              },
              child: Text('OutLinedButton'),
              style: OutlinedButton.styleFrom(
                  primary: Colors.green,
                  side: BorderSide(color: Colors.black87, width: 2.0)),
            ),
            TextButton.icon(
              onPressed: () {
                print('Icon Button');
              },
              icon: Icon(
                Icons.home,
              ),
              label: Text('Go to Home'),
              style: OutlinedButton.styleFrom(
                  primary: Colors.black),
            ),
            ButtonBar(
              alignment: MainAxisAlignment.center,
              buttonPadding: EdgeInsets.all(20),
              children: [
                TextButton(onPressed: (){},
                    child: Text('Text Button')),
                ElevatedButton(onPressed: (){},
                    child: Text('ElevatedButton'))
              ],
            )
          ],
        ),
      ),
    );
  }
}

```


### 예제) Dice Game

![example](/assets/images/flutterex/flutterex8.png)

![example](/assets/images/flutterex/flutterex9.png)

`main.dart`

```python
import 'package:flutter/material.dart';
import 'dice.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Dice game',
      home: LogIn(),
    );
  }
}

class LogIn extends StatefulWidget {
  @override
  State<LogIn> createState() => _LogInState();
}

class _LogInState extends State<LogIn> {
  TextEditingController controller = TextEditingController();
  TextEditingController controller2 = TextEditingController();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Log in'),
        backgroundColor: Colors.redAccent,
        centerTitle: true,
        leading: IconButton(icon: Icon(Icons.menu), onPressed: () {}),
        actions: <Widget>[
          IconButton(icon: Icon(Icons.search), onPressed: () {})
        ],
      ),
      body: Builder(
        builder: (context) {
          return GestureDetector(
            onTap: () {
              FocusScope.of(context).unfocus();
            },
            child: SingleChildScrollView(
              child: Column(
                children: [
                  Padding(padding: EdgeInsets.only(top: 50)),
                  Center(
                    child: Image(
                        image: AssetImage('image/chef.gif'),
                        width: 170.0,
                        height: 190.0),
                  ),
                  Form(
                      child: Theme(
                          data: ThemeData(
                              primaryColor: Colors.teal,
                              inputDecorationTheme: InputDecorationTheme(
                                  labelStyle: TextStyle(
                                      color: Colors.teal, fontSize: 15.0))),
                          child: Container(
                            padding: EdgeInsets.all(40.0),
                            child: Column(
                              children: [
                                TextField(
                                  controller: controller,
                                  decoration: InputDecoration(
                                      labelText: 'Enter "dice"'),
                                  keyboardType: TextInputType.emailAddress,
                                ),
                                TextField(
                                  controller: controller2,
                                  decoration: InputDecoration(
                                      labelText: 'Enter Password'),
                                  keyboardType: TextInputType.text,
                                  obscureText: true,
                                ),
                                SizedBox(height: 40.0),
                                ButtonTheme(
                                    minWidth: 100.0,
                                    height: 50.0,
                                    child: RaisedButton(
                                        color: Colors.orangeAccent,
                                        child: Icon(Icons.arrow_forward,
                                            color: Colors.white, size: 35.0),
                                        onPressed: () {
                                          if (controller.text == 'dice' &&
                                              controller2.text == '1234') {
                                            Navigator.push(
                                                context,
                                                MaterialPageRoute(
                                                    builder: (BuildContext
                                                            context) =>
                                                        Dice()));
                                          } else if (controller.text ==
                                                  'dice' &&
                                              controller2.text != '1234') {
                                            showSnackBar2(context);
                                          } else if (controller.text !=
                                                  'dice' &&
                                              controller2.text == '1234') {
                                            showSnackBar3(context);
                                          } else {
                                            showSnackBar(context);
                                          }
                                        }))
                              ],
                            ),
                          )))
                ],
              ),
            ),
          );
        },
      ),
    );
  }
}

void showSnackBar(BuildContext context) {
  Scaffold.of(context).showSnackBar(SnackBar(
    content: Text(
      '로그인 정보를 다시 확인하세요',
      textAlign: TextAlign.center,
    ),
    duration: Duration(seconds: 2),
    backgroundColor: Colors.blue,
  ));
}

void showSnackBar2(BuildContext context) {
  Scaffold.of(context).showSnackBar(SnackBar(
    content: Text(
      '비밀번호가 일치항ㄶ음',
      textAlign: TextAlign.center,
    ),
    duration: Duration(seconds: 2),
    backgroundColor: Colors.blue,
  ));
}

void showSnackBar3(BuildContext context) {
  Scaffold.of(context).showSnackBar(SnackBar(
    content: Text(
      'dice 정보 확인',
      textAlign: TextAlign.center,
    ),
    duration: Duration(seconds: 2),
    backgroundColor: Colors.blue,
  ));
}

```

`dice.dart`
```python
import 'package:flutter/material.dart';
import 'dart:math';
import 'package:fluttertoast/fluttertoast.dart';

class Dice extends StatefulWidget {
  const Dice({Key? key}) : super(key: key);

  @override
  State<Dice> createState() => _DiceState();
}

class _DiceState extends State<Dice> {
  int leftDice = 1;
  int rightDice = 1;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.redAccent,
      appBar: AppBar(
        backgroundColor: Colors.redAccent,
        title: Text('Dice Game'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            Padding(
              padding: EdgeInsets.all(32.0),
              child: Row(
                children: [
                  Expanded(child: Image.asset('image/dice$leftDice.png')),
                  SizedBox(width: 20.0),
                  Expanded(child: Image.asset('image/dice$rightDice.png')),
                ],
              ),
            ),
            SizedBox(height: 60.0),
            ButtonTheme(
              minWidth: 100.0,
              height: 60.0,
              child: RaisedButton(
                child: Icon(
                  Icons.play_arrow,
                  color: Colors.white,
                  size: 50.0,
                ),
                onPressed: () {
                  setState(() {
                    leftDice = Random().nextInt(6) + 1;
                    rightDice = Random().nextInt(6) + 1;
                  });
                  showToast(
                      "Left Dice : {$leftDice} , Right Dice : {$rightDice}");
                },
                color: Colors.orangeAccent,
              ),
            )
          ],
        ),
      ),
    );
  }
}

void showToast(String message) {
  Fluttertoast.showToast(
      msg: message,
      backgroundColor: Colors.white,
      textColor: Colors.black,
      toastLength: Toast.LENGTH_SHORT,
      gravity: ToastGravity.BOTTOM);
}

```


### http & json 데이터 가져오기

![example](/assets/images/flutterex/flutterex10.png)

`main.dart`

```python
import 'package:flutter/material.dart';

import 'screens/loading.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Weather App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Loading(),
    );
  }
}

```

`loading.dart`

```python
import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'package:http/http.dart' as http;
import 'dart:convert';

class Loading extends StatefulWidget {
  const Loading({Key? key}) : super(key: key);

  @override
  _LoadingState createState() => _LoadingState();
}

class _LoadingState extends State<Loading> {

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    getLocation();
    fetchData();
  }

  void getLocation() async{
    try{
      Position position = await Geolocator.getCurrentPosition(desiredAccuracy: LocationAccuracy.high);
      print(position);
    } catch(e) {
      print('인터넷 연결에 문제가 있습니다.');
    }
  }

  void fetchData() async{
    http.Response response = await http.get('https://samples.openweathermap.org/data/2.5/weather?q=London&appid=b1b15e88fa797225412429c1c50c122a1');
    print(response.body);
    print(response.statusCode);

    if(response.statusCode == 200) {
      String jsonData = response.body;
      var myJson = jsonDecode(jsonData)['weather'][0]['description'];
      print(myJson);
      var windSpeed = jsonDecode(jsonData)['wind']['speed'];
      var sysId = jsonDecode(jsonData)['id'];
      print(windSpeed);
      print(sysId);


    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
            child: RaisedButton(
              onPressed:null,
              child: Text(
                'Get my location',
                style: TextStyle(color: Colors.white),
                ),
              color: Colors.blue,
            )
        )
    );
  }
}


```




### Spin 기능 및 날씨앱 (최종)

![example](/assets/images/flutterex/flutterex11.png)


![example](/assets/images/flutterex/flutterex12.png)


`main.dart`

```python
import 'package:flutter/material.dart';

import 'screens/loading.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Weather App',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Loading(),
    );
  }
}

```


`my_location.dart`

```python

import 'package:geolocator/geolocator.dart';

class MyLocation{

  late double latitude2;
  late double longitude2;

  Future<void> getMyCurrentLocation() async{

    try{
      Position position = await Geolocator.getCurrentPosition(desiredAccuracy: LocationAccuracy.high);
      print(position);
      latitude2 = position.latitude;
      longitude2 = position.longitude;

    } catch(e) {
      print('인터넷 연결에 문제가 있습니다.');
    }

  }
}
```

`network.dart`

```python
import 'package:http/http.dart' as http;
import 'dart:convert';

class Network{

  final String url;
  final String url2;

  Network(this.url, this.url2);

  Future<dynamic> getJsonData() async {
    http.Response response = await http.get(Uri.parse(url));
    // print(response.body);
    // print(response.statusCode);

    if (response.statusCode == 200) {
      String jsonData = response.body;
      var pasingData = jsonDecode(jsonData);
      return pasingData;
    } else {
      print(response.body);
    }
  }


  Future<dynamic> getAirData() async {
    http.Response response = await http.get(Uri.parse(url2));
    // print(response.body);
    // print(response.statusCode);

    if (response.statusCode == 200) {
      String jsonData = response.body;
      var pasingData = jsonDecode(jsonData);
      return pasingData;
    } else {
      print(response.body);
    }
  }


}
```


`model.dart`

```python
import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';
import 'package:flutter_svg/flutter_svg.dart';

class Model {

  Widget getWeatherIcon(int condition) {
    if (condition < 300) {
      return SvgPicture.asset(
        'svg/climacon-cloud_lightning.svg',
        color: Colors.black87,
      );
    } else if (condition < 600) {
      return SvgPicture.asset(
        'svg/climacon-cloud_snow_alt.svg',
        color: Colors.black87,
      );
    } else if (condition < 800) {
      return SvgPicture.asset(
        'svg/climacon-sun.svg',
        color: Colors.black87,
      );
    } else {
      return SvgPicture.asset(
        'svg/climacon-cloud_sun.svg',
        color: Colors.black87,
      );
    }
  }

  Widget getAirIcon(int index){
    if(index == 1) {
      return Image.asset('image/good.png',
      width: 37.0,
      height: 35.0);
    } else if(index == 2) {
      return Image.asset('image/fair.png',
          width: 37.0,
          height: 35.0);
    } else if(index == 3) {
      return Image.asset('image/moderate.png',
          width: 37.0,
          height: 35.0);
    } else if(index == 4) {
      return Image.asset('image/poor.png',
          width: 37.0,
          height: 35.0);
    } else  {
      return Image.asset('image/bad.png',
          width: 37.0,
          height: 35.0);
    }
  }


  Widget getAirCondition(int index){
    if(index == 1) {
      return Text('매우 좋음',
      style: TextStyle(
        color: Colors.indigo,
        fontWeight: FontWeight.bold
        ),
      );
    } else if(index == 2) {
      return Text('좋음',
        style: TextStyle(
            color: Colors.indigo,
            fontWeight: FontWeight.bold
        ),
      );
    } else if(index == 3) {
      return Text('보통',
        style: TextStyle(
            color: Colors.indigo,
            fontWeight: FontWeight.bold
        ),
      );
    } else if(index == 4) {
      return Text('나쁨',
        style: TextStyle(
            color: Colors.indigo,
            fontWeight: FontWeight.bold
        ),
      );
    } else  {
      return Text('매우 나쁨',
        style: TextStyle(
            color: Colors.indigo,
            fontWeight: FontWeight.bold
        ),
      );
    }
  }


}
```


`loading.dart`

```python
import 'package:coding_chef1st/screens/weather_screen.dart';
import 'package:flutter/material.dart';
import 'package:coding_chef1st/data/my_location.dart';
import 'package:coding_chef1st/data/network.dart';
import 'package:flutter_spinkit/flutter_spinkit.dart';

const apikey = 'a759b12d9ea168907120f96bd766b754';


class Loading extends StatefulWidget {
  const Loading({Key? key}) : super(key: key);

  @override
  _LoadingState createState() => _LoadingState();
}

class _LoadingState extends State<Loading> {
  late double latitude3;
  late double longitude3;

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    getLocation();
  }

  void getLocation() async {
    MyLocation myLocation = MyLocation();
    await myLocation.getMyCurrentLocation();
    latitude3 = myLocation.latitude2;
    longitude3 = myLocation.longitude2;
    print(latitude3);
    print(longitude3);

    Network network = new Network(
        'https://api.openweathermap.org/data/2.5/'
            'weather?lat=$latitude3&lon='
            '$longitude3&appid=$apikey&units=metric',
        'https://api.openweathermap.org/data/2.5/'
            'air_pollution?lat=$latitude3&lon='
            '$longitude3&appid=$apikey');

    var weatherData = await network.getJsonData();
    print(weatherData);

    var airData = await network.getAirData();
    print(airData);

    Navigator.push(context, MaterialPageRoute(builder: (context) {
      return WeatherScreen(
        parseWeatherData: weatherData,
        parseAirPollution: airData,
      );
    }));
  }

  // void fetchData() async{
  //
  //     var myJson = pasingData['weather'][0]['description'];
  //     print(myJson);
  //     var windSpeed = pasingData['wind']['speed'];
  //     var sysId = pasingData['id'];
  //     print(windSpeed);
  //     print(sysId);
  //   } else {
  //     print(response.statusCode);
  //   }
  // }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.amber,
      body: Center(
        child: SpinKitDoubleBounce(
          color: Colors.white,
          size: 80.0,
        ),
      ),
    );
  }
}

```


`weather_screen.dart`

```python
import 'package:flutter/material.dart';
import 'package:google_fonts/google_fonts.dart';
import 'package:flutter_svg/flutter_svg.dart';
import 'package:timer_builder/timer_builder.dart';
import 'package:intl/intl.dart';
import 'package:coding_chef1st/model/model.dart';

class WeatherScreen extends StatefulWidget {
  WeatherScreen({this.parseWeatherData, this.parseAirPollution});

  final dynamic parseWeatherData;
  final dynamic parseAirPollution;

  @override
  _WeatherScreenState createState() => _WeatherScreenState();
}

class _WeatherScreenState extends State<WeatherScreen> {
  Model model = Model();
  String? cityName;
  late int temp;
  late Widget icon;
  late Widget airIcon;
  late Widget airState;
  late String des;
  late double dust1;
  late double dust2;

  var date = DateTime.now();

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    updateData(widget.parseWeatherData, widget.parseAirPollution);
  }

  void updateData(dynamic whetherData, dynamic airData) {
    double temp2 = whetherData['main']['temp'];
    temp = temp2.round();
    int condition = whetherData['weather'][0]['id'];
    int index = airData['list'][0]['main']['aqi'];
    cityName = whetherData['name'];
    icon = model.getWeatherIcon(condition);
    des = whetherData['weather'][0]['description'];


    airIcon = model.getAirIcon(index);
    airState = model.getAirCondition(index);

    dust1 = airData['list'][0]['components']['pm10'];
    dust2 = airData['list'][0]['components']['pm2_5'];

    print(temp);
    print(cityName);
  }

  String getSystemTime() {
    var now = DateTime.now();
    return DateFormat("h:mm a").format(now);
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      extendBodyBehindAppBar: true,
      appBar: AppBar(
        // title: Text('dd'),
        backgroundColor: Colors.transparent,
        elevation: 0.0,
        leading: IconButton(
          icon: Icon(Icons.near_me),
          onPressed: () {},
          iconSize: 30.0,
        ),
        actions: [
          IconButton(
            icon: Icon(
              Icons.location_searching,
            ),
            onPressed: () {},
            iconSize: 30,
          )
        ],
      ),
      body: Container(
        child: Stack(
          children: [
            Image.asset(
              'image/background.jpg',
              fit: BoxFit.cover,
              width: double.infinity,
              height: double.infinity,
            ),
            Container(
              padding: EdgeInsets.all(20.0),
              child: Column(
                mainAxisAlignment: MainAxisAlignment.spaceBetween,
                children: [
                  Expanded(
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.spaceBetween,
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            SizedBox(height: 150.0),
                            Text(
                              '$cityName',
                              style: GoogleFonts.lato(
                                  fontSize: 35.0,
                                  fontWeight: FontWeight.bold,
                                  color: Colors.white),
                            ),
                            Row(
                              children: [
                                TimerBuilder.periodic((Duration(minutes: 1)),
                                    builder: (context) {
                                  print('${getSystemTime()}');
                                  return Text(
                                    '${getSystemTime()}',
                                    style: GoogleFonts.lato(
                                        fontSize: 16.0, color: Colors.white),
                                  );
                                }),
                                Text(DateFormat(' - EEEE, ').format(date),
                                    style: GoogleFonts.lato(
                                        fontSize: 16.0, color: Colors.white)),
                                Text(DateFormat('d MMM, yyy').format(date),
                                    style: GoogleFonts.lato(
                                        fontSize: 16.0, color: Colors.white))
                              ],
                            )
                          ],
                        ),
                        Column(
                          crossAxisAlignment: CrossAxisAlignment.start,
                          children: [
                            Text(
                              '$temp\u2103',
                              style: GoogleFonts.lato(
                                  fontSize: 85.0,
                                  fontWeight: FontWeight.w300,
                                  color: Colors.white),
                            ),
                            Row(
                              children: [
                                icon,
                                SizedBox(
                                  width: 10.0,
                                ),
                                Text('$des',
                                    style: GoogleFonts.lato(
                                        fontSize: 16.0, color: Colors.white))
                              ],
                            )
                          ],
                        )
                      ],
                    ),
                  ),
                  Column(
                    children: [
                      Divider(
                        height: 15.0,
                        thickness: 2.0,
                        color: Colors.white30,
                      ),
                      Row(
                        mainAxisAlignment: MainAxisAlignment.spaceBetween,
                        children: [
                          Column(
                            children: [
                              Text('대기질 지수',
                                  style: GoogleFonts.lato(
                                      fontSize: 14.0, color: Colors.white)),
                              SizedBox(
                                height: 10.0,
                              ),
                              airIcon,
                              SizedBox(
                                height: 10.0,
                              ),
                              airState,
                            ],
                          ),
                          Column(
                            children: [
                              Text('미세 먼지',
                                  style: GoogleFonts.lato(
                                      fontSize: 14.0, color: Colors.white)),
                              SizedBox(
                                height: 10.0,
                              ),
                              Text('$dust1',
                                  style: GoogleFonts.lato(
                                      fontSize: 24.0, color: Colors.white)),
                              SizedBox(
                                height: 10.0,
                              ),
                              Text('mg/m3',
                                  style: GoogleFonts.lato(
                                      fontSize: 14.0, color: Colors.white,
                                      fontWeight: FontWeight.bold)),
                            ],
                          ),
                          Column(
                            children: [
                              Text('초미세먼지',
                                  style: GoogleFonts.lato(
                                      fontSize: 14.0, color: Colors.white)),
                              SizedBox(
                                height: 10.0,
                              ),
                              Text('$dust2',
                                  style: GoogleFonts.lato(
                                      fontSize: 24.0, color: Colors.white)),
                              SizedBox(
                                height: 10.0,
                              ),
                              Text('mg/m3',
                                  style: GoogleFonts.lato(
                                      fontSize: 14.0, color: Colors.white,
                                      fontWeight: FontWeight.bold)),
                            ],
                          )
                        ],
                      )
                    ],
                  )
                ],
              ),
            )
          ],
        ),
      ),
    );
  }
}

```








### Stream Builder 예제
- 1초마다 카운터가 증가하는 예제

![example](/assets/images/flutterex/flutterex13.png)



`main.dart`

```python


import 'package:flutter/material.dart';
import 'package:stream_ex/counter.dart';

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Stream Builder',
      theme: ThemeData(
        primaryColor: Colors.blue
      ),
      home: Counter(),
    );
  }
}

```

`counter.dart`

```python
import 'package:flutter/material.dart';


class Counter extends StatefulWidget {
  const Counter({Key? key}) : super(key: key);

  @override
  _CounterState createState() => _CounterState();
}

class _CounterState extends State<Counter> {

  final int price = 2000;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Stream Builder'),
      ),
      body: StreamBuilder<int>(
        initialData: price,
        stream: addStreamValue(),
        builder: (context, snapshot) {
          final priceNumber = snapshot.data.toString();
          return Center(
            child: Text(priceNumber,
              style: TextStyle(
                  fontSize: 40,
                  color: Colors.blue
              ),),
          );
        },
      ),
    );
  }


  Stream<int> addStreamValue() {
    return Stream<int>.periodic(
        Duration(seconds: 1),
            (count) => price + count
    );
  }
}


```










코딩셰프 조금 매운맛 25강부터 시작하기
