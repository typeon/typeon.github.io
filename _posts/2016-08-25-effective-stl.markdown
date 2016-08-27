---
layout: post
title:  "Effective STL summary"
date:   2016-08-25 21:10:00 +0900
categories: c++
---

**1.적재적소에 알맞은 컨테이너를 사용하자.**

STL에는 표준,비표준을 포함하여 많은 컨테이너들이 존재하지만
어떤 컨테이터를 어떻게 사용하느냐에 따라 그 알고리즘 복잡도가 상당히 달라진다.
항상 현재 접한 상황에 어떤 컨테이너가 가장 적당한지를 잘 판단하여 선택하라는 내용이 있다.

**2."컨테이너에 독립적인 코드"를 만들려고 하지말자!**

이 컨테이너 저 컨테이너 모두 적용될 수있는 클래스를 만들려하지말자. 쓸데없는 짓이란 것. STL컨테이너 사용시 typedef를 이용할것.

```
typedef std::list< string > CList  ;
typedef std::list< string >::iterator  CListItr  ;
```
코딩양이 줄어들고.. 컨테이너 타입을 바꿀 수밖에 없는 상황에 변경을 조금 용이하게 해준다.

**3.STL 컨테이너이가 객체를 다룰때는 복사되어 들어가고 복사되어 나온다.**

따라서 클래스의 복사생성자와 복사대입연산자는 깊은복사(deep copy)를 해야 한다.
그리고 일반 객체를 담는 컨테이너경우 slice문제도 발생되는데 이것저것 다 따지기 귀찮으면 그냥 포인터를 담는 컨테이너를 선언해라.
포인터는 스마트 포인터로 담아주면 딱 좋다.

```
typedef std::shared_ptr< CObject > CObjectPtr ;
typedef std::list< CObjectPtr > CListPtr ;
```

**4.size() 결과를 0과 비교할 생각이면 차라리 empty() 를 호출하자.**

```
if(c.size() == 0 )  // bad
if(c.empty()) // good
```

**5.단일 요소를 단위로 동작하는 멤버 함수보다 요소의 범위를 단위로 동작하는 멤버함수가 더 낫다.**

삽입연산자(inserter, back_inserter, front_inserter) 를 사용해서 복사 대상범위를 지정하는 copy 함수는 모두 범위 멤버 함수로 바꾸어야 한다.
단일 요소 멤버함수는 메모리 할당자이 요구도 많고 객체 복사도 빈번하며, 불필요한 연산도 더 자주 발생한다.

- 범위 생성 : container::container(inputiterator begin, inputiterator end);
- 범위 삽입 : void container::insert(iterator pos, inputiterator begin, inputiterator end) ;
- 범위 삽입 : void container::insert(inputiterator begin, inputiterator end) ; // 연관 컨테이너경우
- 범위 삭제 : iterator container::erase(iterator begin, iterator end) ;
- 범위 삭제 : void container::erase(iterator begin, iterator end) ; // 연관 컨테이너경우
- 범위 대입 : void container::assign(iterator begin, iterator end) ;

범위 멤버 함수는 작성이 쉽고, 동작의 의미를 좀더 명확하게 나타내주며 ,수행 성능도 월등히 좋다.

**6.C++ 컴파일러의 어이없는 분석 결과를 조심하자.**

**7.new로 생성한 포인터의 컨테이너를 사용할 때는 스마트 포인터를 사용하자.**

단 auto_ptr은 그 특성상 사용하면 안된다.

```
typedef std::auto_ptr< CObject > CObjectPtr ; // bad
typedef std::shared_ptr< CObject > CObjectPtr ; // good
typedef std::list< CObjectPtr > CListPtr ;
```

**8.데이터 삭제할 때에도 조심스럽게 선택할 것이 많다.**

컨테이너에서 특정한 값을 가진 객체를 모두 없애려면 :

- 컨테이너가 vector, string, deque 이면 erase-remove 합성문을 쓴다.
- 컨테이너가 list이면 list::remove를 쓴다.
- 컨테이너가 표준 연관 컨테이너면 eraes 멤버 함수르 쓴다.

컨테이너에서 특정한 술어구문(함수,함수객체)을 만족하는 객체를 모두 없애려면 :

- 컨테이너가 vector, string, deque 이면 erase-remove_if 합성문을 쓴다.
- 컨테이너가 list이면 list::remove_if를 쓴다.
- 컨테이너가 표준 연관 컨테이너면 remove_copy_if아 swap을 쓰든지 컨테이너 내부를 도는 루프에서 erase를 호출하면서 erase에 넘기는 반복자를 후위 증가 연산자로 증가시킨다.

루프 안에서 무엇(로그작성 또는 전위작업,,후위작업등)인가를 하려면(객체 삭제도 포함해서) " 컨테이너가 표준 시퀀스 컨테이너면 컨테이너 요소를 하나씩 사용하는 루프를 작성하고 이때 erase를 호출 할 때 마다 그 함수의 반환값으로 반복자를 업데이트하는 일을 꼭 해야한다.( 반복자 무효화때문에..) 컨테이너가 표준 연관 컨테이너면 역시 컨테이너 요소를 하나씩 사용하는 루프를 작성하고 erase를 호출하면서 erase에 넘기는 반복자를 후위 증가 연산자로 증가 시킨다.

**10.할당자의 일반적인 사항과 제약 사항에 대해 잘 알아두자.**

**11.커스텀 할당자를 제대로 사용하는 방법을 이해햐자.**

**12.STL 컨테이너의 쓰레드 안정성에 대한 기대는 현실에 맞추어 가지자.**

**13.동적으로 할당된 배열보다는 vector와 string이 낫다.**

배열을 직접 동적으로 할당하려고 쓸데 없는 일(메모리 해제등)에 온 신경을 써야한다.
vector와 string은 자동으로  내부적으로 동적배열 처럼 메모리를 할당하고 해제해준다.

**14.reserve는 필요없이 메모리가 재할당되는 것을 막아준다.**

STL컨테이너(시퀀스컨테이너)의 메모리 관리는 데이터 삽입시 메모리가 부족한 경우 2의 증가율 만큼 용량을 늘리도록 구현하고 있다.
또한 재할당시 컨테이너가 원래 가지고있었던 메모리에 저장된 모든 요소 데이터를 새 메모리에 복사하고 원래의 메모리에 저장된
모든 객체를 소멸시키고 원래의 매모리를 해제한다. 이런 할당-소멸-해제는 상당한 비용이 든다.
따라서 미리 어느정도의 메모리를 할당해 둠으로써 재할당의 회수를 최소화시켜주는것이 바로 reserve이다.

- size() : 현재 컨테이너에 들어있는 효소의 개수를 리턴
- capacity() : 해당 컨테이너에 할당된 메모리로 담을 수 있는 요소의 개수를 리턴
- resize(size_t n) : 컨테이너가 담을수있는 개수를 n개로 무조건 만든다. resize호출후 size() 호출하면 n이 리턴된다.
- reserve(size_T n) :  컨테이너의 용량을 최소한 n개로 맞춘다. size()에 영향을 미치지 않는다.

**15.잊지 말자 ! string은 여러 가지 방식으로 구현되어 있다는 사실.**

**16.기존의 C API에 vector와 string을 넘기는 방법을 알아두자.**

```
void doSomthing( const char* p,, const int* pints,  size_t num) ;
set intset ;
...
string s("test") ;
vector v(intset.begin(), intset.end()) ;
doSomthing(s.c_str(), &v[0], v.size()) ;
```

**17.쓸데없이 남은 용량은 swap을 써서 없애버리자.**

```
string s ;
...
string(s).swap(s) ;

.....................
vector v ;
string s ;
.....
vector().swap(v) ; // v를 완전히 비우고 용량을 최소화한다.
string().swap(s) ; // s를 완전히 비우고 용량을 최소화한다.
```

**18.vecto은 절대 사용하지 말자!**

대신 deque 또는 bitset 을 사용하도록 한다.

**19.상등관계와 동등 관계의 차이를 파악한다.**

- 상등관계 : operator ==
- 동등관계 : operator <

**20.포인터를 저장하는 연관컨테이너에 대해서는 적합한 비교(비교 함수자) 타입을 정해주자.**

포인터를 담는 연관 컨테이너를 만들어 쓸 때에는 컨테이너에 담긴 포인터 값을 기준으로 정렬이 이루어진다는 사실을 기억해야한다.

**21.연관 컨테이너용 비교함수는 같은 같에 대해 false를 반환해야 한다.**

**22.set과 multiset에 저장된 데이터 요소에 대해  key를 바꾸는 일이 없도록한다.**

만약 키를 바꾸고 싶다면:
1.변경하고자하는 컨테이너의 요소의 위치를 찾고
2.변경하고자하는 요소의 복사본을 만들고..
3.컨테이너에서 erase를 호출하여 그 요소를 삭제한 후
4.만들어둔 복사본의 정보를 바꾼다.
5.복사본을 컨테이너에 새로 삽입한다.

즉 변경 할 것을 복사해놓고 변경할 부분을 삭제 후 복사해 놓은것을 변경 후 다시 삽입 하라는 소리이다.

**23.연곤 컨테이너 대신에 정렬된 vector를 쓴느 것이 좋을 때가 있다.**

**24.맵(map)에 추가할 경우 operator[]보다 insert가 낮고 저장된 요소를 갱신(변경)할 경우 insert보다 operator[]이 효율적으로나 미적으로 더 낫다.**

**25.현재 표준이 아니지만, 해쉬 컨테이너에 대해 충분히 대비해두자!**

해쉬 컨테이너는 map이나 set가 다르게 데이터가 정렬된 순서로 저장되어 있지 않습니다. 하지만 검색속도는 map이나 set보다 훨씬더 빠릅니다.

**26.const_iterator 나 reverse_iterator, const_reverse_iterator 도 좋지만 역시 쓸만한 것은 iterator 이다.**

```
iterator -------> const_iterator <-------base()----------const_reverse_iterator
iterator <---base()--->reverse_iterator----------------->const_reverse_iterator
```

요소 삽입과 삭제를 해주는 함수는 매개변수 타입으로 무조건 iterator를 요구한다.
따라서 iterator가 더 쓸만하다.

**27.const_iterator 를 iterator 로 바꾸는데에는 distance와 advance 를 사용한다.**

```
typdef deque intdeque ;
typedef intdeque::iterator iter ;
typedef intdeque::const_iterator ConsIter ;

intdeque d ;
ConstIter ci ;
....
iter i(d.begi()) ;
advence( i, distance( i, ci ) ;
```

**28.reverse_iterator 에 대응되는 기점 반복자를 사용하는 방법을 정확하게 이해하자.**

```
vector< int > v ;
v.reserve(5) ;

for(int i = 0 ; i<=5 ; i++ ) {
    v.push_back(i) ;
}

vector::reverse_iterator ri = find(v.rbein(), v.rend(), 3 ) ;
vector::iterator i(ri.base()) ;
```

```
         ri
1      | 2      | 3      | 4      |
                  i
```

* reverse_iterator 인 ri 로 지정된 위치에 대한 요소 삽입(요소의 삽입은 반보자가 가리키는 위치의 바로 앞에서 이루어진다.)을 흉내내려면 ri.base()를 사용한다.
  요소 삽입이 목적이라면 ri와 ri.base()는 동등하면 ri.base()는 ri에 정확히 대응되는 반복자이다
* reverse_iterator 인 ri 로 지정된 위치에 대한 요소 삭제를 흉내내려면 ri.base()의 앞에 있는 위치에서 삭제를 수행해야 한다.
  요소 삭제가 목적이라면 ri와 ri.base()는 동등하지 않으며 , ri.base()는 ri에 대응되는 iterator가 아니다.
  (v.erase(++ri).base()) -> ri를 전진한후 ri.base()를 호출하면 원래 삭제하려는 ri의 위치와 동등해진다.)

**29.문자 단위의 입력에는 istreambuf_iterator 이 사용도 적절하다.**

```
ifstream inputfile("data.txt") ;
string filedata((istreambuf_iterator(inputfile)), istreambuf_iterator()) ;
```

istreambuf_iterator 는 스트림 버퍼 안에서 다음위치에 있는 문자는 어느것이든 가지고 오는것이 이 반복자의 특성이고 속도도 istream_iterator 보다 훨씬 빠르다.

**30.알고리즘 데이터 기록 범위는 충분히 크게 잡자.**

목적지 범위를 지정해야 하는 알고리즘을 사용할때에는 언제나 이 목적지 범위의 크기를 미리 확보(reserve)해두는것이 좋다.

**31.정렬시 선택사항들을 제대로 파악해 놓자.**

* sort : 내림차순또는 오른차순으로 모든 데이터를 정렬한다. (vector, string, deque 만사용가능)
* stable_sort :  정렬시 순서 유지 정렬(stable sort)는 정렬 알고리즘에서 두개의 값이 동등하다면 정렬 후에도 그 둘의 상대적인 위치가 변하지 않는다.
  (vector, string, deque 만사용가능)
* partial_sort : 만약 상위 20위까지만 정렬하고 싶을때 부분 정렬이 사용한다. (vector, string, deque 만사용가능)
  ``partial_sort (w.begin(), w.begin() +20, w.end(), compare);``
* nth_element : 만약 상위 20위까지 순서에 상관없이 얻고싶을때 사용한다.
  ``nth_element (w.begin(), w.begin() +20, w.end(), compare) ;``
* partition :  주어진 범위에 있는 요소의 위치를 재배열하는데, 어떤 기준에 맞는 요소는 모두 그 범위의 앞부분에 몰아놓는다.
  ``vector::iterator partition_pos = partition(w.begin(), w.end(), compare()) ;``
  partition_pos 는 compare에 만족하지 않는 객체중 처음 것을 가리키는 반복자를 반환한다.

```
만족 하는 것들.........|만족 안하는 것들.................
                  ▲ partition_pos
```

list컨테이너는 멤버 한수인 sort를 사용해야한다. 위의 partial_sort, nth_element등은 사용할 수 없다.

**32.요소를 정말로 제거하고자 한다면 remove, unique 알고리즘에는 꼭 erase를 사용하자.**

remove는 어느 것도 진짜로 없애지 않는다. 없앨 수 없기 때문이다.
진짜로 요소를 제거하고 싶으면 remove뒤에 erase를 수행해야 한다.
단 list에서는 이 두개가 하나로 합쳐진 remove라는 멤버함수가 제공되고 있다. 따라서 list의 remove는 실제로 데이터를 삭제한다.

**33.remove와 비슷한 알고리즘을 포인터의 컨테이너에 적용할 때에는 각별히 조심하자.**

(객체의 포인터를 저장할때는 스마트포인터를 이용하여 저장하면 모든 것이 해결된다.)

**34.정렬된 범위에 대해 동작하는 알고리즘이 어떤 것인지 파악해 두자.**

* binary_search
* lower_bound
* upper_bound
* equal_range
* set_union
* set_intersection
* set_difference
* set_sysmmetric_difference
* merge : 병합정렬(merge sort) 알고리즘의 첫 단계(pass)와 똑같은 일을 한다고 보면 된다. 즉 정렬된 범위를 받아 이것을 합쳐 정렬된 하나의 범위를 만드는 것.
* inplace_merge :
* includes : 어떤 범위안에 우리가 원하는 값이 들어 있는지 여부를 알아볼 때 유용하다.
* unique(꼭 정렬될 필요는 없다.) : 정렬된 데이터에서 중복된 요소를 제거하는 것.(이것은 remove처럼 동작하므로 이후에 erase를 호출해야한다.)
* unique_copy(꼭 정렬될 필요는 없다.)

**35.대소문자를 구분하지 않는 문자열 비교는 mismatch 아니면, lexicographical_compare를 써서 구현할수있다.**

(대소문자를 구분하지 않는 문자열비교는 Exceptional C++ 항목 2,3 도 참고가 된다.)

**36.copy_if를 적절히 구현해 사용하자.**

**37.범위 내의 데이터 값을 요약하거나 더하는 데에는 accumulate나 for_each를 사용하자.**

* accumulate는 우리가 원하는 요약 결과를 바로 반환하지만 for_each는 자신이 매개변수로 받은 함수 객체를 반환하기 때문에 이 객체에서 요약정보를 뽑아내야 한다.

Functor 안에 result란 멤버 함수가 있는경우:
```
for_each(a.begin(), a.end(), Functor()).result() ;
```

**38.함수자 클래스는 값으로 전달되도록 설계하자.**

보통 일반 타입(int, double등)과 함수객체 그리고 반복자는 값으로 전달하는 것이 좋다.
함수객체는 최대한 작게 만들고 단형성으로 만들어야한다. 즉, 가상함수를 가지지 못하도록 해야 한다.

**39.술어 구문은 순수 함수로 만들자.**

* 술어 구문(predicate)란 bool값을 반환하는 함수를 말한다.
* 순수 함수(pure function)란 이 함수가 반환하는 값이 그 함수의 매개변수에 종속되는 함수를 말한다.
* 술어 구문 클래스(predicate class)란 operator()가 술어 구문인 함수자 클래스를 말한다.

"술어 구문 클래스의 operator()는 반드시 const로 만들자" 술어 구문 클래스의 oprator()가 순수 함수이어야 하고 , 이 제약은 술어구문 함수에도 적용된다.

```
class PredicateClass : public unary_function<Widget, bool> {
public:
   bool operator()(const Widget& w) const {
      ....
   }
   ...
}
```

**40.함수자 클래스는 어댑터(not1, not2, bind1st, bind2nd) 적용이 가능하게 하자.**

함수나 함수객체에 어댑터(not1, not2, bind1st, bind2nd) 를 사용하려면  몇가지 typedef가 정의되어 있어야 함께 사용할 수 있다.
이 typedef를 정의하기 위해서 기본적인 `std::unary_function``나 `std::binray_function`을 상속시키면 된다.

`std::unary_function`나 `std::binray_function`을 상속하는경우:

```
// 비포인터 타입은 const와 참조자를 빼는것이 관례이다
struct WidgetNNameCompare: std::binary_function<Widget, Widget, bool) {
   bool operator()(const Widget& lhs, const Widget& rhs) const ;
} ;

// 포인터타입은 똑같이 넘겨주면 된다.
struct WidgetNNameCompare: std::binary_function<const Widget*, const Widget*, bool) {
   bool operator()(const Widget* lhs, const Widge*  rhs) const ;
} ;
```

`std::unary_function`나 `std::binray_function`는 함수 객체 어댑터에소 요구하는 typedef를 제공해 주기 때문에 어댑터 적용이 가능한 함수 객체를 만들려면 이들 클래스를 상속하면 가장 좋다.

**41.`ptr_fun`, `mem_fun`, `men_fun_ref`의 존재에는 분명한 이유가 있다.**

STL에서 알고리즘이나 멤버함수에 사용되는 함수객체는 비멤버 함수에 대한 문법형태를 통해 호출된다.
따라서 클래스내의 멤버함수를 호출할수 없기때문에 이멤버함수를 비멤버 함수처럼 호출해주기 위해 `ptr_fun`, `mem_fun`, `men_fun_ref`가 필요한것이다.

* `ptr_fun` : STL컴포넌트에 일반 함수를 넘길 때:
```
void test( const Widget& w) ;
for_each( s.begin(), s.end(), ptr_fun(test)) ;
```
* `mem_fun` : 포인터를 갖고잇는 객체를 STL컴포넌트에 넘길때:
```
list<Widget*> lpw ;
for_each( s.begin(), s.end(), mem_fun(&Widget::test)) ;
```
* `mem_fun_ref` : 일반 객체를 STL컴포넌트에 넘길때:
```
list<Widget> lpw ;
for_each( s.begin(), s.end(), mem_fun_ref(&Widget::test)) ;
```

**42.`less<T>`는 `operator <`의 의미임을 꼭 알아두자**

**43.loop를 만들지 말고 알고리즘(특히 for_each, transform 를 많이 사용함)을 사용해라.** 효율, 정확성, 유지보수에 좋다.

**44.같은 이름을 가진것이 있다면 일반 알고리즘보다 멤버 함수가 더 낫다.**

find나 remove는 일반 알고리즘도 있고 특정 컨테이터의 멤버함수에도 있다. 이런경우 컨테이너의 멤버함수가 더 효율적이고 성능상 유리하다.

**45.count, find, binary_search, lower_bound, upper_bound, equal_range를 제대로 파악해두자.**

* count : 제가 찾는 값이 있나요 ? 있다면 몇개나 있나요 ? (선형시간-상등성)
* find : 제가 찾는 값이 있나여 ? 있다면 어디에 있나요 ? (선형시간-상등성)
* binary_search : 값이 있는지 없는지 ? (정렬된 데이터에 해당되며 로그시간이고 동등성을 사용)
* equal_range : 원하는 값이 있습니까 ? 있다면 어디에 있지요 ?(정렬된 데이터에 해당되며 로그시간이고 동등성을 사용)
* lower_bound : 원하는 값이 있나요 ? 있다면 첫째 사본은 어디에 있고 없다면 어디에 삽입되어야 하나요 ?
* upper_bound : 원하는 값이 있나요 ? 있다면 그 위치의 다음은 어디인가여 ?

**46.알고리즘의 매개 변수로는 함수 대신 함수 객체가 더 낫다.**

효율도 훨씬 빠르고, 컴파일 환경에도 견고하게 대응한다.

**47.쓰기 전용 코드는 만들지 말자.**

즉 난해하게 STL을 사용하지 말라는것이다.

**48.용도에 맞는 헤더를 항상 #include 하자.**

**49.STL 에 관련된 컴파일러 진단 메시지를 해석하는 능력을 가지자.**

**50.STL 관련 웹 사이트와 친해지자.**

- Original source: [http://blog.daum.net/shuaihan/15481898](http://blog.daum.net/shuaihan/15481898)
