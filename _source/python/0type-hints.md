# python类型注释

## 简单类型注释
```python
def str_to_str(str_: str = "abc") -> str:
    return str_

str_to_str("abc"), str_to_str(1)
```

>变量类型不满足要求时，有IDE提示，但不强制执行。

```python
def void_function() -> None:
    print("This function returns nothing.")
```

## 复杂类型注释
```python
from typing import List, Set, Dict, Tuple

l: List[List[int]] = [[1, 2, 3], [4, 5, 6]]
d: Dict[str, float] = {"a" : 1.2, "b" : 2.3}
t: Tuple[int, str] = (1, "abc")
```


## 类型别名
```python
floatVector = List[float]

fv: floatVector = [1.1, 2.2]
```

## 泛型

```python
from typing import TypeVar

T = TypeVar("T")

def xToX(x: T) -> T:
    return x

xToX(1), xToX('abc')
```

## 多种可能类型
```python
from typing import Union

a: Union[int, str] = 'a'
```



[详见](https://www.pythonsheets.com/notes/python-typing.html)