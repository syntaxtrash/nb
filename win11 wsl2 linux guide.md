---
title: win11 wsl2 linux guide
created: '2024-08-28T09:31:17.317Z'
modified: '2024-09-24T15:08:15.349Z'
---

# win11 wsl2 linux guide
by why? 
pretty much everything I deploy runs on a linux server. Might as well make my self get used to it.

why not just commit and leave windows?
wsl2 is a pretty good start I think, I do coding in wsl2, everything coding related stays there, unless I really need to use windows. I do gaming on windows. 

who cares?
you probably know me irl and will ask me a reason why this, now you know the answer.

## Installation

install wsl:
```bash
wsl --install
# installs ubuntu by default.
```

see linux distros available:
```bash
wsl --list --online
# or wsl -l -o
```

install specific linux distro:
```bash
wsl --install -d <Distribution Name>
```

If you want to install additional distributions from inside a Linux/Bash command line (rather than from PowerShell or Command Prompt), you must use `.exe` in the command: 
```bash
wsl.exe --install -d <Distribution Name>
```

or to list available distributions: 
```bash
wsl.exe -l -o
```

read more : https://learn.microsoft.com/en-us/windows/wsl/install#install-wsl-command

## Uninstall 
uninstall linux distro:
```bash
wsl --unregister <Distribution Name>
```

read more: https://learn.microsoft.com/en-us/windows/wsl/basic-commands#unregister-or-uninstall-a-linux-distribution
