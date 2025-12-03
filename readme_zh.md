### 如何为新电脑配置 Python 开发环境

### 1 Powershell 和 代理
##### 1.1 检查网络访问
    [首先你需要处理代理，你可以使用 clash 或其他应用]
    [你也可以使用 watt-took-kit 来访问 github]
    [然后我们需要使用 winget 来安装一些应用]

##### 1.2 检查你的 powershell 版本以及 winget 是否可用
    [如果 winget 可用，那么]
    `winget search --id Microsoft.PowerShell`
    [否则，去 microsoft powershell 网页]
    下载 .msi 格式的 powershell 安装包并安装
    [然后你可能需要更新 winget](以管理员身份运行)
    `Install-Module -Name Microsoft.WinGet.Client -Force -Repository PSGallery`
    `Repair-WinGetPackageManager -AllUsers` 

##### 1.3 配置你的 powershell
    `Install-Module PSReadLine -Force -AllowClobber` (添加智能提示模块)
    `New-Item –Path $Profile –Type File –Force` (创建配置文件)
    `notepad $Profile` (编辑它)
    然后根据情况添加以下内容（比如你的端口）：

```shell
# powershell settings
# intellisense
Set-PSReadLineKeyHandler -Key Tab -Function AcceptSuggestion

# proxy
# manually
function proxy {
    # 设置 HTTP 和 HTTPS 代理
    $env:HTTP_PROXY = "http://127.0.0.1:7890"
    $env:HTTPS_PROXY = "http://127.0.0.1:7890"
    $env:ALL_PROXY = "socks5://127.0.0.1:7890" 
    Write-Host " Proxy ON at 127.0.0.1:7890" -ForegroundColor Green
}

function unproxy {
    $env:HTTP_PROXY = ""
    $env:HTTPS_PROXY = ""
    $env:ALL_PROXY = ""
    Write-Host " Proxy OFF" -ForegroundColor Yellow
}
# follow your system proxy
$regPath = "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
$proxyEnable = (Get-ItemProperty -Path $regPath).ProxyEnable
$proxyServer = (Get-ItemProperty -Path $regPath).ProxyServer

if ($proxyEnable -eq 1) {
    if ($proxyServer -match "(\d+\.\d+\.\d+\.\d+:\d+)") {
        $proxyAddress = "http://" + $matches[1]
        $env:HTTP_PROXY = $proxyAddress
        $env:HTTPS_PROXY = $proxyAddress
        $env:ALL_PROXY = $proxyAddress
        Write-Host " Proxy ON: $proxyAddress" -ForegroundColor Cyan
    }
}
```

### 2 Vscode
##### 2.1 安装
    去 vscode 官网下载并安装，(勾选所有选项)

##### 2.2 扩展
    - Git Extension Pack
    - Python Extension Pack
    [可选:]
    - Chinese LanguagePack
    - Github Theme
    - Material Icon Theme
    - Powershell
    - Markdown Preview Enhanced

##### 2.3 Git
    - 去它的官网按照指引操作，或者直接
    `winget install --id Git.Git -e --source winget`
    - 设置 git 账户
    `git config --global user.name "Your Name"`
    `git config --global user.email "your_email@example.com"`
    - 设置 git 代理，例如
    `git config --global http.proxy http://127.0.0.1:7890`
    `git config --global https.proxy http://127.0.0.1:7890`
    
##### 2.4 字体
    [我使用 JetBrains Mono]
    去它的官网下载
    进入 fonts\ttf\ 安装
        (JetBrainsMono-Bold.ttf 或 Regular Italic ...)
    设置 vscode 配置，搜索 font 并编辑 `JetBrains Mono, monospace`

### 3 Python
#### 3.1 Conda
    去它的官网 (miniconda) 并按照指引操作。
    安装时勾选所有选项。
    然后打开 powershell 并运行
    `conda init`
    你也可以：
        `conda env list` 检查环境
        `conda activate <envname>` 激活特定环境
        `conda install python=<version>` 更新 base 环境的 python
    [要修改新环境的文件路径，编辑 conda 文件夹下的 `.condarc`]
    添加你的路径，例如：
```
envs_dirs:
- C:\Coding\Envs\envs
pkgs_dirs:
- C:\Coding\Envs\pkgs
```
    然后确保用户对你的 envs 文件夹有权限（你可以右键点击它打开属性并在安全选项卡中更改）

#### 3.2 uv
    [我建议你使用 uv 来安装包]
    去它的官网并按照指引操作，或者
    `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
    然后使用 `uv pip install <module>` 来安装包
    如果出现错误提示 'No virtual environment found'
    将此添加到你的 powershell 配置文件中：
    `$Env:UV_SYSTEM_PYTHON = 1`
