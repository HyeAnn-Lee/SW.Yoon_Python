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
     
  map은 iterator object를 하나 반환한다.
   
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
 
  filter도 iterator object를 반환한다.
  
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
