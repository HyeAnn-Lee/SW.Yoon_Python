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
 
* `dir([1, 2])`의 return element 중에 `__iter__` method가 있으므로 `[1, 2]`는 iterable이다.

  또는, `hasattr([1, 2], '__iter__')`의 return이 `True`이므로 `[1, 2]`는 iterable이다.

* for loop에 iterable이 아닌 iterator를 둬도 정상적으로 동작한다. 왜냐하면 `iter`에 iterator를 전달하면 그 iterator를 그대로 return하기 때문이다.

      >>> ir1 = iter([1, 2])
      >>> ir2 = iter(ir1)
      >>> ir1 is ir2      # ir1과 ir2는 같은 object를 reference한다.
      True
      >>> id(ir1) == id(ir2)
      True
 
  즉, iterable이 와야 하는 위치에는 iterator가 올 수 있다.
