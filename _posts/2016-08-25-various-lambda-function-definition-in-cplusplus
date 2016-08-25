---
layout: post
title:  "various lambda function definitions in c++"
date:   2016-08-25 08:47:00 +0900
categories: c++
---

#### 기본적인 람다식

```
auto func = []() {
   std::cout << "람다 테스트" << endl;
};
func();
```

#### 인자가 있는 람다식

```
auto func = [](int k) {
   std::cout << "람다 테스트 int k : " << k <<  endl;
};
func(10);
```

#### 반환값이 있는 람다식

```
auto func = [](int k) {
   return k;
};
int k = func(10);
```

#### 반환값이 명시되어 있는 람다식

```
auto func = [](int k)  -> int { // 반환값을 int로 지정한다!
   return k;
};
int k = func(10);
```

#### 값을 참조 하는 람다식

```
int nNum = 10;
auto func = [&](int k) {   // []안의 &는 참조.. 람다식 밖에 있는 변수를 참조한다 
                           // [&nNum]과 같이 특정 변수만 가져올수도 있다!
   nNum = k;
};
func(10);
```

#### 값을 복사 하는 람다식

```
int nNum = 10;
auto func = [=](int k) {   // []안의 = 은 값을 복사 해온다. 
                           // [nNum]과 같이 사용할수 있다.
   int nTest = nNum;
   nNum = k;               // 이와같이 하면 값의 복사기 때문에 nNum에 값을 넣어도 nNum의 값은 바뀌지 않는다.
};                         // 다군다나 람다식에서는 무의미하기 때문인지 에러를 발생시킨다.
func(10);
```

#### 재귀함수로  사용하는 람다식

```
std::tr1::function<void(int)> funtest = [&funtest](int k)    //auto 대신 function을 사용했다.
{                                                //재귀 호출에서는 타입을 명시해야되기때문에 function으로
	if(k != 0) { //람다식이 function임을 알려준다.
		cout << k << endl;
		funtest(--k);
	}
};
funtest(10);                                                                              
```

- original source: [http://icartsh.tistory.com/14](http://icartsh.tistory.com/14)
