
> **标注有 [!] 符号的条例非常重要，一定要重视，一定要重视！！！**
> *在操作之前，你需要思考自己是否适合linux，同时，如果你有玩游戏的需求，请在[ProtonDB](https://www.protondb.com/)或者别的可以的地方查询是否有你游玩的游戏*

很多的相关数据都会默认的保存在Roaming，Local文件夹中，你可以通过在`win+R`后输入`%AppData%` 打开这些文件夹

# first. all

1. [x] 如果你现在使用的windows账号是正版预装的，记得回忆一下你是否还记得你的账号密码；如果你是手动购买的密钥，你可以通过相关的工具（比如ReoduKey）来查询你的密钥
2. [x] 记录网卡，显卡型号 [!]
	```bash
	Get-PnpDevice -PresentOnly | Select-Object FriendlyName, InstanceId | Out-File D:\HardwareList.txt
	```
> 该命令需要终端具有管理员权限
你不需要备份任何的驱动，在linux下无法使用任何的windows驱动；比如可以通过easyeffects来模拟出类似Waves MaxxAudio的重低音和清晰度

这里简单提及一下，通过这条命令，可以完整的备份所有的驱动文件，虽然这对linux完全没有作用：`dism /online /export-driver /destination:"D:\DriversBackup"`
3. [x] 关闭 BitLocker 加密 [!]
4. [x] 关闭 windows 快速启动 [!]
一般情况下按照这样的流程执行即可，如果不适用，请自行查阅相关资料：
```txt
控制面板 -> 电源选项 -> 选择电源按钮的功能 -> 更改当前不可用的设置 -> 取消勾选启用快速启动
```
5. [x] 备份社交应用的聊天记录等，包括但不限于Wechat, QQ, telegram, Discord，像是discord这种云端存储聊天数据的应用，同样需要备份私人聊天数据
6. [x] 导出浏览器书签，建议开启Chrome, edge的账号同步，也可以手动导出书签文件
7. [x] 导出windows中的软件列表:
	```bash
	Get-ItemProperty HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName
	```
以上的命令只能看到32位程序，如果想要一份完整的软件清单，可以使用这个命令：
```bash
Get-ItemProperty HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall\*, HKLM:\Software\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*, HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\* | Select-Object DisplayName | Sort-Object DisplayName | Where-Object { $_.DisplayName -ne $null }
```
8. [x] 导出SSH 密钥 [!]
路径： `C:\Users\uesrname\.ssh` 该路径包含Github/服务器的连接密钥
9. [x] Host文件： `C:\Windows\System32\drivers\etc\hosts` ，如果你有手动配置过服务器的映射，你需要备份这个文件目录
10. [x] 备份字体，一般情况下,最好备份路径`C:\Windows\Fonts`类似微软雅黑，Consolas或者宋体，否则在linux中可的word可能会出现排版错误，并且有的用户的字体会安装在`C:\Users\username\AppData\Local\Microsoft\Windows\Fonts`，同样需要备份；
在 Linux 下，中文字体渲染（Hinting/Antialiasing）和 Windows 不同，建议后续在 CachyOS 中安装 `noto-fonts-cjk` 并配置好 `fontconfig`
11. [x] 备份媒体数据，包括但不限于 `C:\Users\username` 下的所有docs, pictures, downloads
很多现代软件（如 Obsidian, Zotero, VS Code 插件）的数据都在 `%AppData%` 下。建议完整备份整个 `%AppData%` 和 `%LocalAppData%` 文件夹
12. [x] 备份游戏存档
13. [x] 如果你配置并使用过类似Git，建议同样备份：`C:\Users\username\.gitconfig`
14. [x] 如果你存在任何的代码仓库，建议全部 `git push` 到云端 `GitHub/Gitee`
15. [ ] 进入 BIOS/UEFI 设置：
	1. [ ] Secure Boot： 设置成 disabled
	2. [ ] SATA Mode: 如果硬盘模式为 RAID ON ，建议修改成 AHCI ; 但是这样会导致原来的windows无法启动
虽然 CachyOS 支持通过 `sbctl` 等工具手动配置 Secure Boot，但初次安装**务必先关闭**。如果你必须开启（为了刷双系统打某些带反作弊的游戏），过程会非常繁琐
16. [x] 导出/解绑 专业软件授权
某些专业软件（如 Adobe 全家桶、IDM、某些 IDE、或者带加密狗的软件）是绑定硬件 ID 或系统 ID 的。在重装系统前，**务必在软件内执行“注销/反激活”操作**，否则在 Linux 下可能无法通过虚拟机或 Wine 再次激活，甚至导致授权失效
17. [!] 提前准备一个代理网络应用安装包
18. [x] 备份输入法词库
如果你有长期积累的输入法习惯（搜狗、微信输入法等），记得导出用户词库。在 Linux 下使用 Rime 或 Fcitx5 时可以尝试转换导入
如果你使用的是官方自带的微软输入法，可以按照一下的流程尝试：
```txt
设置 -> 语言和区域 -> 找到你的语言旁边的... -> 点击之后选择语言选项 -> 找到微软拼音旁边的... -> 词库和自学习 -> 导出自学习词汇和添加或编辑自定义短语 -> 全部导出
```
19. [x] 浏览器扩展数据
虽然开启了账号同步，但某些插件（如 Authenticator 二步验证插件、暴力猴脚本、Proxy SwitchyOmega 配置）可能不会自动同步，建议手动导出备份
20. [ ] 在格式化硬盘，先制作一个 Linux 的 live USB 启动盘，然后从 U 盘启动进入使用模式 Try Linux [!]
	1. [ ] 检查wifi
	2. [ ] 检查声音
	3. [ ] 检查摄像头
	4. [ ] 检查触控板

---

# second. cachyOS/Arch

> 单独对CachyOS/Arch说明

1. [x] 检查 CPU 指令集兼容性
CachyOS 默认提供针对 `x86-64-v3` 和 `v4` 优化的内核与仓库
虽然现在的电脑大多支持 v3，但建议在 Windows 下先用 `CPU-Z` 或在终端运行相关脚本确认你的 CPU 是否支持 `AVX2` (v3) 或 `AVX512` (v4)。如果不支持，安装时需要选择兼容版本
```bash
PS C:\WINDOWS\system32> Get-CimInstance Win32_Processor | Select-Object Name

PS C:\WINDOWS\system32> $env:PROCESSOR_IDENTIFIER
```
1. [x] 备份 GPG 密钥
如果你在 Git 中使用了 GPG 签名（除了 SSH 密钥外），记得备份 `C:\Users\username\AppData\Roaming\gnupg` 文件夹
2. [x] WSL 导出
如果你在 Windows 下使用了 WSL (Windows Subsystem for Linux)，记得使用 `wsl --export <Distro> <Path>.tar` 导出镜像，或者直接进入 WSL 备份里面的代码
3. [ ] 关于第 16 条 (Live USB 检查) 
在 CachyOS 的 Live 环境下，请额外检查：
    [!] NVIDIA 显卡驱动： CachyOS 安装器通常会询问是否安装闭源驱动。在 Live 环境下确认 `lsmod | grep nvidia` 是否加载（如果选择了 NVIDIA 引导项）
    [!] 高刷新率支持： 检查显示设置里能否正常开启 144Hz/240Hz
    [!] 深度睡眠 (Suspend/Hibernate)： 笔记本用户务必测试闭合盖子后能否正常唤醒，这是 Linux 笔记本最常见的痛点

---

# lastest. attention

最后

[!] 制作全盘镜像（可选但强烈建议）
如果你硬盘空间充裕，建议使用 `Clonezilla` 或 `微PE` 下的 `傲梅轻松备份` 制作一个全盘的 `GHO` 或 `WIM` 镜像放在移动硬盘里
万一你在 Linux 下遇到无法解决的硬件兼容问题（比如某些高端笔记本的音频阵列驱动），这个镜像可以让你 10 分钟内原封不动地回到 Windows

---

# personal

```bash
PS C:\WINDOWS\system32> Get-CimInstance Win32_Processor | Select-Object Name
Name
----
13th Gen Intel(R) Core(TM) i7-13620H
PS C:\WINDOWS\system32> $env:PROCESSOR_IDENTIFIER
Intel64 Family 6 Model 186 Stepping 2, GenuineIntel
```

repository选择： `x86-64-v3`
kernel选择： `linux-cachyos`
驱动选择： 需要勾选NVIDIA闭源驱动