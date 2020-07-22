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



## 04 List comprehension
