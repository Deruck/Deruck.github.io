# python异常处理

## 抛出异常
- 抛出一个空异常
```python
raise
```
```
RuntimeError: No active exception to reraise
```

- 抛出普通异常
```python
raise Exception
```
```
Exception: 
```

- 抛出含内容的普通异常
```python
raise Exception("error information")
```
```
Exception: error information
```
- 抛出特定异常
```python
raise ValueError("value error information")
```
```
ValueError: value error information
```
- python内置异常类型

| 异常名称            | 描述                                           |
| ------------------- | ---------------------------------------------- |
| `AssertionError`    | 当assert断言条件为假的时候抛出的异常           |
| `AttributeError`    | 当访问的对象属性不存在的时候抛出的异常         |
| `IndexError`        | 超出对象索引的范围时抛出的异常                 |
| `KeyError`          | 在字典中查找一个不存在的key抛出的异常          |
| `NameError`         | 访问一个不存在的变量时抛出的异常               |
| `OSError`           | 操作系统产生的异常                             |
| `SyntaxError`       | 语法错误时会抛出此异常                         |
| `TypeError`         | 类型错误，通常是不通类型之间的操作会出现此异常 |
| `ZeroDivisionError` | 进行数学运算时除数为0时会出现此异常            |
| `IOError`           | 输入/输出异常；基本上是无法打开文件            |
| `KeyError`          | 试图访问字典里不存在的键                       |
| `ValueError`        | 传入一个调用者不期望的值，即使值的类型是正确的 |

- 抛出自定义异常
```python
class MyError(Exception):
    def __init__(self, value):
        self.value = value
    def __str__(self):
        return repr(self.value)
    
raise MyError('my error info')
```
```
MyError: 'my error info'
```

## 捕获异常
- `try...except...`
```python
try:
    raise Exception
except Exception:
    print("An exception occurs!")
```
```
An exception occurs!
```
- `else`
```python
try:
    pass
except Exception:
    print("An exception occurs!")
else:
    print("No exception occurs!")
```
```
No exception occurs!
```

- `finally`
```python
try:
    raise Exception
except Exception:
    print("An exception occurs!")
finally:
    print("These statements will always be executed.")
```
```
An exception occurs!
These statements will always be executed.
```