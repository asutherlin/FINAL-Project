# Blue Team: Summary of Operations

## Table of Contents
- Network Topology
- Description of Targets
- Monitoring the Targets
- Patterns of Traffic & Behavior
- Suggestions for Going Further

### Network Topology
<a href="url"><img src="https://github.com/asutherlin/FINAL-Project/blob/main/screen_shots/Final%20Project%20Topology%20(1).png" align="right" height="500" width="500" ></a>

The following machines were identified on the network:
- ELK 
  - **Operating System**: Linux
  - **Purpose**: Kibana logging events
  - **IP Address**: 192.168.1.100
- Capstone
  - **Operating System**: Linux
  - **Purpose**: N/A in this project
  - **IP Address**: 192.168.1.105
- Kali
  - **Operating System**: Kali Linux
  - **Purpose**: Red Team Attacker Machine
  - **IP Address**: 192.168.1.90
- Target 1
  - **Operating System**: Linux
  - **Purpose**: Target
  - **IP Address**: 192.168.1.110

---

### Description of Targets
The target of this attack was: `Target 1` 192.168.1.110

Target 1 is an Apache web server and has SSH enabled, so ports 80 and 22 are possible ports of entry for attackers. As such, the following alerts have been implemented:

### Monitoring the Targets

#### Traffic to these services should be carefully monitored. To this end, we have implemented the alerts below:
---
<a href="url"><img src="https://github.com/asutherlin/FINAL-Project/blob/main/screen_shots/HTTP%20Request%20Size%20Monitor1.png" align="right" height="250" width="450" ></a>
#### HTTP Request Size Monitor

The “HTTP Request Size Monitor” is implemented as follows:
 - **Metric**: http.request.bytes
  - **Threshold**: The SUM of bytes over all documents is ABOVE 3500 for the last 1 minute
  - **Vulnerability Mitigated**: HTTP Request Smuggling
  - **Reliability**: Low. Not a perfect solution because you can continue to have false positives until a more accurate baseline is applied against the threshold. 

---
<a href="url"><img src="https://github.com/asutherlin/FINAL-Project/blob/main/screen_shots/Excessive%20HTTP%20ERRORS1.png" align="right" height="250" width="450" ></a>
#### Excessive HTTP Errors

The “Excessive HTTP Errors” is implemented as follows:
  - **Metric**: http.response.status_code
  - **Threshold**: The TOP 5 get ABOVE 400 for the last 5 minutes
  - **Vulnerability Mitigated**: Brute Force Attack
  - **Reliability**: Medium. Threshold is dependent on the normal number of users logged in and using the application. Specificity of the status code,, such as in the 400s, would make this more reliable. 

---
<a href="url"><img src="https://github.com/asutherlin/FINAL-Project/blob/main/screen_shots/Edit%20CPU%20Usage%20Monitor1.png" align="right" height="250" width="450" ></a>
#### CPU Usage Monitor
The “CPU Usage Monitor” is implemented as follows:
  - **Metric**: system.process.cpu.total.pct
  - **Threshold**: The MAX of pct over all documents is ABOVE 0.5 for the last 5 minutes
  - **Vulnerability Mitigated**: Possible malicious software or DDoS Attacks
  - **Reliability**: Medium. A spike in the CPU can be caused by normal daily usage. However, you can catch unexpected actions, such as updates, outside of normal schedule. 
