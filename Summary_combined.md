## 01 Reference count와 Garbage collection
* Reference count가 0이 되면 메모리에서 없어진다.
## 02 Mutable object와 Immutable object
* Mutable : `list`, `dict`, etc.

  Immutable : `tuple`, `str`, etc.

* 아래 예시는 서로 비슷해보이지만 다르다.

        >>> r = [1, 2]
        >>> r += [3, 4]
        >>> r
        [1, 2, 3, 4]

        >>> s = (1, 2)
        >>> s += (3, 4)
        >>> s
        (1, 2, 3, 4)

  다음 함수를 이용하여 확인해볼 수 있다.

      id(obj, /)
        Return the identity of an object.
        This is guaranteed to be unique among simultaneously existing objects.
        (CPython uses the object's memory address.)

## 03 Deep copy와 Shallow copy
* `is`와 `==`의 차이

      >>> r1 = ['str', ('tu', 'ple'), [1, 2]]
      >>> r2 = list(r1)
      >>> r1 is r2
      False
      >> r1[0] is r2[0]
      True
      >> r1[1] is r2[1]
      True
      >> r1[2] is r2[2]
      True

* Immutable object는 shallow copy를 해도 되지만, mutable object는 문제가 될 수 있다.

* Deep copy

      import copy
      x = copy.copy(y)      # make a shallow copy of y
      x = copy.deepcopy(y)  # make a deep copy of y
    
  `deepcopy`를 쓰면 `y` 안의 immutable object는 shallow copy되고 mutable object는 deep copy된다.

## 04 List comprehension
* example

      >>> r1 = [1, 2, 3, 4, 5]
      >>> r2 = [x*2 for x in r1]
      >>> r2
      [2, 4, 6, 8, 10]
 
* conditional statement


      >>> r1 = [1, 2, 3, 4, 5]
      >>> r2 = [x*2 for x in r1 if x%2]
      >>> r2
      [2, 6, 10]
  
* nested loop

      >>> r1 = ['s', 't']
      >>> r2 = ['1', '2', '3']
      >>> r3 = [i+j for i in r1 for j in r2]
      >>> r3
      ['s1', 's2', 's3', 't1', 't2', 't3']

* nested loop + conditional statement

      >>> r = [n*m for n in range(2, 6) for m in range(1, 6) if (n*m)%2]
      >>> r
      [3, 9, 15, 5, 15, 25]

## 05 Iterable object와 Iterator object
* Iterable object : `list`, `tuple`, `str`, etc.

* `iter()`는 input으로 받은 iterable에 접근할 수 있는 iterator를 반환한다.

      >>> l1 = [1, 2]
      >>> ir = iter(l1)
      >>> next(ir)
      1
      >>> next(ir)
      2
      >>> next(ir)
      Traceback (most recent call last):
        File "<pyshell#4>", line 1, in <module>
          next(ir)
      StopIteration

* special method : Python interpreter에 의해 호출되는 method

  ex) `__init__`, `__iter__`, `__next__`, etc.

      >>> l1 = [1, 2]
      >>> ir = l1.__iter__()
      >>> ir.__next__()
      1
      >>> ir.__next__()
      2
 
* `dir([1, 2])`의 return element 중에 `__iter__` method가 있으므로 [1, 2]는 iterable이다.

  또는, `hasattr([1, 2], '__iter__')`의 return이 `True`이므로 [1, 2]는 iterable이다.

* 다음 코드는,

      for i in [1, 2, 3]:
          do something
   
  내부적으로 다음과 같은 형태로 동작한다.
  
      ir = iter([1, 2, 3])
      while True:
          try:
              i = next(ir)
              do something
          except StopIteration:
              break

* for loop에 iterable이 아닌 iterator를 둬도 정상적으로 동작한다. 왜냐하면 `iter`에 iterator를 전달하면 그 iterator를 그대로 return하기 때문이다.

      >>> ir1 = iter([1, 2])
      >>> ir2 = iter(ir1)
      >>> ir1 is ir2      # ir1과 ir2는 같은 object를 reference한다.
      True
      >>> id(ir1) == id(ir2)
      True
 
  즉, iterable이 와야 하는 위치에는 iterator가 올 수 있다.
  
## 06 object처럼 다뤄지는 함수 그리고 Lambda

* 매개변수에 함수를 전달할 수 있다.
      
      >>> def say():
              print('hello')
      >>> def caller(fun):
              fun()
      >>> caller(say)
      hello

* 함수 내에서 함수를 만들어서 이를 반환할 수 있다.
      
      >>> def fac(n):
              def exp(x):
                  return x ** n
              return exp
      >>> f2 = fac(2)
      >>> f2(3)
      9

* 이름없는 함수 lambda

      lambda args: expression
  
  `args`에는 매개변수 선언을, `expression`에는 함수의 body를 둔다. 예를 들어,
  
      >>> f1 = lambda n1, n2: n1+n2
      >>> f1(3, 4)
      7
      
      >>> f2 = lambda: print('yes')
      >>> f2()
      yes
      
      >>> def fac(n):
              return lambda x: x**n
      >>> f2 = fac(2)
      >>> f2(3)
      9


## 07 map & filter
* `map`

      class map(object)
      | map(func, *iterables) --> map object
      |
      | Make an iterator that computes the function using arguments from each of the iterables.
      | Stops when the shortest iterable is exhausted.
     
  map은 generator object(iterator object)를 하나 반환한다.
   
      >>> def sum(n1, n2):
      >>>     return n1+n2
      >>> st1 = [1, 2, 3]
      >>> st2 = [1, 3, 5]
      >>> st3 = list(map(sum, st1, st2))
      >>> st3
      [2, 5, 8]

* slicing

      >>> st = [1, 2, 3, 4, 5, 6, 7, 8]
      >>> st[:]
      [1, 2, 3, 4, 5, 6, 7, 8]
      >>> st[::2]
      [1, 3, 5, 7]
      >>> st[::-1]
      [8, 7, 6, 5, 4, 3, 2, 1]
      >>> s = 'hello'
      >>> s[::-1]
      'olleh'
      
* lambda + map

      >>> st = ['one', 'two', 'three']
      >>> rst = list(map(lambda s: s[::-1], st))
      >>> rst
      ['eno', 'owt', 'eerht']
 
* `filter`

      class filter(object)
      | filter(function or None, iterable) --> filter object
      |
      | Return an iterator yielding those items of iterable for which function(item) is true.
      | If function is None, return the items that are true.
 
  filter도 generator object를 반환한다.
  
      >>> def is_odd(n):
              return n%2
      >>> st = [1, 2, 3, 4, 5]
      >>> ost = list(filter(is_odd, st))
      >>> ost
      [1, 3, 5]
 
* map + filter

      >>> st = list(range(1, 11))
      >>> fst = list(filter(lambda n: not(n%3), map(lambda n: n**2, st)))
      >>> fst
      [9, 36, 81]

## 08 map과 filter를 대신하는 list comprehension
* map 또는 filter를 사용해야 하는 경우, 특히 map과 filter를 동시에 사용해야 하는 경우에는 list comprehension으로 대체하는 것에 대해 생각해 볼 필요가 있다. 예를 들어,

      >>> st = list(range(1, 11))
      >>> fst = list(filter(lambda n: n**2, map(lambda n: n%2, st)))
      >>> fst
      [1, 9, 25, 49, 81]
  
  는
  
      >>> st = list(range(1, 11))
      >>> fst = [n**2 for n in st if n%2]

  로 간단히 할 수 있다.

## 09 Generator function

* Generator: iterator object의 한 종류. Generator를 전달하면서 next 함수를 호출하면 값을 하나씩 얻을 수 있다.
* Generator를 만드는 두 가지 방법
  + generator function
  + generator expression
  
* generator function으로 generator 만들기: `yield`가 하나라도 들어가면 generator가 된다.

      >>> def gen_num():
              print('first number')
              yield 1
              print('second number')
              yield 2
              print('third number')
              yield 3
      >>> gen = gen_num()   # gen_num()이 실행되는 것이 아니라 generator object가 만들어져 return된다.
      >>> type(gen)
      <class 'generator'>
      >>> next(gen)         # gen_num()의 첫 번째 문장부터 시작해서 첫 번째 yield문을 만날 때까지 실행
      first number
      1                     # yield가 return의 역할
      >>> next(gen)
      second number
      2
      >>> next(gen)
      third number
      3
      
  여기서 다시 next 함수를 호출하면 StopIteration exception이 발생한다.

* lazy evalution: 함수 호출 이후에 그 실행의 흐름을 next 함수가 호출될 때까지 미루는(늦추는) 특성

* 사용한 메모리 공간의 크기 확인

      >>> import sys
      >>> help (sys.getsizeof)
      getsizeof(...)
          getsizeof(object[, default]) -> int
          Return the size of object in bytes.
  
 
* generator의 장점
  
      >>> def gpows(s):
              for i in s:
                  yield i**2
      >>> st = gpows([1, 2, 3, 4, 5, 6, 7, 8, 9])
      >>> for i in st:
              print(i, end = ' ')
      1 4 9 16 25 36 49 64 81
      >>> import sys
      >>> sys.getsizeof(st)
      64
      
  
  위의 예시에서, generator를 사용했기 때문에 list의 길이에 상관없이 사용하는 메모리 공간의 크기가 동일하다. Generator object는 return할 값들을 미리 만들어서 저장해 두지 않기 때문이다. 만약 generator를 쓰지 않고
  
      >>> def pows(s):
              r = []
              for i in s:
                  r.append(i**2)
              return r
 
  으로 한다면 사용하는 메모리 공간의 크기는 list의 길이에 비례해서 늘어난다.

* `yield from`

      >>> def get_nums():
              ns = [0, 1, 0, 1, 0, 1]
              for i in ns:
                  yield i
   
   는
   
      >>> def get_nums():
              ns = [0, 1, 0, 1, 0, 1]
              yield from ns
    
    와 같다.

## 10 Generator Expression
* iterable object를 통해서 값을 출력하는 함수

      def show_all(s):
          for i in s:
              print(i, end = ' ')
      
* list comprehension을 이용한 예

      >>> st = [2*i for i in range(1, 10)]
      >>> show_all(st)
      2 4 6 8 10 12 14 16 18
      
* generator function을 이용한 예

      >>> def times2():
              for i in range(1, 10)
                  yield 2*i
      >>> g = times2()      # generator object 생성
      >>> show_all(g)
      2 4 6 8 10 12 14 16 18
  
  list comprehension에 비해 메모리는 효율적으로 사용할 수 있지만, 함수를 별도로 정의해야 한다는 단점이 있다.
  
* generator expression을 이용한 예

      >>> g = (2*i for i in range(1, 10))
      >>> show_all(g)
      2 4 6 8 10 12 14 16 18
 
  메모리를 효율적으로 사용할 수 있으며 간결하다. 특히,
  
      >>> show_all(2*i for i in range(1, 10))
      2 4 6 8 10 12 14 16 18
   
   으로 쓸 수도 있다.

* list comprehension vs. generator expression

      >>> def two():
              print('two')
              return 2
      
  라는 함수를 만들어서 보면, list comprehension의 경우

      >>> f = [two()*i for i in range(1, 5)]
      two
      two
      two
      two
      >>> next(f)
      Traceback (most recent call last):
        File "<pyshell#14>", line 1, in <module>
          next(f)
      TypeError: 'list' object is not an iterator
      >>> f
      [2, 4, 6, 8]
      >>> show_all(f)
      2 4 6 8
  
  인 반면, generator expression의 경우
  
      >>> g = (two()*i for i in range(1, 5))
      >>> next(g)
      two
      2
      >>> next(g)
      two
      4
      >>> g
      <generator object <genexpr> at 0x000002C9D3660740>
      >>> show_all(g)
      two
      6 two
      8

  으로, next 함수 호출 시마다 'two'가 하나씩 출력되었다는 것은 next 함수가 호출되는 순간에 던져줄 값이 만들어진 증거이다.
      
## 11 Tuple packing & Tuple unpacking
* packing & unpacking

  Tuple packing은 소괄호가 없어도 된다.
  
      >>> tup = 23, 12
      >>> tup
      (23, 12)
  
  `*`: Tuple unpacking 과정에서 둘 이상의 값을 list로 묶어서 하나의 변수에 저장할 수 있다.
  
      >>> nums = (1, 2, 3, 4, 5)    # tuple이 아니라 list여도 동일하게 동작한다.
      >>> n1, n2, *others = nums
      >>> n1
      1
      >>> n2
      2
      >>> others
      [3, 4, 5]                     # tuple이 아니라 list다.
  
* function call & return

      >>> def ret_nums():
              return 1, 2, 3, 4, 5
      >>> nums = ret_nums()
      >>> nums
      (1, 2, 3, 4, 5)
      >>> n, *others = ret_nums()
      >>> n
      1
      >>> others
      [2, 3, 4, 5]      # list다.
      
  parameter 앞에 `*`가 오면 '나머지 값들은 tuple로 묶어서 이 변수에 저장하겠다'는 의미이다.
  
      >>> def show_nums(*other, n1, n2):
              print(other, n1, n2, sep=', ')
      >>> show_nums(1, 2, 3, 4)
      (1, 2), 3, 4                  # tuple이다.
      >>> show_nums(1, 2, 3, 4, 5)
      (1, 2, 3), 4, 5

  function call에서 `*`가 사용되면 tuple unpacking으로 이어진다.
  
      >>> def show_man(name, age, height):
              print(name, age, height, sep=', ')
      >>> p = ('Yoon', 22, 180)       # tuple이 아니라 list여도 동일하게 동작한다.
      >>> show_man(*p)
      Yoon, 22, 180
      
* tuple 안에 tuple이 있을 때는 tuple 안의 값의 구조와 동일한 형태로 변수를 선언한 후 unpacking

      >>> t = (1, 2, (3, 4))
      >>> a, b, (c, d) = t
      >>> print(a, b, c, d, sep=', ')
      1, 2, 3, 4
      
* for-loop에서의 unpacking

      >>> ps = [('Lee', M, 172, 26), ('Jung', F, 182, 55), ('Yoon', M, 179, 32)]  # list 안의 tuple
      >>> for n, _, h, _ in ps:
              print(n, h, sep=', ')
      
      Lee, 172
      Jung, 182
      Yoon, 179
  
  list 안의 list, tuple 안의 tuple, tuple 안의 list에 대해서도 동일하게 작동한다.
  
## 12 Named Tuple
* Named Tuple
  
      >>> from collections import namedtuple
      >>> Tri = namedtuple('Triangle', ['bottom', 'height'])          # Triangle이라는 이름의 class 생성
      >>> t = Tri(3, 7)
      >>> print(t[0])
      3
      >>> print(t.bottom, t.height)
      3 7
      >>> t[0] = 15
      Trackback(most recent call last):
        File "<pyshell#5>", line 1, in <module>
          t[0] = 15
      TypeError: 'Triangle' object does not support item assignment
      
  위의 코드에서,

      Tri = namedtuple('Tri', ['bottom', 'height'])
      
  또는

      Tri = namedtuple('Tri', 'bottom, height')
      
  로 바꿔도 된다.
