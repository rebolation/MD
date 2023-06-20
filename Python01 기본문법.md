# Python 기본문법



### 로깅

```python
import logging
logging.basicConfig(level=logging.INFO, filename="log.txt")
logging.info(f"log...")
```



### 삼항 연산

```python
print("10억을 받았습니다" if False else "꿈을 꾸었습니다") # 꿈을 꾸었습니다
```



### 람다

```python
f=lambda x: x*2; print(f(1))                        # 2
print((lambda x: x > 3)(1))                         # False
x=1; print((lambda : x)())                          # 1
```



### 문자열

슬라이스

```python
print("012345"[::2])                                # 024
print("012345"[-4:])                                # 2345
print("012345"[::-1])                               # 543210
```

포맷

```python
print("{}, {}!".format("Hello", "world"))           # Hello, world!
s="world"; print(f"Hello, {s}!")                    # Hello, world!
```

대소문자

```python
print("a".islower())                                # True
print("a".upper())                                  # A
print("a".lower())                                  # a
print("aaaa".capitalize())                          # Aaaa
```

문자열 ↔ 리스트 변환

```python
print(list("aabbcc"))                               # ['a', 'a', 'b', 'b', 'c', 'c']
print("aa,bb,cc".split(","))                        # ['aa', 'bb', 'cc']
print("-".join(["aa","bb","cc"]))                   # aa-bb-cc
```



### 튜플

생성

```python
t=(); print(type(t))                                # <class 'tuple'>
t=(1, )                                             # (1,)
t=1, 2, 3, 4; print(t)                              # (1, 2, 3, 4)
arr=[1,2,3,4]; t=tuple(arr); print(t);              # (1, 2, 3, 4)
```

리스트로 변환

```python
t=1, 2, 3, 4; arr=list(t); print(arr);              # [1, 2, 3, 4]
```

언팩

```python
t=(1,2,3); print(t, *t)                             # (1, 2, 3) 1 2 3
t=(1,2,3); a,b,c = t; print(a,b,c);                 # 1 2 3
a,b,c = 1,2,3; print(a,b,c);                        # 1 2 3
```



### 리스트

추가, 삭제

```python
arr=[0,1,2]; arr.insert(1,100); print(arr)          # [0, 100, 1, 2]
arr=[0,1,2]; del arr[1]; print(arr)                 # [0, 2]
```

정렬

```python
arr=[3,2,1]; arr.sort(); print(arr)                 # [1, 2, 3]
print(sorted([3,1,2]))                              # [1, 2, 3]
print(sorted([3,1,2], reverse=True))                # [3, 2, 1]
print(sorted([[1,2],[1,1]]))                        # [[1, 1], [1, 2]]
print(sorted([[1,2],[1,1]], key=lambda a: a[0]))    # [[1, 2], [1, 1]]
print(sorted([[1,2],[1,1]], key=lambda a: a[1]))    # [[1, 1], [1, 2]]
print(sorted([[1,2],[1,1]], key=lambda a: (a[1],a[0])))    # [[1, 1], [1, 2]]
```

zip

```python
print(list(zip([1,2,3],[1,2,3])))                   # [(1, 1), (2, 2), (3, 3)]
print([x+y for x,y in zip([1,2,3],[1,2,3])])        # [2, 4, 6]
```

언팩

```python
t=[1,2,3]; print(t, *t)                             # [1, 2, 3] 1 2 3
t=[1,2,3]; a,b,c = t; print(a,b,c);                 # 1 2 3
```

스타 익스프레션

```python
a,b,*c = [1,2,3,4,5,6,7]; print(c);                 # [3, 4, 5, 6, 7]
_,_,*c = [1,2,3,4,5,6,7]; print(c);                 # [3, 4, 5, 6, 7]
*a,_,_ = [1,2,3,4,5,6,7]; print(a);                 # [1, 2, 3, 4, 5]
_,*b,_ = [1,2,3,4,5,6,7]; print(b);                 # [2, 3, 4, 5, 6]
a,b,*c = (1,2,3,4,5,6,7); print(c);                 # [3, 4, 5, 6, 7] (튜플)
```

리스트 컴프리헨션

```python
print([n**2 for n in range(10) if n % 2 == 0])      # [0, 4, 16, 36, 64]
```

map

```python
def f(x): return x%2==0;
print(list(map(f, [1,2,3])))                        # [False, True, False]
print(list(map(lambda x: x%2==0, [1,2,3])))         # [False, True, False]
print(list(map(lambda x,y: x+y, [1,2,3], [1,2,3]))) # [2, 4, 6]
```

filter

```python
print(list(filter(lambda x: x%2==0, [1,2,3,4,5])))   # [2, 4]
```

reduce

```python
from functools import reduce
print(reduce(lambda x,y: x+y, [1,2,3,4,5]))          # 15
```



### 세트

생성

```python
s={1,1,1,1,1,2,2,2,2,3,3,3,4,4,5}; print(s)         # {1, 2, 3, 4, 5}
```

연산

```python
a,b={1,2},{2,3}; print(a|b, a.union(b))                # {1, 2, 3} {1, 2, 3}
a,b={1,2},{2,3}; print(a&b, a.intersection(b))         # {2} {2}
a,b={1,2},{2,3}; print(a-b, a.difference(b))           # {1} {1}
a,b={1,2},{2,3}; print(a^b, a.symmetric_difference(b)) # {1, 3} {1, 3}
a,b={1,2},{1,2,3}; print(a<=b, a.issubset(b))          # True True
a,b={1,2},{1,2,3}; print(b>=a, b.issuperset(a))        # True True
a,b={1,2},{1,2,3}; print(a<b)                          # True
a,b={1,2},{1,2,3}; print(b>a)                          # True
c,d={1,2},{1,2}; print(c==d)                           # True
e,f={1,2},{2,3}; print(e!=f)                           # True
g={1}; print(g.isdisjoint({2}), g.isdisjoint({1,2}))   # True False
s={1}; s.add(2); s.remove(2); s.remove(2);             # 에러
s={1}; s.add(2); s.discard(2); s.discard(2); print(s); # {1}
s={1,2}; print(s.pop()); s.clear(); print(len(s));     # 1 0
```

세트 컴프리헨션

```python
print({i for i in range(10) if i < 5});                # {0, 1, 2, 3, 4}
```



### 레인지

range 사용

```python
print(tuple(range(3)))                              # (0, 1, 2)
print(tuple(range(1,4)))                            # (1, 2, 3)
print(tuple(range(1,10,2)))                         # (1, 3, 5, 7, 9)
print(tuple(range(9,0,-2)))                         # (9, 7, 5, 3, 1)
```



### 딕트

생성, 삭제, 갱신

```python
d={"a":1,"b":2,"c":3}; print(d)                     # {'a': 1, 'b': 2, 'c': 3}
del d["a"]; print(d)                                # {'b': 2, 'c': 3}
d={"a":1,"b":2}; d.update({"c":3}); print(d);       # {'a': 1, 'b': 2, 'c': 3}
```

zip

```python
k=("a","b","c"); v=(1,2,3); print(dict(zip(k,v)))   # {'a': 1, 'b': 2, 'c': 3}
```

딕트 컴프리헨션

```python
print({k:v*2 for k, v in zip(("a","b"), (1,2))})    # {'a': 2, 'b': 4}
```

키, 밸류

```python
d={"a":1,"b":2}; arr=list(d.keys()); print(arr);    # ['a', 'b']
d={"a":1,"b":2}; arr=list(d.values()); print(arr);  # [1, 2]
d={"a":1,"b":2}; print(sum(list(d.values())));      # 3
```

정렬

```python
from operator import itemgetter, attrgetter
print(sorted([{'a':1},{'a':0}], key=itemgetter('a')))  # [{'a': 0}, {'a': 1}] - faster
print(sorted([{'a':1},{'a':0}], key=lambda x: x['a'])) # [{'a': 0}, {'a': 1}]
```

in

```python
print("a" in {"a":1,"b":2,"c":3})                   # True
print(1 in {"a":1,"b":2,"c":3}.values())            # True
```

is, copy()

```python
d={"a":1}; d2=d; d["a"]=10; print(d2, d is d2);     # is
d={"a":1}; d2=d.copy(); d["a"]=10; print(d2, d is d2);# copy()
```



### 시퀀스 객체와 반복 가능한 객체

> 반복 가능한 객체 : 문자열, 튜플, 리스트, 세트, 레인지, 딕셔너리 (요소의 순서가 정해져 있지 않음)
>
> 시퀀스 객체 : 문자열, 튜플, 리스트, 레인지 (요소의 순서가 정해져 있음)

in, not in

```python
print(10 in range(100))                             # True
print('a' in ['a','c'])                             # True
print('a' not in {'a','c'})                         # False
```

더하기, extend

```python
print(list(range(3)) + list(range(3)))              # [0, 1, 2, 0, 1, 2]
s=[1]; s.extend([2]); s+=[3]; print(s)
```

반복

```python
print([1,2,3] * 2)                                  # [1, 2, 3, 1, 2, 3]
```

인덱스

```python
print([1,2,3][0])                                   # 1
print([1,2,3][-1])                                  # 3
```

슬라이싱

```python
print([1,2,3][:2])                                  # [1, 2]
print((1,2,3,4,5,6,7,8,9,10)[::2])                  # (1, 3, 5, 7, 9)
```

대체

```python
s=[1,2,3,4,5]; s[0:5:2]=[0]*3; print(s)             # [0, 2, 0, 4, 0]
```

len

```python
print(len((1,2,3)))                                 # 3
```

min, max

```python
print(min([1,2,3,4,5,6,7,8,9,10]))                  # 1
print(max([1,2,3,4,5,6,7,8,9,10]))                  # 10
```

count

```python
print([1,2,2,3,3,3].count(3))                       # 3
```

reverse

```python
s=[1,2,3]; s.reverse(); print(s)                    # [3, 2, 1]
```

insert, pop

```python
s=[1,2,3]; s.insert(0,9); s.pop(); s.pop(1); print(s) # [9, 2]
```

del, remove

```python
a=[1,2,3]; del a[1]; a.remove(3); print(a)          # [1]
```

for 문

```python
for i in [1,2,3]: print(i)                          # 1  2  3
for i in range(5, 0, -1): print(i)                  # 5 4 3 2 1
for i in enumerate([10,20,30]): print(i)            # (0, 10) (1, 20) (2, 30)
```



### 스코프

LEGB

```python
g = 0                                               # global variable
l = 0                                               # global variable
def func():
    global g
    g = 1                                           # global variable
    l = 1                                           # local variable
func()                                              
print('g :', g)                                     # 1
print('l :', l)                                     # 0
```



### 함수 인자

포지션 방식 호출과 키워드 방식 호출

```python
def f(a,b,c): print(a,b,c)             
f(1,2,3)                                            # 1 2 3
f(c=1,b=2,a=1)                                      # 3 2 1
```

가변인자(함수 정의 시 아규먼트에 *를 붙임) - 다수의 파라미터들을 하나의 튜플이라고 생각해보자...

```python
def f(*arguments): print(arguments)
f(1);                                               # (1,)
f(1,2,3);                                           # (1, 2, 3)
```

키워드 가변인자(함수 정의 시 아규먼트에 **를 붙임)

```python
def f(**keywordarguments): print(keywordarguments)
f(c=3,b=2,a=1)                                      # {'c': 3, 'b': 2, 'a': 1}
```

가변인자 + 키워드 가변인자(함수 정의 시 아규먼트에 * 와 **를 붙임)

```python
def f(*pa, **ka): print(pa, ka)
f(1,2,c=3)                                          # (1, 2) {'c': 3}
```

디폴트인자(함수 정의 시 아규먼트에 기본값 지정)

```python
def f(a,b,c=3): print(a,b,c)
f(1,2)                                              # 1 2 3
```

함수의 디폴트 인자가 리스트, 딕트 등이면 함수 종료 후에도 살아남음

```python
def f(x, l=[]): l += [i for i in range(x)]; print(l)  # 디폴트 인자가 리스트
f(2); f(2); f(2);
# [0, 1]
# [0, 1, 0, 1]
# [0, 1, 0, 1, 0, 1]
```

언팩(파라미터를 넘겨줄 때 * 또는 **를 붙임)

```python
def f(a,b,c): print(a,b,c)    
f(*[1,2,3]);                                        # 1 2 3
f(**{'a':1,'b':2,'c':3})                            # 1 2 3
```



### 클래스

기본 자료형들은 클래스.. 따라서 메소드가 존재함

```python
print(int, float, str, tuple, list, set, range, dict) # <class 'int'> ...
print(isinstance(2.0, float))                         # True
print((-2.0).is_integer())                            # True
```

(비공개)클래스 속성, (비공개)인스턴스 속성, 생성자

```python
class File:
    files = []                                        # 클래스 속성
    __pwd = "C:\\"                                    # 비공개 클래스 속성
    def __init__(self, name):                         # 생성자(매직메소드:시작과 끝이 __)
        self.__name = name                            # 비공개 인스턴스 속성
        File.files.append(name)                       # 클래스 속성
```

(비공개)클래스 메소드, (비공개)인스턴스 메소드, 스태틱 메소드

```python
class File:
    ....
    def info(self):                                   # 인스턴스 메소드
        print(File.__pwd)                             # 비공개 클래스 속성
        print(self.__name)                            # 비공개 인스턴스 속성    
    @staticmethod
    def filelist():                                   # 스태틱 메소드
        print(File.files)
    @classmethod
    def newfile(cls,name):                            # 클래스 메소드
        return cls(name)                              # __init__ 호출
        
f1 = File("autoexec.bat")
f2 = File.newfile("config.sys")
File.filelist()                                       # ['autoexec.bat', 'config.sys']
f2.info()                                             # C:\ config.sys
```

상속, super()

```python
class A:
    def __init__(self):
        print("A.__init__")
class B(A):
    def __init__(self):
        super().__init__()                            # super().__init__()
        print("B.__init__")
```

다중 상속

```python
class A: pass 
class B: pass 
class C(A,B): pass                                    # 다중 상속
print(issubclass(C, B), C.mro())                      # issubclass, mro()
```

추상클래스

```python
from abc import *
class A(metaclass=ABCMeta):                           # 추상 클래스
    @abstractmethod
    def implement_me(self):
        pass
class B(A):
    def implement_me(self):
        pass
```



### 예외 처리

try, except, else, finally

```python
try:
    raise Exception('raise Exception!')
except ZeroDivisionError as e:    
    print(e)
except Exception as e:
    print(e)
else:
    print('OK')
finally:
    print('the end')
```

예외 전파

```python
def bug():
    raise Exception()
def bug_try():
    try:
        raise Exception('bug_try')
    except Exception as e:
        print('inside:', e)
def bug_reraise():
    try:
        raise Exception('bug_reraise')
    except Exception as e:
        print('inside:', e)
        raise                             # re-raise

try:
    bug()                                 # outside
    bug_try()                             # inside
    bug_reraise()                         # inside, outside
    raise Exception('raise Exception!')   # never run
except MyException as e:
    print('outside:', e)
except Exception as e:
    print('outside:', e)
else:
    print('OK')
finally:
    print('the end')
```

커스텀 예외

```python
class MyException(Exception):             # custom exception
    def __init__(self):
        super().__init__('MyException...')
def bug():
    raise MyException()
bug()                                     # __main__.MyException: MyException...
```



### 매직 메소드





### 제너레이터

yield와 yield from 으로 생성

```python
def myg(): yield 0; yield 1; yield 2;
def myg(): f=[0,1,2]; yield from f;
g = myg()
```

제너레이터 컴프리헨션으로 생성(괄호()로 감쌈)

```python
g=(i for i in range(10) if i % 2 == 0); print(g)    # <generator object ... >
l=[i for i in range(10) if i % 2 == 0]; print(l)    # [0, 2, 4, 6, 8] (not a generator)
```

\__next__와 next() 로 다음 번 yield 까지 실행

```python
g=(i for i in range(10)); print(g)                  # <generator object ... >
print(g.__next__()); print("outside")               # 0 outside
print(next(g)); print("outside")                    # 1 outside
```

for in 으로 마지막 yield 까지 실행

```python
g=(i for i in range(10)); print(g)                  # <generator object ... >
for i in g: print(i)                                # 0 1 2 3 4 5 6 7 8 9
```

StopIteration 예외

```python
g=(i for i in range(10)); print(g)                  # <generator object ... >
for i in g: print(i)                                # 0 1 2 3 4 5 6 7 8 9
print(next(g))                                      # StopIteration (Exception)
```



### 데코레이터

기본형(파라미터 없음)

```python
def mydeco(f):                                      # deco func
    def wrapper(): print("my!"); f();
    return wrapper
    
class Mydeco:                                       # deco class
    def __init__(self, func): self.func=func
    def __call__(self): print("my!"); self.func()
    
@mydeco                                             # using decorator
def hello(): print('hello world')
hello()    
```

확장형1(함수 파라미터)

```python
def mydeco(f):                                      # deco func (func param)
    def wrapper(p): print("my!"); f(p);
    return wrapper
    
class Mydeco:                                       # deco class (func param)
    def __init__(self, func): self.func=func
    def __call__(self, *args, **kwargs):
        print("my!"); self.func(*args, **kwargs)
        
@mydeco                                             
def hello(p): print(p, 'world')
hello("hello")
```

확장형2(데코레이터 파라미터와 함수 파라미터)

```python
def mydeco(dp):                                     # deco func (deco param)
    def real_decorator(func):
        def wrapper(p): print(dp); func(p)
        return wrapper
    return real_decorator
    
class Mydeco:                                       # deco class (deco param)
    def __init__(self, dp): self.dp=dp
    def __call__(self, func):
        def wrapper(*args,**kwargs):
            print(self.dp); func(*args,**kwargs)
        return wrapper
        
@Mydeco("my!")
def hello(p): print(p, 'world')
hello("hello")
```



### 멀티스레딩

```python
import threading
import time
class Worker(threading.Thread):
    def run(self):
        print("start")
        time.sleep(2)
        print("end")
threads = []
for i in range(5):
    t = Worker()
    t.start()
    threads.append(t)
print("1 - no mainthread wait")
for t in threads:
    t.join()                                                # join
print("2 - mainthread wait (join)")
for i in range(5):
    t = Worker()
    t.daemon = True                                         # daemon
    t.start()
print("3 - no daemon end (mainthread end)")
```



### 멀티프로세싱



### 코루틴

필요할 때 정리 예정...

```
# yield 또는 async/await를 사용해 메인루틴과 코루틴을 교대로 수행한다는 정도만 기억...
```

```python
def myco():
    while True:
        print("myco")
        x = (yield)
        print(x)
        
co = myco()
next(co) # co.send(None)
print("------")
co.send("1")
print("------")
co.send("2")
print("------")
co.send("3")
```



### 참고

- [공식 문서](https://docs.python.org/ko/3)
- [파이썬 코딩 도장](https://dojang.io/course/view.php?id=7)
- [레벨업 파이썬](https://wikidocs.net/70861)

