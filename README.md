# 终端美化方案（Windows）

## 使用工具

PowerShell 7.x版本（Windows Terminal）

​	推荐使用 PowerShell 7

​	Windows Terminal 是一个终端外壳，powershell才是真正的终端，win11中 WT 内置，win10需要安装。

Oh-My-Posh	终端提示符美化、增强引擎

Caskaydia Cove Nerd Font	开发者常用字体，内置git、json等专属图标

Terminal-Icons	用于在终端显示不同图标

Posh-Git		提供OhMyPosh对`git`的支持，可实时显示git分支等

fastfetch		进入终端显示的相关信息、logo

## 相关工具下载

winget	-- Windows自带的包管理工具。

PowerShell 7

```powershell
winget install Microsoft.PowerShell
```

Oh-My-Posh

```powershell
winget install JanDeDobbeleer.OhMyPosh
```

字体

 [下载地址](https://www.nerdfonts.com/font-downloads)，选择一个字体，下载后解压缩。

打开 设置 -> 个性化 -> 字体，可看到添加字体的窗口，将解压缩后的目录中所有的`ttf`文件全部拖入其中，等待系统自动安装完成。

FastFetch

```powershell
winget install fastfetch
```

Posh-Git、Terminal-Icons

```powershell
Install-Module -Name Terminal-Icons -Repository PSGallery
Install-Module posh-git -Scope CurrentUser
```

## 开始配置

### 设置默认终端

终端页按 `Ctrl + ,`

![](https://raw.githubusercontent.com/tcole-dev/LearningNotes/master/img/QQ20260423-014505.png) 

powershell 7版本的图标是黑底白字，而不是 win 11、win 10自带的powershell 5的蓝底白字。

### Oh-My-Posh配置

根据上述命令直接安装的oh-my-posh版本中，没有`Get-PoshThemes`等命令和自带的主题，需要我们自行下载GitHub上的一些主题，保存到本地。

```powershell
# 在C:\Uers\用户名 下创建.post-thems，保存主题
mkdir ~/.posh-themes
```

下载主题（直接复制到终端运行即可）

```powershell
Invoke-WebRequest -Uri https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -OutFile ~/.posh-themes/themes.zip
Expand-Archive -Path ~/.posh-themes/themes.zip -DestinationPath ~/.posh-themes -Force
Remove-Item ~/.posh-themes/themes.zip
```

​	此外，还有在线使用方案，不需要下载，但必须保持网络畅通：

```powershell
oh-my-posh init pwsh --config https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/jandedobbeleer.omp.json | Invoke-Expression
```

**设置主题**

在终端输入`notepad $PROFILE`（powershell初始化时执行，后续很多操作都需要修改这个文件），添加：

```powershell
oh-my-posh init pwsh --config "$HOME/.posh-themes/jandedobbeleer.omp.json" | Invoke-Expression
```

<font color=orange>这里的`jandedobbeleer.omp.json`只是举例，可以选择任意喜欢的主题，在 [此网站](https://ohmyposh.dev/docs/themes) 可以进行预览不同主题，再将`jandedobbeleer`修改为其他主题的名字</font>

配置完毕后，退出重进，即可生效。

### 终端配色

我们能注意到WSL、cmd、PowerShell还有Linux的各终端，配色都不太相同，比如文字的颜色等。如何进行修改呢？

打开终端设置页，在终端页面 `Ctrl + ,` 可以打开设置，在相应终端的外观页中，可以为其设置不同的已有配色方案，但还是不够，我们可以自行创建一个新的方案，或是直接使用他人的方案。

[此网站](https://windowsterminalthemes.dev/)中可以获取更多配色方案，点击Get Theme，即可复制配色方案json

终端页按 `Ctrl + Shift + ,` 进入配置文件。（非 $PROFILE），在`schemes`的列表中复制获取到的json，保存后即可。

这里给出一个配色方案：

```json
{
	"background" : "#282C34",
	"black" : "#282C34",
	"blue" : "#61AFEF",
	"brightBlack" : "#808080",
	"brightBlue" : "#61AFEF",
	"brightCyan" : "#56B6C2",
	"brightGreen" : "#98C379",
	"brightPurple" : "#C678DD",
	"brightRed" : "#E06C75",
	"brightWhite" : "#DCDFE4",
	"brightYellow" : "#E5C07B",
	"cyan" : "#56B6C2",
	"foreground" : "#DCDFE4",
	"green" : "#98C379",
	"name" : "One Half Dark",
	"purple" : "#C678DD",
	"red" : "#E06C75",
	"white" : "#DCDFE4",
	"yellow" : "#E5C07B"
},
{
	"background" : "#FAFAFA",
	"black" : "#383A42",
	"blue" : "#0184BC",
	"brightBlack" : "#383A42",
	"brightBlue" : "#0184BC",
	"brightCyan" : "#0997B3",
	"brightGreen" : "#50A14F",
	"brightPurple" : "#A626A4",
	"brightRed" : "#E45649",
	"brightWhite" : "#FAFAFA",
	"brightYellow" : "#C18401",
	"cyan" : "#0997B3",
	"foreground" : "#383A42",
	"green" : "#50A14F",
	"name" : "One Half Light",
	"purple" : "#A626A4",
	"red" : "#E45649",
	"white" : "#FAFAFA",
	"yellow" : "#C18401"
}
```

### Posh-Git、Terminal-Icons

这两者的配置很简单，终端输入`notepad $PROFILE`，添加：

```powershell
Import-Module Terminal-Icons
Import-Module posh-git
```

保存退出即可。

![](https://raw.githubusercontent.com/tcole-dev/LearningNotes/master/img/QQ20260423-014505.png) 

此时就可以看到 git 的分支、ls列出文件对应图标。

### FastFetch

fastfetch命令可以输出当前操作系统的一些信息、logo，这些信息的选项都可以提供配置文件进行选择。

找到fastfetch的配置文件：`C:\Users\用户名\.config\fastfetch\config.jsonc`，若没有，则使用`fastfetch --gen-config`生成。

打开配置文件，其中`modules`中的不同选项即为fastfetch右边显示的各种信息，logo为左边显示的操作系统的logo。

```json
"modules": [
    "os",
    "kernel",
    "uptime",
    "cpu",
    "gpu",
    "memory",
    "disk",
    "localip",
    "publicip"
]
```

**logo的自定义**

这里介绍一个妙妙工具：`chafa`，GitHub[仓库](https://github.com/hpjansson/chafa)，我没有在GitHub的releases中找到Windows的版本，需要Windows版本，[下载地址](https://hpjansson.org/chafa/download/)

这个工具可以将图片转化为可用于FastFetch logo的像素图，这里推荐直接用chafa生成ANSI文件，每次FastFetch直接使用该文件即可。因此如果有wsl环境，也可以直接用Linux版本。

```bash
chafa D:/document/picture/name.png --size=30x15 > logo.ansi
```

config.jsonc 中配置：

```
"logo": {
    "type": "file",
    "source": "D:/document/picture/logo.ansi"
}
```

### 背景图

终端页，`Ctrl + ,` -> 指定终端 -> 外观 -> 背景图像。注意修改不透明度，不透明过高，有时候会显得很违和

![](https://raw.githubusercontent.com/tcole-dev/LearningNotes/master/img/QQ20260423-005014.png) 

同时，还可以设置终端的不透明度，在上面的位置继续下滑：

![](https://raw.githubusercontent.com/tcole-dev/LearningNotes/master/img/QQ20260423-005040.png) 

### 最终效果

![](https://raw.githubusercontent.com/tcole-dev/LearningNotes/master/img/终端最终效果.png) 

## 总结

终端的美化到这里也就差不多结束了，有很多地方没有深入解释，也没有过于深入的进行定制，感兴趣的同学可以自行探索。

Oh-My-Posh可能导致进入终端速度比原来稍慢，如上图达到了1.692秒，如果不能接受，可以尝试使用`StarrShip`，比Oh-My-Posh更加轻量、搞笑，但主题支持稍少。

参考文章：

[乘风破浪，遇见最美Windows 11之现代Windows开发运维 - 再谈Windows Terminal(终端)主题和字体美化，Oh-My-Posh、Terminal-Icons、Posh-git](https://www.cnblogs.com/taylorshi/p/16482694.html)