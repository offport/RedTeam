Turn off Windows Defender

`Set-MpPreference -DisableRealtimeMonitoring $true`

`New-ItemProperty -Path "HKLM:\SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 1 -PropertyType DWORD -Force`

Turn off Windows Firewall

`NetSh Advfirewall set allprofiles state off`