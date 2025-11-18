# Python 泛型

Python 的泛型（Generics）机制虽然不像 Java 或 C# 那样在运行时强制类型约束，但自 Python 3.5 引入 **类型注解（Type Hints）** 和 `typing` 模块后，Python 也支持了强大的泛型编程能力，主要用于 **静态类型检查**（如通过 `mypy`）和提升代码可读性与维护性。

---

## 📘 一、什么是泛型？

泛型是一种编程语言特性，允许你编写**与具体类型无关的代码**，在使用时再指定类型。

例如：定义一个列表，它可以是 `int` 列表、`str` 列表，也可以是任意类型的列表。

```python
def first_item(items: list[T]) -> T: ...

```

这里的 `T` 就是一个**类型变量**，代表“某种类型”，具体类型在调用时确定。

---

## 🧩 二、Python 泛型的核心模块：`typing`

Python 从 3.5 开始通过 `typing` 模块支持泛型。主要工具包括：

- `TypeVar`
- `Generic`
- 常见泛型容器：`List`, `Dict`, `Optional`, `Union`, `Callable` 等
- 自 Python 3.9+，内置容器也支持泛型（如 `list[int]`）

---

## 🔤 三、`TypeVar`：定义类型变量

`TypeVar` 是创建泛型的基础。

```python
from typing import TypeVar

T = TypeVar('T')        # 可以是任何类型
S = TypeVar('S', str, int)  # 约束：只能是 str 或 int

```

### 示例：泛型函数

```python
from typing import TypeVar

T = TypeVar('T')

def identity(x: T) -> T:
    return x

# 使用
a = identity(10)        # 类型推断为 int
b = identity("hello")   # 类型推断为 str

```

静态类型检查器（如 mypy）会知道返回值类型与输入一致。

---

## 🧱 四、`Generic`：定义泛型类

使用 `Generic[T]` 可以定义泛型类。

### 示例：一个简单的泛型栈

```python
from typing import Generic, TypeVar, List

T = TypeVar('T')

class Stack(Generic[T]):
    def __init__(self) -> None:
        self._items: List[T] = []

    def push(self, item: T) -> None:
        self._items.append(item)

    def pop(self) -> T:
        return self._items.pop()

    def is_empty(self) -> bool:
        return len(self._items) == 0

# 使用
s1 = Stack[int]()
s1.push(1)
s1.push(2)
x: int = s1.pop()  # 类型安全

s2 = Stack[str]()
s2.push("hello")
s2.push("world")
y: str = s2.pop()

```

> ✅ 类型检查器会确保你不会往 Stack[int] 中 push 字符串。

---

## 📦 五、常用泛型类型（`typing` 模块）

|类型|说明|示例|
|---|---|---|
|`List[T]`|泛型列表|`List[int]`, `List[str]`|
|`Dict[K, V]`|键值对字典|`Dict[str, int]`|
|`Tuple[T1, T2]`|固定长度元组|`Tuple[int, str]`|
|`Set[T]`|集合|`Set[float]`|
|`Optional[T]`|等价于 `Union[T, None]`|`Optional[str]` 表示可能是 `str` 或 `None`|
|`Union[T1, T2]`|联合类型|`Union[int, str]`|
|`Callable[[Arg1, Arg2], Return]`|函数类型|`Callable[[int], str]`|
|`Iterable[T]`|可迭代对象|`for x in xs: ...`|
|`Iterator[T]`|迭代器|`iter(xs)` 返回类型|

---

## 🐍 六、Python 3.9+：内置容器支持泛型

从 Python 3.9 开始，你可以直接使用内置类型进行泛型标注，不再需要 `typing.List` 等。

```python
# Python 3.9+
def process(items: list[str]) -> dict[str, int]:
    return {item: len(item) for item in items}

# 之前版本必须写：
# from typing import List, Dict
# def process(items: List[str]) -> Dict[str, int]: ...

```

> ✅ 推荐新项目使用 list[int] 等形式。

---

## 🔁 七、泛型函数（不显式继承 `Generic`）

大多数泛型函数不需要继承 `Generic`，只需使用 `TypeVar`。

```python
from typing import TypeVar, Sequence

T = TypeVar('T')

def first(seq: Sequence[T]) -> T:
    if seq:
        return seq[0]
    raise IndexError("seq is empty")

# 使用
first([1, 2, 3])        # 返回 int
first(['a', 'b'])       # 返回 str

```

`Sequence[T]` 是 `list`, `tuple`, `str` 等的抽象基类。

---

## 🔄 八、协变（Covariant）与逆变（Contravariant）

Python 支持通过 `TypeVar` 指定方差：

```python
from typing import TypeVar

# 协变：Dog 是 Animal 的子类，则 List[Dog] 可当作 List[Animal]？
T_co = TypeVar('T_co', covariant=True)      # 协变
T_contra = TypeVar('T_contra', contravariant=True)  # 逆变

```

### 应用场景：只读容器（协变）

```python
from typing import Generic, TypeVar

T = TypeVar('T', covariant=True)

class ImmutableList(Generic[T]):
    def __init__(self, items: list[T]) -> None:
        self._items = items.copy()

    def get(self, index: int) -> T:
        return self._items[index]

    def __len__(self) -> int:
        return len(self._items)

```

协变常用于不可变数据结构。

---

## 🛠️ 九、静态类型检查工具

Python 的泛型主要用于**静态分析**，需要工具支持：

- [`mypy`](https://mypy-lang.org/)：最流行的 Python 类型检查器
- `pyright` / `pylance`（VS Code）
- `pyre`（Facebook 开发）

示例检查：

```bash
pip install mypy
mypy your_script.py

```

---

## ⚠️ 十、注意事项

1. **运行时无类型约束**
    
    Python 的泛型在运行时**不做强制检查**，类型错误不会在运行时报错（除非你手动检查）。

    ```python
    s: list[int] = []
    s.append("hello")  # 运行时不报错！但 mypy 会警告
    
    ```

2. **类型擦除**
    
    类型信息在运行时被忽略（类似 Java），不能通过 `type()` 获取泛型参数。
    
3. **建议配合 IDE 使用**
    
    VS Code、PyCharm 等能根据类型提示提供自动补全和错误提示。

---

## ✅ 十一、最佳实践

|建议|说明|
|---|---|
|使用 `TypeVar` 定义泛型函数/类|提高代码复用性和类型安全|
|新项目使用 `list[int]` 而非 `List[int]`|更简洁，Python 3.9+ 推荐|
|对可为空的值使用 `Optional[T]`|明确表达 `None` 是合法值|
|使用 `Generic[T]` 构建可复用组件|如容器、工具类|
|配合 `mypy` 使用|发挥泛型最大价值|

---

## 🎯 总结

|特性|Python 泛型支持情况|
|---|---|
|类型变量|✅ `TypeVar`|
|泛型类|✅ `class MyClass(Generic[T])`|
|泛型函数|✅ 使用 `TypeVar`|
|泛型方法|✅ 支持|
|运行时类型检查|❌ 不支持（需手动）|
|静态类型检查|✅ `mypy` 等工具支持|
|内置容器泛型（3.9+）|✅ `list[int]`|

---

## 📚 参考文档

- [PEP 484 – Type Hints](https://peps.python.org/pep-0484/)
- [mypy 官方文档](https://mypy.readthedocs.io/)
- [typing 模块官方文档](https://docs.python.org/3/library/typing.html)
