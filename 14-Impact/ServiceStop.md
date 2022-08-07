Adversaries may stop or disable services on a system to render those services unavailable to legitimate users. 

**Linux**
- List all services `systemctl list-unit-files --type service -all`
- Start services `sudo systemctl start service.service`
- Restart services `sudo systemctl restart service.service`
- Enable services `sudo systemctl enable service.service`
- Disable services `sudo systemctl disable service.service`

**Windows**
- To start a service, type: `net start ServiceName`
- To stop a service, type: `net stop ServiceName`
- To pause a service, type: `net pause ServiceName`
- To resume a service, type: `net continue ServiceName`
