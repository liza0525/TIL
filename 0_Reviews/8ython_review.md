# 8ython 총정리

> 확실하게 알아야 하는 개념, 몰랐는데 알게 된 개념, 잘 외워지지 않은 것 위주로 정리

## Day 1  - Python Intro

### 식별자 법칙

- 영문알파벳, 언더바, 숫자로 구성 / 첫글자에 숫자 x / 대소문자 구별

- 예약어 사용 불가 :assert, nonlocal, yield 등

### 기초 문법

- 인코딩 선언시 다음과 같이 한다. (기본 설정 :  UTF-8)

```python
#-*-coding:<encoing-name>-*-
```

- Docstring : 어떤 함수 등을 설명하기 위해 multi-line comment를 넣은 것, 함수.클래스 선언 후 설명을 하기 위해 사용
- 파이썬에서 한줄 코딩 시 세미콜론 이용 가능(거의 쓰진 않음)

```python
print("happy") print("hacking") # 이건 SyntaxError
print("happy"); print("hacking") # 이건 compile 됨
```



- print문 내에 줄 바꿈을 하고 싶으면 백슬래시를 쓴다.
- `[]` `{}` `()`는 `\` 없이도 가능하다.

### 변수(variable) 및 자료형

- 자료형 확인은 type(), 해당 변수의 메모리 주소 확인은 id()

- 서로 다른 변수라도 같은 값을 할당하면 메모리 주소 값은 동일하게 나온다.

  다른 값을 할당할 때, 메모리 주소 값이 달라짐

- 동시에 두 변수에 두 값을 할당할 땐 튜플의 형식으로 할당한다.

  - 변수의 개수가 더 많을 땐 TypeError, 변수의 개수가 더 적을 땐 ValueError가 난다.
  - 두 변수의 값을 바꾸고 싶을 땐 다음과 같이 해도 된다.

  ```python
  x, y = 5, 10
  x, y = y, x
  ```

  

- 자료형 : 기본적으로 3가지 뿐이다! **(숫자, 글자, boolean)**



1. 수치형

- int형/ float형 (파이썬 3.x엔 long 타입은 없다.)

- 0o(8진수), 0b(2진수), 0x(16진수)

- arbitrary-precision arithmetic

  아주 큰 정수를 표현할 때 사용하는 메모리의 크기 변화

  수를 표현하는 데에 byte가 부족하면 더 끌어다 쓰는 개념

```python
import sys
max_int = sys.maxsize
print(max_int)
big_num = max_int * max_int
print(big_num)
```

- float(부동소수점) : 실수를 표현할 때 부동 소수점을 이용하기 때문에 실제 세계에선 값이 같아도, 컴퓨터 내에서는 다를 수 있다.

  e-2(10의 -2승)

- 처리방법

```python
a = 3.5-3.15
b = 0.35
print(a-b)
print(1e-10)
abs(a - b) <=1e-10
```

```python
import sys
a = 3.5-3.15
b = 0.35
print(abs(a-b) <= sys.float_info.epsilon)
```

```python
import math
a = 3.5-3.15
b = 0.35
print(math.isclose(a,b))
#python 3.5 버전부터!
```

- complex(복소수) : 허수부는 j로 표현 (예/ a = 3+4j)
  - .conjugate() 켤레복소수 / .imag 허수부(float) / .real 실수부(float)

2. Bool

- True, False
  - False : 0, None(NoneType class), 빈 리스트 또는 빈 딕셔너리 또는 빈 문자열

3. 문자열

- Escape 문자열 : \0 null \r 캐리지리턴 
- 여러줄에 걸쳐 문자열 출력시 docstring 이용, Double quotes를 쓰도록!

```python
print("""여러줄에
    걸쳐서
    출력하기
""")
```

- String interpolation
  - %-formatting(다른 formatting은 숙지했으니 생략) : c나 java의 pirntf()와 비슷

  ```python
  print('이름은 %s, 나이는 %s, 전공은 %s, 사는 곳는 %s입니다.' % (name, age, major, address))
  ```



### 연산자

```python
# divmod는 나눗셈과 관련된 함수입니다.
quotient, remainder = divmod(5,2)
print(quotient, remainder)

#또는
quotient = 5 // 2
remainder = 5 % 2
```

- 논리 연산자에서..
  - 파이썬에서 and는 a가 거짓이면 a를 리턴하고, 참이면 b를 리턴한다.
  - 파이썬에서 or은 a가 참이면 a를 리턴하고, 거짓이면 b를 리턴한다.

- Concatenation : 숫자가 아닌 자료형은 +로 합칠 수 있다. (str, list, tuple 등, dict는 안됨)

- Identity : `is` 연산자로 동일한 object인지 확인

```python
# is는 맛만 봅시다.
# 파이썬에서 -5부터 256까지의 id는 동일합니다.
a = 3
b = 3
a is b
# True

# id는 다르죠!
a = 257
b = 257
a is b
# False
```

- 연산자 우선순위

> `()`을 통한 grouping
>
> Slicing
>
> Indexing
>
> 제곱연산자 **
>
> 단항연산자 +, - (음수/양수 부호)
>
> 산술연산자 *, /, %
>
> 산술연산자 +, -
>
> 비교연산자, `in`, `is`
>
> ``not`
>
> `and`
>
> `or`



### 기초 형변환

- 암시적 형변환(파이썬이 알아서 형변환)
  - True + 3 => 4
  - int + float => float
  - int/float + complex => complex

- 명시적 형변환
  - int(), float(), str(), list(), tuple() 등
  - string 3.5를 int로 변환하면 ValueError

### 시퀀스 자료형

> list , tuple, range, string, binary(이건 안 다룸)

- tuple은 수정이 전혀 불가능하다!(immutable)

- ```python
  l = list(range(0,31,3))
  # l[::3]
  ```

> Set, Dictionary

- set은 중복값 x
- dict은 key와 value가 한 쌍
  - 동일한 key에 다른 value를 넣으려 하면 이전 value는 없어진다.
  - key는 immutable한 모든 것이 가능

- immutable : string tuple range

  mutable : list set dictionary



## Day 2- control_of_flow

- if문과 for문을 쓸 때 4spaces를 지켜 써야 한다(PEP-8권장)

### if 문

- 조건 표현식

```python
a = int(input())

value = a if a >= 0 else 0
#이는 아래의 코드와 같다.
if a >= 0 :
	value = a
else:
	value = 0
```



### for문

- enumerate() : list의 index와 value를 tuple 형식으로 반환한다

```python
# enumerate()를 활용해서 출력해봅시다.
lunch = ['짜장면', '초밥']
for idx, menu in enumerate(lunch):
    print(menu)
    print(idx)
print(list(enumerate(lunch)))
print(list(enumerate(lunch, start=1)))
'''
결과
===============================
짜장면
0
초밥
1
[(0,'짜장면'),(1,'초밥')]
[(1,'짜장면'),(2,'초밥')]
'''
```

```python
for key in dict:
for key in dict.keys():
# 위 두가지 for문 표현은 같은 것
for value in dict.values():
for key, value in dict.items():
```



### break, continue, else(, pass)

- else : for문이 끝까지 반복된 경우에만 시행 (break가 걸리면 시행이 안됨)



## Day2, 3 - Function

- Computer Science(Programming)에서의 가장 큰 challenge

  : complexity(복잡도) ==[해결책]==> **Abstraction(요약)**

  Computating Thinking보다 중요한 게 Abstraction 잘하는 것이 더 중요!

  Abstraction을 구현하기 위한 수단이 **함수(function)**

- `dir(__builtins__)` : 내장함수들의 목록 (대문자 : class/소문자 : 함수 또는 변수)

- 함수의 return : 무조건 한 개의 '객체'만 반환된다!(tuple, list 등으로 여러 값을 반환할 수는 있음)

- 함수 인자의 기본 값 : 함수가 호출될 때, 인자를 지정하지 않아도 기본 값을 설정 가능

```python
def func(p1=v1):
    return p1

print(func())
```

```python
def greeting(name='john', age):
    print(f'{name}은 {age}살입니다.')
'''
기본 매개 변수 이후 기본 값이 없는 매개 변수를 넣으면 안된다.
SyntaxError
'''
```

```python
def greeting(age, name='john'):
    print(f'{name}은 {age}살입니다.')

greeting(age=30, name='tom')
greeting(name='tom', age=30)
# 위와 같이 모두 쓸 수 있음
```

### 가변인자 리스트

```python
def func(*args):
# tuple 형식으로 처리가 된다.
```

### 정의 되지 않은 인자 처리 (**kwarg)

```python
# 아래에 코드를 작성해주세요.
def fake_dict(**kwargs):
    result = []
    for key, value in kwargs.items():
        result.append(f'{key}: {value}')
    print(', '.join(result))
        
fake_dict(한국어='안녕', 영어='hi', 독일어='Guten Tag')
```

### Dictionary 인자로 넘기기(unpacking arguments list)

```python
def user(username, password, password_confirmation):
    if password == password_confirmation:
        print(f'{username}님, 회원가입이 완료되었습니다.')
    else:
        print('비밀번호와 비밀번호 확인이 일치하지 않습니다.')
        
my_account = {
    'username': '홍길동',
    'password': '1q2w3e4r',
    'password_confirmation': '1q2w3e4r'
}

user(**my_account)
```



### 이름 공간 및 스코프(Scope)

> `L`ocal scope: 정의된 함수 (함수 실행 -> 리턴)
>
> `E`nclosed scope: 상위 함수(함수 실행 ->리턴)
>
> `G`lobal scope: 함수 밖의 변수 혹은 import된 모듈(모듈 호출 또는 이름 선언 -> 끝)
>
> `B`uilt-in scope: 파이썬안에 내장되어 있는 함수 또는 속성(파이썬 실행 ->끝)





## Day 4 - data_structures

-  각 iterable 자료형에 대한 모듈은 기존 markdown 볼 것!

|  자료형  |                         메소드 종류                          |
| :------: | :----------------------------------------------------------: |
|  문자열  | `.capitalize()` 앞글자만 대문자로 `.title()` 어포스트로피나 공백 이후 문자를 대문자로 `.upper()` 모두 대문자로 `.lower()` 모두 소문자로 `.swapcase()` 대<->소문자 변경 `.join(iterable)` 해당 문자를 문자열 사이에 삽입 `.replace(old, new[, count])` 문자열을 갯수만큼 변경 `.strip([chars])` 양 끝에 있는 공백이나 \n을 삭제 `.fin(x)` x의 첫번째 위치 반환, 없으면 -1 반환 `.index(x)` x의 첫번째 위치 반환, 없으면 error `.isalpha()` `.isdecimal()` `.isdigit()` `.isnumeric()` `.isspace()` `.istitle()` `.islower()` |
|  리스트  | `.append(x)` 값 추가 `.extend(iterable)` list와 list를 붙여준다 `.insert(i,x)` i번째 인덱스에 x 넣기 `.remove(x)` 가장 앞에 있는 x 지우기 `.pop(i)` i번째 인덱스의 값을 지우며 반환 `.index(i)` i번째 인덱스에 있는 값 반환 `.count(x)` x의 갯수 반환 `.sort()` `sorted()` `.reverse()` `reversed()` |
| 딕셔너리 | `.pop(key[, default])` 해당 key의 값을 없애고 반환, key가 없을 시엔 default 반환 `.update(key=new_value)` 해당 key의 값을 변경 `.get(key[,default])` 해당 key의 값을 반환, 해당 key가 없으면 None 반환 |
|   세트   | `.add(item)`, `.update(*other)` iterable한 값을 넣는다. `.remove(elem)`, `.discard(elem)` 값 삭제, set에 없는 값을 삭제해도 error 없다. `.pop()` |



### 리스트 복사(얕은 복사, 깊은 복사)

- list의 경우에는 값이 복사가 되는 것이 아닌 주소가 복사된다.

```python
num = [1, 2, 3]
num2 = num
num2[0] = 0
print(num)

num3 = num[:]
print(num3)

'''
결과
[0,2,3]
[0,2,3]
'''

# 다른 방법으로 복사해봅시다.
a = [1,2,3]
b = list(a) # 딕셔너리 복사도 비슷하다. a라는 어떤 딕셔너리가 있을 때, b = dict(a) 이런식으로
# tuple은 어차피 원본 변경 자체가 되지 않는 자료형이므로 이 개념과는 무관하다.
print(b)
'''
결과
[1,2,3]
'''
```

어쨌거나 값이 복사가 되는 것이 아니기 때문에 **얕은 복사**라고 한다.

- 깊은 복사

```python
import copy

matrix = [
    [1,2,3],
    [4,5,6],
]
matrix3 = copy.deepcopy(matrix)
```



### List Comprehension

```python
numbers = list(range(1, 11))
cubic_list = [num ** 3 for num in numbers]
print(cubic_list)
```

```python
# 짝수 만들기
numbers = list(range(1, 11))
even_list = [num for num in numbers if num%2 == 0]
print(even_list)
```



### Dictionary Comprehension

```python
cubic_dict = { x: x ** 3 for x in range(1,11)}
print(cubic_dict)
```

```python
dusts = {'서울': 72, '대전': 82, '구미': 29, '광주': 45, '중국': 200}
dusts_value = {key:'나쁨' if value > 80 else '보통' for key, value in dusts.items()}
print(dusts_value)
```

```python
dusts = {'서울': 72, '대전': 82, '구미': 29, '광주': 45, '중국': 200}
dusts_value2 = {key:'매우 나쁨' if value > 150 else '나쁨' if value > 80 else '보통' for key, value in dusts.items()}
print(dusts_value2)
```



### map(), zip(), filter()

- **map(function, iterable)** : 해당 함수에 iterable한 자료형의 값을 넣어 그 return 값들의 모음을 map object로 반환
- **zip(*iterable)** : 복수의 iterable 자료형을 매칭하여 tuple 모음의 zip object로 반환
- **filter(function, iterable)** : 반환 값이 **참인 것**들만 구성하여 filter object로 반환



## Day 6, 7 - OOP with python

- 클래스 = 속성 + 행위의 집합체

### 클래스 및 인스턴스

- 클래스는 선언과 동시에 클래스 객체가 생성된다. (local scope)

```python
# Phone 클래스를 만들어봅시다.
class Phone:
    power = False
    number = ''
    book = {}
    model = 'Samsung Galaxy S10'
    
    def on(self):
        if not self.power:
            self.power = True
            print('--------------------------')
            print(f'{self.model}')
            print('--------------------------')
            
    def off(self):
        if self.power:
            self.power = False
            print('전원이 꺼집니다.')# Phone 클래스를 만들어봅시다.
class Phone:
    power = False
    number = ''
    book = {}
    model = 'Samsung Galaxy S10'
    
    def on(self):
        if not self.power:
            self.power = True
            print('--------------------------')
            print(f'{self.model}')
            print('--------------------------')
            
    def off(self):
        if self.power:
            self.power = False
            print('전원이 꺼집니다.')
```



- 인스턴스 객체는 class()를 호출함으로써 선언된다.
  - 이름공간 탐색 시 인스턴스 -> 클래스 -> 전역 순으로 탐색

- 용어 정리

```python
class Person:                     #=> 클래스 정의(선언) : 클래스 객체 생성
    name = 'unknown'              #=> 멤버 변수(data attribute)
    def greeting(self):           #=> 멤버 메서드(메서드)
        return f'{self.name}' 
richard = Person()      # 인스턴스 객체 생성
tim = Person()          # 인스턴스 객체 생성
tim.name                # 데이터 어트리뷰트 호출
tim.greeting()          # 메서드 호출
```

### 클래스-인스턴스간의 이름공간

> - 클래스를 정의하면, 클래스 객체가 생성되고 해당되는 이름 공간이 생성된다.
> - 인스턴스를 만들게 되면, 인스턴스 객체가 생성되고 해당되는 이름 공간이 생성된다.
> - 인스턴스의 어트리뷰트가 변경되면, 변경된 데이터를 인스턴스 객체 이름 공간에 저장한다.
> - 즉, 인스턴스에서 특정한 어트리뷰트에 접근하게 되면 인스턴스 => 클래스 순으로 탐색을 한다.



### 생성자 / 소멸자

```python
def __init__(self):
    print('생성될 때 자동으로 호출되는 메서드입니다.')

def __del__(self):
    print('소멸될 때 자동으로 호출되는 메서드입니다.')
    
def __repr__(self):
        '''
        객체가 표현될 때 쓰이는 문자열
        '''
def __str__(self):
    
# 위의 형식처럼 양쪽에 언더스코어가 있는 메서드를 스페셜 메서드 혹은 매직 메서드라고 부른다
```

- 객체를 지우기 => del instance_name



## Day 7, 8 - OOP advanced

### 클래스 변수 / 인스턴스 변수

- 클래스 변수
  - 클래스의 속성입니다.
  - 클래스 선언 블록 최상단에 위치합니다.
  - `Class.class_variable` 과 같이 접근/할당합니다.

  ```python
    class TestClass:
        class_variable = '클래스변수'
        ...
  
    TestClass.class_variable  # '클래스변수'
    TestClass.class_variable = 'class variable'
    TestClass.class_variable  # 'class variable'
  
    tc = TestClass()
    tc.class_variable  # 인스턴스 => 클래스 => 전역 순서로 네임스페이스를 탐색하기 때문에, 접근하게 됩니다.
  ```

- 인스턴스 변수
  - 인스턴스의 속성입니다.
  - 메서드 정의에서 `self.instance_variable` 로 접근/할당합니다.
  - 인스턴스가 생성된 이후 `instance.instance_variable` 로 접근/할당합니다.

  ```python
    class TestClass:
        def __init__(self, arg1, arg2):
            self.instance_var1 = arg1
            self.instance_var2 = arg2
  
        def status(self):
            return self.instance_var1, self.instance_var2   
  
    tc = TestClass(1, 2)
    tc.instance_var1  # 1
    tc.instance_var2  # 2
    tc.status()  # (1, 2)
  ```

### 인스턴스 메서드 / 클래스 메서드 / 스태틱(정적) 메서드

- 인스턴스 메서드
  - 인스턴스가 사용할 메서드 입니다.
  - **정의 위에 어떠한 데코레이터도 없으면, 자동으로 인스턴스 메서드가 됩니다.**
  - **첫 번째 인자로 self 를 받도록 정의합니다. 이 때, 자동으로 인스턴스 객체가 self 가 됩니다.**

  ```python
    class MyClass:
        def instance_method_name(self, arg1, arg2, ...):
            ...
  
    my_instance = MyClass()
    my_instance.instance_method_name(.., ..)  # 자동으로 첫 번째 인자로 인스턴스(my_instance)가 들어갑니다.
  ```

- 클래스 메서드
  - 클래스가 사용할 메서드 입니다.
  - **정의 위에 @classmethod 데코레이터를 사용합니다.**
  - **첫 번째 인자로 cls 를 받도록 정의합니다. 이 때, 자동으로 클래스 객체가 cls 가 됩니다.**

  ```python
    class MyClass:
        @classmethod
        def class_method_name(cls, arg1, arg2, ...):
            ...
  
    MyClass.class_method_name(.., ..)  # 자동으로 첫 번째 인자로 클래스(MyClass)가 들어갑니다.
  ```

- 스태틱(정적) 메서드
  - 클래스가 사용할 메서드 입니다.
  - **정의 위에 @staticmethod 데코레이터를 사용합니다.**
  - **인자 정의는 자유롭게 합니다. 어떠한 인자도 자동으로 넘어가지 않습니다.**

  ```python
    class MyClass:
        @staticmethod
        def static_method_name(arg1, arg2, ...):
            ...
  
    MyClass.static_method_name(.., ..)  # 아무일도 자동으로 일어나지 않습니다.
  ```

- Overloading

  - `__gt__` : greater than, 연산자 overload하는 것을 연습
  - `__eq__` : equal to
  - `__ge__` : greater than and equal to

```python
class Person:
    population = 0
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
        Person.population += 1
        
    def greeting(self):
        print(f'{self.name} 입니다. 반갑습니다!')
        
    def __repr__(self):
         return f'< "name:" {self.name}, "age": {self.age} >'
    
    def __gt__(self, other):
        return self.age > other.age
    
    def __eq__(self, other):
        return self.age == other.age
    
    def __ge__(self, other):
        return self.age >= other.age
    # 나이를 기준으로 gt eq ge 재정의(오버로드)
    # lt le ne 모두 알아서 정의됨
    
p1 = Person('john',34)
p2 = Person('ashley',32)

p1 > p2 # True
```



## 상속

### 기초

- 클래스에서 가장 큰 특징은 '상속' 기능을 가지고 있다는 것이다.
- 부모 클래스의 모든 속성이 자식 클래스에게 상속 되므로 코드재사용성이 높아집니다.

```python
class DerivedClassName(BaseClassName):
    code block

class SubClass(SuperClass):

class ChildClass(ParentClass):
```

- super()
  - 자식 클래스에 메서드를 추가 구현할 수 있습니다.
  - 부모 클래스의 내용을 사용하고자 할 때, `super()`를 사용할 수 있습니다.

- 메서드 오버라이딩
  - 메서드를 재정의할 수도 있습니다

### 상속관계에서의 이름공간

- 기존에 인스턴스 -> 클래스순으로 이름 공간을 탐색해나가는 과정에서 상속관계에 있으면 아래와 같이 확장됩니다.
- instance => class => global
- 인스턴스 -> 자식 클래스 -> 부모 클래스 -> 전역