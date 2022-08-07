Adversaries may shutdown/reboot systems to interrupt access to, or aid in the destruction of, those systems. Operating systems may contain commands to initiate a shutdown/reboot of a machine or network device.

**Linux**

- Shutdown
  - `sudo shutdown -n now`
  - `sudo shutdown -P now`
  - shut down after 10 min `sudo shutdown -r 10`

- Reboot
  - `sudo reboot`
  - `sudo shutdown -r now`


**Windows**

- Shutdown
  - `shutdown /s`

- Reboot
  - `shutdown /s`

- Log off
  - `shutdown /l`

**OSX**

- Shutdown
  - `sudo shutdown -h now`
  - shut down after 30 min `sudo shutdown -h +30`

- Reboot
  - `sudo shutdown -r now`
