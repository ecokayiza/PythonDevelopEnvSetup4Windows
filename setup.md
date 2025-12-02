### how to config python develop env for a new computer

### 1 Powershell and Proxy
##### 1.1 check your web access 
    [first you need to handle proxy, you can use clash or any app]
    [you can also use watt-took-kit to access github]
    [then We need to use winget to install some apps]

##### 1.2 check your powershell version and if winget is ok
    [if winget is ok, then]
    `winget search --id Microsoft.PowerShell`
    [else, go to microsoft powershell web page]
    download powshell package with .msi and install
##### 1.3 config your powershell
    `Install-Module PSReadLine -Force -AllowClobber` (add intellisence module)
    `New-Item –Path $Profile –Type File –Force` (to create config file)
    `notepad $Profile` (to edit it)
    then add those contents accordingly(like your port):

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
            Write-Host " 已开启代理: $proxyAddress" -ForegroundColor Cyan
        }
    }
    ```

    [then you might need to update winget]
    `winget upgrade Microsoft.AppInstaller` (use cmd and run as administrator)

### 2 Vscode
##### 1 setup
    go to vscode website and dowanload it then install, (check all options)

##### 2 extensions
    - Git Extension Pack
    - Python Extension Pack
    [optional:]
    - Chinese LanguagePack
    - Github Theme
    - Material Icon Theme
    - Powershell
    - Markdown Preview Enhanced

##### 3 font
    [I use JetBrains Mono]
    go to its web can download
    go inside fonts\ttf\  to install
        (JetBrainsMono-Bold.ttf or Regular Italic ...)
    set vscode config , search font and edit `JetBrains Mono, monospace`

### 3 Python
#### 1 Conda
    Go to its website (miniconda) and follow it.
    check all options while installing.
    then open powershell and run
    `conda init`
    you can also:
        `conda env list` to check envs
        `conda activate <envname>` to activate certain env
        `conda install python=<version>` to update python in base env
#### 2 uv
    [I suggest you use uv to install paks]
    go to its website and flow it or 
    `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
    then use `uv pip install <module>` to install paks
    if there is erros says 'No virtual environment found'
    add this to your powershell config file:
    `$Env:UV_SYSTEM_PYTHON = 1`
