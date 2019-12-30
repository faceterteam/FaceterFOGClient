# Faceter Fog Agent

The part of Faceter Fog Client which provide node state, mark and Facial Recognition tasks interoperability.

* Copy [fogagent.service](fogagent.service) into в /etc/systemd/system
* Run folow comands:
```bash
systemctl daemon-reload
systemctl enable fogagent
```
* Copy [agent-start.sh](agent-start.sh) into main Fog agent folder
* Set your FoggerId and NodeId into agent-start.sh
* Check the status with 
```bash
systemctl status fogagent
```
