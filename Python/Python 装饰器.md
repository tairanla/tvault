# Python 装饰器

装饰器（decorators）是 Python 中的一种高级功能，它允许你动态地修改函数或类的行为。

装饰器是一种函数，它接受一个函数作为参数，并返回一个新的函数或修改原来的函数。

![](https://www.runoob.com/wp-content/uploads/2024/03/decorators-python-1.png)

装饰器的语法使用 **@decorator_name** 来应用在函数或方法上。

Python 还提供了一些内置的装饰器，比如 **@staticmethod** 和 **@classmethod**，用于定义静态方法和类方法。

**装饰器的应用场景：**

- **日志记录**: 装饰器可用于记录函数的调用信息、参数和返回值。
- **性能分析**: 可以使用装饰器来测量函数的执行时间。
- **权限控制**: 装饰器可用于限制对某些函数的访问权限。
- **缓存**: 装饰器可用于实现函数结果的缓存，以提高性能。

## **基本语法**

Python 装饰允许在不修改原有函数代码的基础上，动态地增加或修改函数的功能，装饰器本质上是一个接收函数作为输入并返回一个新的包装过后的函数的对象。

```python
def decorator_function(original_function):
    def wrapper(*args, **kwargs):
        # 这里是在调用原始函数前添加的新功能
        before_call_code()
        
        result = original_function(*args, **kwargs)
        
        # 这里是在调用原始函数后添加的新功能
        after_call_code()
        
        return result
    return wrapper

# 使用装饰器
@decorator_function
def target_function(arg1, arg2):
    pass  # 原始函数的实现
```

**解析：**decorator 是一个装饰器函数，它接受一个函数 func 作为参数，并返回一个内部函数 wrapper，在 wrapper 函数内部，你可以执行一些额外的操作，然后调用原始函数 func，并返回其结果。

- `decorator_function` 是装饰器，它接收一个函数 `original_function` 作为参数。
- `wrapper` 是内部函数，它是实际会被调用的新函数，它包裹了原始函数的调用，并在其前后增加了额外的行为。
- 当我们使用 `@decorator_function` 前缀在 `target_function` 定义前，Python会自动将 `target_function` 作为参数传递给 `decorator_function`，然后将返回的 `wrapper` 函数替换掉原来的 `target_function`。

## **使用装饰器**

装饰器通过 **@** 符号应用在函数定义之前，例如：

```python
@time_logger
def target_function():
    pass
```

等同于：

```python
def target_function():
    pass
target_function = time_logger(target_function)
```

这会将 target_function 函数传递给 decorator 装饰器，并将返回的函数重新赋值给 target_function。从而，每次调用 target_function 时，实际上是调用了经过装饰器处理后的函数。

通过装饰器，开发者可以在保持代码整洁的同时，灵活且高效地扩展程序的功能。

以下是一个简单的装饰器示例，它会在函数执行前后打印日志：

```python
def my_decorator(func):
    def wrapper():
        print("在原函数之前执行")
        func()
        print("在原函数之后执行")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

输出：

```txt
在原函数之前执行
Hello!
在原函数之后执行
```

- my_decorator 是一个装饰器函数，它接受 say_hello 作为参数，并返回 wrapper 函数。
- @my_decorator 将 say_hello 替换为 wrapper。

## **带参数的装饰器**

如果原函数需要参数，可以在装饰器的 wrapper 函数中传递参数：

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("在原函数之前执行")
        func(*args, **kwargs)
        print("在原函数之后执行")
    return wrapper

@my_decorator
def greet(name):
    print(f"Hello, {name}!")

greet("Alice")
```

以上代码代码定义了一个装饰器 my_decorator，它会在被装饰的函数执行前后分别打印一条消息。装饰器通过 wrapper 函数包裹原函数，并在调用原函数前后添加额外操作。

输出：

```txt
在原函数之前执行
Hello, Alice!
在原函数之后执行
```

装饰器本身也可以接受参数，此时需要额外定义一个外层函数：

```python
def repeat(num_times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(num_times):
                func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(3)
def say_hello():
    print("Hello!")

say_hello()
```

repeat 函数是一个装饰器工厂，它接受一个参数 num_times，返回一个装饰器 decorator。decorator 接受一个函数 func，并返回一个 wrapper 函数。wrapper 函数会调用 func 函数 num_times 次。使用 @repeat(3) 装饰s ay_hell 函数后，调用 say_hello 会打印 "Hello!" 三次。

![](https://www.runoob.com/wp-content/uploads/2024/03/decorators-python-2.png)

```txt
Hello!
Hello!
Hello!
```

## 类装饰器

除了函数装饰器，Python 还支持类装饰器。

类装饰器（Class Decorator）是一种用于动态修改类行为的装饰器，它接收一个类作为参数，并返回一个新的类或修改后的类。

类装饰器可以用于：

- 添加/修改类的方法或属性
- 拦截实例化过程
- 实现单例模式、日志记录、权限检查等功能

**类装饰器有两种常见形式：**

- 函数形式的类装饰器（接收类作为参数，返回新类）
- 类形式的类装饰器（实现 ****call**** 方法，使其可调用）

### **函数形式的类装饰器**

以下实例给类添加日志功能：

```python
def log_class(cls):
    """类装饰器，在调用方法前后打印日志"""
    class Wrapper:
        def __init__(self, *args, **kwargs):
            self.wrapped = cls(*args, **kwargs)  # 实例化原始类
        
        def __getattr__(self, name):
            """拦截未定义的属性访问，转发给原始类"""
            return getattr(self.wrapped, name)
        
        def display(self):
            print(f"调用 {cls.__name__}.display() 前")
            self.wrapped.display()
            print(f"调用 {cls.__name__}.display() 后")
    
    return Wrapper  # 返回包装后的类

@log_class
class MyClass:
    def display(self):
        print("这是 MyClass 的 display 方法")

obj = MyClass()
obj.display()
```

输出：

```txt
调用 MyClass.display() 前
这是 MyClass 的 display 方法
调用 MyClass.display() 后
```

### **类形式的类装饰器（实现 **call** 方法）**

以下实例实现单例模式（Singleton）：

```python
class SingletonDecorator:
    """类装饰器，使目标类变成单例模式"""
    def __init__(self, cls):
        self.cls = cls
        self.instance = None
    
    def __call__(self, *args, **kwargs):
        """拦截实例化过程，确保只创建一个实例"""
        if self.instance is None:
            self.instance = self.cls(*args, **kwargs)
        return self.instance

@SingletonDecorator
class Database:
    def __init__(self):
        print("Database 初始化")

db1 = Database()
db2 = Database()
print(db1 is db2)  # True，说明是同一个实例
```

输出：

```txt
Database 初始化
True
```

## 内置装饰器

Python 提供了一些内置的装饰器，例如：

1. **`@staticmethod`**: 将方法定义为静态方法，不需要实例化类即可调用。
2. **`@classmethod`**: 将方法定义为类方法，第一个参数是类本身（通常命名为 `cls`）。
3. **`@property`**: 将方法转换为属性，使其可以像属性一样访问。

```python
class MyClass:
    @staticmethod
    def static_method():
        print("This is a static method.")

    @classmethod
    def class_method(cls):
        print(f"This is a class method of {cls.__name__}.")

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        self._name = value

# 使用
MyClass.static_method()
MyClass.class_method()

obj = MyClass()
obj.name = "Alice"
print(obj.name)
```

## 多个装饰器的堆叠

你可以将多个装饰器堆叠在一起，它们会按照从下到上的顺序依次应用。例如：

```python
def decorator1(func):
    def wrapper():
        print("Decorator 1")
        func()
    return wrapper

def decorator2(func):
    def wrapper():
        print("Decorator 2")
        func()
    return wrapper

@decorator1
@decorator2
def say_hello():
    print("Hello!")

say_hello()
```

输出：

```txt
Decorator 1
Decorator 2
Hello!
```

## 类装饰器

### 1. @classmethod 类方法装饰器

#### 基本概念

- 类方法的第一个参数是类本身（通常命名为`cls`）
- 可以通过类名或实例调用
- 常用于创建替代构造器

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @classmethod
    def from_string(cls, person_str):
        """替代构造器：从字符串创建对象"""
        name, age = person_str.split('-')
        return cls(name, int(age))  # cls等同于Person

    @classmethod
    def get_species(cls):
        """获取类属性"""
        return "Homo sapiens"

# 使用示例
person1 = Person("Alice", 25)
person2 = Person.from_string("Bob-30")  # 使用类方法创建对象

print(Person.get_species())  # Homo sapiens
print(person1.get_species())  # 也可以通过实例调用

```

### 2. @staticmethod 静态方法装饰器

#### 基本概念

- 静态方法不接收类或实例作为第一个参数
- 与普通函数类似，但逻辑上属于类
- 不能访问类或实例的属性

```python
class MathUtils:
    @staticmethod
    def add(x, y):
        """静态方法：执行加法运算"""
        return x + y

    @staticmethod
    def is_even(number):
        """静态方法：判断是否为偶数"""
        return number % 2 == 0

# 使用示例
result = MathUtils.add(5, 3)  # 8
print(MathUtils.is_even(4))   # True

# 也可以通过实例调用
utils = MathUtils()
print(utils.add(2, 7))  # 9

```

### 3. @property 属性装饰器

#### 基本概念

- 将方法转换为只读属性
- 提供对属性访问的控制

```python
class Circle:
    def __init__(self, radius):
        self._radius = radius

    @property
    def radius(self):
        """获取半径"""
        return self._radius

    @radius.setter
    def radius(self, value):
        """设置半径"""
        if value < 0:
            raise ValueError("半径不能为负数")
        self._radius = value

    @property
    def area(self):
        """计算面积（只读属性）"""
        return 3.14159 * self._radius ** 2

# 使用示例
circle = Circle(5)
print(circle.radius)  # 5
print(circle.area)    # 78.53975

circle.radius = 10    # 通过setter设置
print(circle.area)    # 314.159

```

### 4. 抽象类相关装饰器

#### 使用abc模块

```python
from abc import ABC, abstractmethod

class Animal(ABC):
    """抽象基类"""

    def __init__(self, name):
        self.name = name

    @abstractmethod
    def make_sound(self):
        """抽象方法：子类必须实现"""
        pass

    @abstractmethod
    def move(self):
        """抽象方法：子类必须实现"""
        pass

    def sleep(self):
        """具体方法：子类可以继承使用"""
        return f"{self.name} is sleeping"

class Dog(Animal):
    """具体实现类"""

    def make_sound(self):
        return "Woof!"

    def move(self):
        return "Running"

class Bird(Animal):
    """另一个具体实现类"""

    def make_sound(self):
        return "Tweet!"

    def move(self):
        return "Flying"

# 使用示例
# animal = Animal("Generic")  # 会报错：不能实例化抽象类

dog = Dog("Buddy")
print(dog.make_sound())  # Woof!
print(dog.move())        # Running
print(dog.sleep())       # Buddy is sleeping

```

### 5. @dataclass 数据类装饰器

#### 简化类的定义

```python
from dataclasses import dataclass, field
from typing import List

@dataclass
class Student:
    name: str
    age: int
    grades: List[float] = field(default_factory=list)

    def average_grade(self):
        return sum(self.grades) / len(self.grades) if self.grades else 0

# 自动生成了__init__, __repr__, __eq__等方法
student = Student("Alice", 20, [85, 90, 78])
print(student)  # Student(name='Alice', age=20, grades=[85, 90, 78])
print(student.average_grade())  # 84.33333333333333

```

### 6. 各种方法类型的对比

```python
class Example:
    class_var = "I'm a class variable"

    def __init__(self, value):
        self.instance_var = value

    def instance_method(self):
        """实例方法：访问实例和类变量"""
        return f"Instance: {self.instance_var}, Class: {self.class_var}"

    @classmethod
    def class_method(cls):
        """类方法：只能直接访问类变量"""
        return f"Class method: {cls.class_var}"

    @staticmethod
    def static_method():
        """静态方法：不能直接访问类或实例变量"""
        return "Static method: No access to class or instance variables"

# 调用方式对比
obj = Example("test")

# 实例方法
print(obj.instance_method())

# 类方法
print(Example.class_method())
print(obj.class_method())

# 静态方法
print(Example.static_method())
print(obj.static_method())

```

### 7. 实际应用示例

```python
from abc import ABC, abstractmethod
from datetime import datetime

class DatabaseConnection(ABC):
    """数据库连接抽象基类"""

    def __init__(self, host, port):
        self.host = host
        self.port = port
        self.connected = False

    @classmethod
    def from_config(cls, config_dict):
        """从配置字典创建连接"""
        return cls(config_dict['host'], config_dict['port'])

    @staticmethod
    def validate_host(host):
        """验证主机地址"""
        return isinstance(host, str) and len(host) > 0

    @property
    def connection_info(self):
        """连接信息属性"""
        status = "Connected" if self.connected else "Disconnected"
        return f"{self.host}:{self.port} - {status}"

    @abstractmethod
    def connect(self):
        """抽象方法：连接数据库"""
        pass

    @abstractmethod
    def disconnect(self):
        """抽象方法：断开连接"""
        pass

class MySQLConnection(DatabaseConnection):
    """MySQL连接实现"""

    def connect(self):
        print(f"Connecting to MySQL at {self.host}:{self.port}")
        self.connected = True

    def disconnect(self):
        print(f"Disconnecting from MySQL at {self.host}:{self.port}")
        self.connected = False

# 使用示例
config = {'host': 'localhost', 'port': 3306}
mysql_conn = MySQLConnection.from_config(config)
print(mysql_conn.connection_info)
mysql_conn.connect()
print(mysql_conn.connection_info)

```

### 8. 总结

|装饰器|参数|访问权限|主要用途|
|---|---|---|---|
|`@classmethod`|`cls`|类属性|替代构造器、类操作|
|`@staticmethod`|无|无|工具函数、与类相关的独立函数|
|`@property`|无|实例属性|控制属性访问、计算属性|
|`@abstractmethod`|无|根据方法类型|定义接口规范|
|`@dataclass`|无|自动生成|简化数据类定义|
