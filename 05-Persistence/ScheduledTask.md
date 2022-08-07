Adversaries may abuse task scheduling functionality to facilitate initial or recurring execution of malicious code.

One of the most famous persistence techniques is creating a scheduled task that will execute within a time range to execute the target code.

The following line can create a scheduled task that will execute every minute. After that, a shell under the C:\tmp\shell.cmd path is executed.

`schtasks /create /sc minute /mo 1 /tn "persistenttask" /tr C:\tmp\shell.cmd /ru "SYSTEM"`
