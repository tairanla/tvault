# uv

## 安装

```bash
# MacOS & Linux curl
curl -LsSf <https://astral.sh/uv/install.sh> | sh

# MacOS
brew install uv

# MacOS & Linux wget
wget -qO- <https://astral.sh/uv/install.sh> | sh

# Windows 
powershell -ExecutionPolicy ByPass -c "irm <https://astral.sh/uv/install.ps1> | iex"

# Windows
winget install --id=astral-sh.uv  -e

# 直接使用 pip 安装
pip install uv
```

## Shell 自动补全

```bash
# bash
echo 'eval "$(uv generate-shell-completion bash)"' >> ~/.bashrc
echo 'eval "$(uvx --generate-shell-completion bash)"' >> ~/.bashrc

# zsh
echo 'eval "$(uv generate-shell-completion zsh)"' >> ~/.zshrc
echo 'eval "$(uvx --generate-shell-completion zsh)"' >> ~/.zshrc

# ps
if (!(Test-Path -Path $PROFILE)) {
  New-Item -ItemType File -Path $PROFILE -Force
}
Add-Content -Path $PROFILE -Value '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'
Add-Content -Path $PROFILE -Value '(& uvx --generate-shell-completion powershell) | Out-String | Invoke-Expression'
```

## 升级

```bash
uv self update

# pip 升级 uv
pip install --upgrade uv
```

## 卸载

```bash
# 1. 清除缓存
uv cache clean
rm -r "$(uv python dir)"
rm -r "$(uv tool dir)"

# 2. 卸载
# MacOS & Linux
rm ~/.local/bin/uv ~/.local/bin/uvx

# Windows ps
rm $HOME\\.local\\bin\\uv.exe
rm $HOME\\.local\\bin\\uvx.exe
rm $HOME\\.local\\bin\\uvw.exe
```

## 使用

安装不同版本的 Python：

```bash
uv python install 3.10 3.11 3.12 3.13
```

使用指定的 Python 版本：

```bash
uv python pin 3.12
```

创建虚拟环境：

```bash
uv venv
source .venv/bin/activate

# 退出虚拟环境
# deactivate
```

从 pip 迁移（适用于原项目使用 requirements.txt 管理项目依赖）：

```bash
uv init
uv add -r requirements.txt
```

参考：

- [https://docs.astral.sh/uv](https://docs.astral.sh/uv)

## uv Python 项目

```bash
# 创建项目
uv init my-project

# 创建 package
uv init my-app --package
# 创建 lib
uv init my-lib --lib
# 创建最小项目，只会在目录文件下创建一个 pyproject.toml 文件
uv init my-bare --bare

# 安装依赖
uv add requests
uv add --dev ruff pytest

# 运行工具
uv run ruff check
uv run pytest

# 运行
uv run main.py

# 从 GitHub 克隆的项目（没有环境），如何初始化？
uv sync

# 安装测试工具，安装完之后直接 pytest 可以使用了
uv tool install pytest
uv tool uninstall pytest

# 使用临时环境，并在文件中生成依赖信息
uv init --script main.py
# 临时环境运行
uv run main.py

# 删除环境
rm -r .venv
```

uvx 使用临时环境：

```bash
# 在临时环境使用 pytest
uvx pytest
```

## 常规 Python 项目

```bash
mkdir my_project && cd my_project

# 手动创建项目基础文件
touch README.md .gitignore main.py

# 创建虚拟环境
python -m venv .venv
source .venv/bin/activate

# 安装依赖
pip install requests

# 导出到 requirements.txt，每次新安装依赖后都需要重新导出
pip freeze > requirements txt

# 从 requirements.txt 安装依赖
pip install -r requirements.txt

# 退出并删除环境
deactivate
rm -rf .venv
```

## 配置国内镜像站点

1. 全局生效：设置全局环境变量

```bash
export UV_DEFAULT_INDEX=https://pypi.tuna.tsinghua.edu.cn/simple
```

2. 项目生效：编辑 _pyproject.toml_ 文件，添加配置

```bash
[[tool.uv.index]]
url = "<https://pypi.tuna.tsinghua.edu.cn/simple>"
default = true
```

## 发布

### 注册 pypi 账号

[https://pypi.org/account/register/](https://pypi.org/account/register/)

### 生成一个 Token

我们发布上传包到 pypi 的时候需要使用到这个 token

[https://pypi.org/manage/account/token/](https://pypi.org/manage/account/token/)

### 初始化一个项目

```bash
uv init --lib my-py-project && cd my-py-project
uv build
uv publish
```

> 执行 uv publish 后，username 输入 `__token__` ，密码输入获取到的 pypi Token。

## 参考

- [https://docs.astral.sh/uv/](https://docs.astral.sh/uv/)

[发布一个包](https://www.notion.so/20265182765c80519175f973c8ef9b2c?pvs=21)
