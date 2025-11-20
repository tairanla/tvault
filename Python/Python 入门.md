# Python

## 基础类型

- Number（数字）
- String（字符串）
- Boolean（布尔）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dict（字典）

| 不可变    | 可变   |
| ------ | ---- |
| Number | List |
| String | Set  |
| Tuple  | Dict |

### type 函数

通过 `type()` 查询变量类型：

```python
a = 1
print(type(a)) # <class 'int'>
```

### isinstance 函数

通过 isinstance() 判断变量是否属于某类型：

```python
a = 1
print(isinstance(a, int)) # True
```

如果某类型是另外一个类型的子类，那么变量会被认为是一种父类类型。

Python3 中 bool 是 int 的子类，True == 1；False == 0（所以 bool 可以与 int 进行基础运算，例如 True + 1）

```python
print(isinstance(True, int)) # True
```

## Number

int、float、bool、complex（复数）

int 除法，`/` 得到浮点数，`//` 得到整数：

```python
# 得到一个浮点数
2 / 4 # 0.5
# 得到一个整数
2 // 4 # 0
```

乘方运算，`**`：

```python
2 ** 4 # 16
```

复数实例：

```python
a + bj
# 或者
complex(a, b)
```

## String

截取字符串：

```python
str = "Hello"
print(str[1:3]) # 打印从第二个到第三个（不是第四个哦），el
print(str[0:-1]) # 打印从第一个到倒数第二个，Hell
print(str[2:]) # 打印第三个到最后，llo
```

拼接字符串，使用 `+`：

```python
str = "Hello" + " World"
print(str) # Hello World
```

Repeat 字符串，使用 `*`：

```python
str = "Hello" * 2
print(str) # HelloHellostr = "Hello" * 2
print(str) # HelloHello
```

转义，使用 `\\n` ，取消转义，使用 `r""` （原始字符串）：

```python
>>> print("Hello\\nWorld")
Hello
World
>>> print(r"Hello\\nWorld")
Hello\\nWorld
```

多行字符串，使用 `"""..."""` 或者 `'''...'''`：

```python
>>> print('''Hello
... World
... ''')
Hello
World
```

## Boolean

```python
admin = True
print(type(admin)) # <class 'bool'>
if admin:
	print("管理员")
else:
	print("普通用户")
```

## List

### 创建

```python
ls = ["a", "c", "b"]

ls = list(range(1, 5))
print(ls) # 得到结果 [1, 2, 3, 4]

# 指定步长
ls = list(range(2, 11, 2))
print(ls) # 得到结果 [2, 4, 6, 8, 10]
```

### 访问元素

```python
ls = ["a", "c", "b"]
a = ls[0]

# 最后一个元素
b = ls[-1]

# 遍历
for item in ls:
	print(item)

# 切片
print(ls[0:2]) # ["a", "c"]
print(ls[:2]) # ["a", "c"]
print(ls[1:]) # ["c", "b"]
print(ls[-2:] # 最后2个 ["c", "b"]
```

### 加入元素

```python
ls = ["a", "c", "b"]
# 往后追加
ls.append("d")

# 指定位置追加
ls.insert(1, "a1")
```

### 删除元素

```python
ls = ["a", "c", "b"]
# 指定位置删除
del ls[0]

# 弹出，同样是删除的效果，并返回删除的元素
deleted_item = ls.pop() # 得到结果 "b"

# 指定位置弹出
ls.pop(0)

# 根据值删除
ls.append("hello")
ls.remove("hello") # 只会删除第一个匹配的元素
```

### 复制

```python
ls = ["a", "c", "b"]
new_ls = ls[:] # 得到的是新的列表
```

### 排序

1. 排序并永久改变：

```python
ls = ["a", "c", "b"]
ls.sort()
print(ls) # 得到结果 ["a", "b", "c"]

ls.sort(reverse=True)
print(ls) # 得到结果 ["c", "b", "a"]
```

1. 排序但不改变原列表

```python
ls = ["a", "c", "b"]
new_ls = sorted(ls)
print(ls) # 得到结果 ["a", "c", "b"]
print(new_ls) # 得到结果 ["a", "b", "c"]

new_ls = sorted(ls, reverse=True)
print(ls) # 得到结果 ["a", "c", "b"]
print(new_ls) # 得到结果 ["c", "b", "a"]
```

### 反向

```python
ls = ["a", "c", "b"]
ls.reverse()
print(ls) # 得到结果 ["b", "c", "a"]
```

### 数值列表

```python
nums = [1, 2, 3]
min(nums) # 1
max(nums) # 3

import statistics
statistics.mean(nums) # 平均值，2

sum(nums) # 6
```

### 列表推导

```python
ls = ["a", "c", "b"]
new_ls = [item.title() for item in ls] # ['A', 'C', 'B']
```

## Tuple

列表可修改，元组不可修改（元组里的元素不能修改，但是元组本身的变量可以重新赋值）。

### 创建

```python
tp = (1, 2)
```

### 访问元素

```python
tp = (1, 2)
print(tp[0])
print(tp[1])

# 遍历
for item in tp:
	print(item)
```

## Set

集合是不含重复元素的列表。

### 创建

```python
st = {1, 2, 1, 0, 0}
print(st) # {0, 1, 2}
```

## Dict

### 创建

```python
dc = {"id": 1, "name": "dp"}
```

### 访问

```python
dc = {"id": 1, "name": "dp"}
print(dc["id"])
print(dc["name"])

# 使用 get() 访问
print(dc.get("id"))
print(dc.get("name"))
print(dc.get("age", 10)) # 不存在则使用默认值赋值

# 遍历，使用 items() 返回一个键值对列表
for k, v in dc.items():
	print(k, v)
	
# 遍历所有 key
for k in dc.keys():
	print(k)
	print(dc[k])

# 遍历所有 value
for v in dc.values():
	print(v)

# 排序
for k in sorted(dc.keys()):
	print(f"{k}: {dc[k]}")
```

### 加入元素

```python
dc = {}
dc["id"] = 1
dc["name"] = "dp"
```

### 修改

```python
dc = {"id": 1, "name": "dp"}
dc["name"] = "pding"
```

### 删除

```python
dc = {"id": 1, "name": "dp"}
del dc["id"]
print(dc) # {"name": "dp"}
```

## 函数

### 位置实参

调用函数时关联到函数定义的形参，顺序需要注意保持一致

```python
def new_user(id, name):
	return {"id": id, "name": name}

new_user(1, "dp")
```

### 关键字实参

调用函数时直接在实参中将名称和值关联起来，顺序无关紧要

```python
def new_user(id, name):
	return {"id": id, "name": name}

new_user(id=1, name="dp")
new_user(name="jay", id=2)
```

### 形参默认值

```python
def new_user(id, name="admin"):
	return {"id": id, "name": name}

new_user(1)
```

### 可选形参

可选参数放到参数列表最后面。

```python
def get_name(first_name, last_name, middle_name=""):
	if middle_name:
		return f"{first_name} {middle_name} {last_name}"
	else
		return f"{first_name} {last_name}"
```

### 任意数量实参

必须在函数定义中将接纳任意数量实参的形参放在最后。

Python 优先匹配位置形参和关键字形参，在将剩余的实参收集到最后一个形参中。

```python
def print_names(*names):
	print(f"第一个元素：{names[0]}") # 本质是元组，访问元组的第一个元素
	print(names)

print_names("dp")
print_names("dp", "jay")
```

`*names` 本质是创建了一个元组，将函数的参数全部封装到这个元组中。

### 任意数量的关键字实参

接收任意数量的键值对。

## 处理用户输入

```python
user_input = input("请输入：")
print(f"\n用户输入：{user_input}")
```

## 文件读写

### 读

```python
from pathlib import Path

path = Path("test.txt")
content = path.read_text()
print(content)
```

### 写

```python
from pathlib import Path

path = Path("test.txt")
path.write_text("Write something")
```

### 异常处理

```python
from pathlib import Path

try:
	path = Path("test_not_exist.txt")
	content = path.read_text()
	print(content)
except FileNotFoundError:
	print("未找到 'test.txt' 文件")
```

## JSON 处理

### 序列化

将变量转换成 JSON 格式数据。

```python
import json

nums = [1, 2, 3]
nums_json = json.dumps(nums)
```

### 反序列化

将 JSON 格式数据转换成变量。

```python
import json

nums_json = "[1, 2, 3]"
nums = json.loads(nums_json)
```
