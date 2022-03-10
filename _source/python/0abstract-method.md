# python抽象方法

Python内置的继承机制，无法定义抽象方法，子类可能会忘记定义必要方法。

```python
class Animal:
    def __init__(self, name):
        self._name = name
        
    def eat(self):
        print("抽象食物")
        
class Dog(Animal):
    def __init__(self, name):
        super(Dog, self).__init__(name)
        
    def eat(self):
        print("狗吃肉")
        
class Cow(Animal):
    def __init__(self, name):
        super(Cow, self).__init__(name)


dog = Dog("旺财")
dog.eat()

cow = Cow("阿福")
cow.eat()
```
```
狗吃肉
抽象食物
```

`abc`模块可以帮助实现抽象基类。

>This module provides the infrastructure for defining  abstract base classes (ABCs) in Python

- 首先在父类中加入`metaclass=ABCMeta`
- 在父类的抽象方法上方添加`@abstractclassmethod`

```python
from abc import ABCMeta, abstractclassmethod

class Animal(metaclass=ABCMeta):
    def __init__(self, name):
        self._name = name
        
    @abstractclassmethod
    def eat(self):
        print("抽象食物")

        
class Cow(Animal):
    def __init__(self, name):
        super(Cow, self).__init__(name)

cow = Cow("阿福")
cow.eat()
```
```
TypeError: Can't instantiate abstract class Cow with abstract methods eat
```

实现抽象方法
```python
class Cow(Animal):
    def __init__(self, name):
        super(Cow, self).__init__(name)
        
    def eat(self):
        print("牛吃草")
        
cow = Cow("阿福")
cow.eat()
```
```
牛吃草
```
