Turn off Windows Defender

`Set-MpPreference -DisableRealtimeMonitoring $true`

Turn off Windows Firewall

`NetSh Advfirewall set allprofiles state off`