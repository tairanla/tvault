# Telepresence

## 安装

### Windows

Windows ARM64

```bash
# To install Telepresence, run the following commands
# from PowerShell as Administrator.

# 1. Download the latest windows zip containing telepresence.exe and its dependencies (~60 MB):
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-windows-amd64.zip -OutFile telepresence.zip

# 2. Unzip the telepresence.zip file to the desired directory, then remove the zip file:
Expand-Archive -Path telepresence.zip -DestinationPath telepresenceInstaller/telepresence
Remove-Item 'telepresence.zip'
cd telepresenceInstaller/telepresence

# 3. Run the install-telepresence.ps1 to install telepresence's dependencies. It will install telepresence to
# C:\telepresence by default, but you can specify a custom path by passing in -Path C:\my\custom\path
powershell.exe -ExecutionPolicy bypass -c " . '.\install-telepresence.ps1';"

# 4. Remove the unzipped directory:
cd ../..
Remove-Item telepresenceInstaller -Recurse -Confirm:$false -Force

# 5. Telepresence is now installed and you can use telepresence commands in PowerShell.
```

Windows ARM64

```bash
# To install Telepresence, run the following commands
# from PowerShell as Administrator.

# 1. Download the latest windows zip containing telepresence.exe and its dependencies (~60 MB):
$ProgressPreference = 'SilentlyContinue'
Invoke-WebRequest https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-windows-arm64.zip -OutFile telepresence.zip

# 2. Unzip the telepresence.zip file to the desired directory, then remove the zip file:
Expand-Archive -Path telepresence.zip -DestinationPath telepresenceInstaller/telepresence
Remove-Item 'telepresence.zip'
cd telepresenceInstaller/telepresence

# 3. Run the install-telepresence.ps1 to install telepresence's dependencies. It will install telepresence to
# C:\telepresence by default, but you can specify a custom path by passing in -Path C:\my\custom\path
powershell.exe -ExecutionPolicy bypass -c " . '.\install-telepresence.ps1';"

# 4. Remove the unzipped directory:
cd ../..
Remove-Item telepresenceInstaller -Recurse -Confirm:$false -Force

# 5. Telepresence is now installed and you can use telepresence commands in PowerShell.
```

### MacOS

brew 安装：

```bash
brew install telepresenceio/telepresence/telepresence-oss
```

AMD（Intel）二进制：

```bash
# 1. Download the binary.
sudo curl -fL https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-darwin-amd64 -o /usr/local/bin/telepresence

# 2. Make the binary executable:
sudo chmod a+x /usr/local/bin/telepresence
```

ARM（Apple Silicon）二进制：

```bash
# 1. Ensure that no old binary exists. This is very important because Silicon macs track the executable's signature
# and just updating it in place will not work.
sudo rm -f /usr/local/bin/telepresence

# 2. Download the binary.
sudo curl -fL https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-darwin-arm64 -o /usr/local/bin/telepresence

# 3. Make the binary executable:
sudo chmod a+x /usr/local/bin/telepresence
```

### Linux

```bash
# 1. Download the latest binary (~95 MB):
# AMD
sudo curl -fL https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-linux-amd64 -o /usr/local/bin/telepresence

# ARM
sudo curl -fL https://github.com/telepresenceio/telepresence/releases/latest/download/telepresence-linux-arm64 -o /usr/local/bin/telepresence

# 2. Make the binary executable:
sudo chmod a+x /usr/local/bin/telepresence
```

## 目标集群安装 Traffic Manager

### 安装

配置 .kube/config，并且 context 指定为目标集群。

```bash
telepresence helm install
```

### 升级

使用升级了版本的 telepresence 命令行工具执行：

```bash
telepresence helm upgrade
```

### 卸载

```bash
telepresence helm uninstall
```

## 连接

```bash
telepresence connect
```

此时，在本地可以访问集群端 Service 了。

- 以 service dns 访问：`curl <service-name>.<namespace>.svc.cluster.local`
- 以 service ip 访问：`curl <service-ip>`

## 本地访问集群 Pod IP

```bash
telepresence connect --also-proxy 10.244.0.0/16
```

> 其中 10.244.0.0/16 是集群中 Pod CIDR。

以 pod ip 访问：`curl <pod-ip>`
