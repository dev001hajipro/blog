---
title: "Powershell"
date: 2022-04-16T15:41:11+09:00
draft: false
---

## PowerShellで管理者権限で実行させるスクリプト

PowerShell4以降は、先頭行に`#Requires -RunAsAdministrator`を書く。

```powershell
#Requires -RunAsAdministrator
```

例えば、ユーザー環境変数やシステム環境変数をスクリプトで設定する場合は、管理者権限で動作させる必要がある。

```PowerShell
Write-Output "add XYZXYZXYZ to User environment variable."
[Environment]::SetEnvironmentVariable('XYZXYZXYZ', 'abc', [System.EnvironmentVariableTarget]::User)
[Environment]::GetEnvironmentVariable('XYZXYZXYZ', [System.EnvironmentVariableTarget]::User)

Write-Output "add ZZZZZZZZZ to Machine(=System) environment variable."
[System.Environment]::SetEnvironmentVariable('ZZZZZZZZZ', 'abc', [System.EnvironmentVariableTarget]::Machine)
[System.Environment]::GetEnvironmentVariable('ZZZZZZZZZ', [System.EnvironmentVariableTarget]::Machine)
```
