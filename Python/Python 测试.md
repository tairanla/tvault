# Python 测试

## pytest

- 使用 pip 安装 pytest

```python
# 更新 pip，因为 pip 的更新较为频繁
python3 -m pip install --upgrade pip

# 安装 pytest
python3 -m pip install --upgrade pytest
```

- 使用 uv 安装 pytest

```python
uv tool install pytest # 安装
uv tool upgrade pytest # 升级版本
uv tool uninstall pytest # 卸载
```

## 规范

在默认情况（可以自定义规则）下，pytest 运行测试有一些规则要求：

- 测试文件必须以 `test_` 作为前缀
- 测试函数必须以 `test_` 作为前缀

## 示例

- _name_func.py_

```python
def get_full_name(f_name, l_name, m_name=""):
	if m_name:
		full_name = f"{f_name} {m_name} {l_name}"
	else:
		full_name = f"{f_name} {l_name}"
	return full_name.title()
```

- _test_name_func.py_

```python
from name_func import get_full_name

def test_first_last_name():
	full_name = get_full_name("a", "b")
	
	assert full_name == "A B"

def test_first_last_middle_name():
	full_name = get_full_name("a", "b", "c")
	
	assert full_name == "A B C"
```

- 运行测试：

```bash
uv run pytest test_name_func.py
```

## pytest 夹具

夹具可以帮助我们创建供多个测试使用的资源，使用 `@pytest.fixture` 装饰器标记。

_夹具标记的函数名与测试函数的某个形参同名，将自动运行夹具，得到返回值传递给测试函数。_

```python
class User:
	def __init__(self, id, name):
		self.id = id
		self.name = name
		
import pytest

@pytest.fixture
def user():
	return User(1, "dp")

def test_get_name(user):
	assert user.name == "dp"

def test_user_id(user):
	assert user.id == "id"
```
