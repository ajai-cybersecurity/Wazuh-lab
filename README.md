# Wazuh SIEM SOC Lab – Reverse Shell Detection 🔒

📑 **Table of Contents**

* Overview
* Lab Components
* System Architecture
* Setup Steps
* Detection Engineering
* Attack Simulation
* Attack Timeline
* Detection Results
* Indicators of Compromise
* Skills Demonstrated
* Recommendations
* Conclusion

---

# 🌐 Overview

This project demonstrates how to build a **Security Operations Center (SOC) monitoring lab using the Wazuh SIEM platform**.

The lab simulates a **real-world cyber attack scenario** where a Kali Linux attacker performs post-exploitation activities on a Windows endpoint. The generated logs are collected by the **Wazuh SIEM deployed on an Ubuntu server**, allowing security analysts to monitor, detect, and investigate malicious activity.

The attack scenario includes:

* Reverse shell command execution
* System reconnaissance
* Network discovery
* Tool transfer
* Persistence creation

All attacker behavior is analyzed using **SIEM alerts and mapped to MITRE ATT&CK techniques**.

---

# 💻 Lab Components

| Component     | Role                    | Description                                         |
| ------------- | ----------------------- | --------------------------------------------------- |
| Ubuntu Server | Wazuh Manager           | Central SIEM server that collects and analyzes logs |
| Windows 11    | Wazuh Agent             | Endpoint monitored by the SIEM                      |
| Kali Linux    | Attacker Machine        | Used to simulate attacker behavior                  |
| VirtualBox    | Virtualization Platform | Runs the lab environment                            |
| Wazuh         | SIEM Platform           | Log monitoring, detection, and alerting             |

---

# 🗺️ System Architecture

```
                ┌───────────────────────────────┐
                │        Ubuntu Server          │
                │      Wazuh SIEM Manager       │
                │       IP: 192.168.1.98        │
                └──────────────┬────────────────┘
                               │
                        Internal Lab Network
                               │
        ┌──────────────────────┴──────────────────────┐
        │                                             │
┌───────────────┐                           ┌────────────────┐
│  Windows 11   │                           │  Kali Linux    │
│ Wazuh Agent   │                           │   Attacker     │
│192.168.58.1   │                           │192.168.58.3    │
└───────────────┘                           └────────────────┘
```

---

# ⚙️ Setup Steps

## 1 Install Ubuntu Server

Install Ubuntu Server on VirtualBox.

Minimum configuration:

```
RAM: 4 GB
Storage: 20 GB
CPU: 2 cores
```

Update the system:

```bash
sudo apt update
sudo apt upgrade -y
```

---

## 2 Install Wazuh SIEM

Run the official Wazuh installation script.

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

After installation, access the dashboard:

```
https://<Ubuntu-IP>:443
```

---

## 3 Install Wazuh Agent on Windows

Download the Windows agent from:

```
https://wazuh.com/downloads/
```

During installation configure:

```
Manager IP : Ubuntu Server IP
Authentication Key : Generated from manage_agents
```

Verify connection in the dashboard:

```
Management → Agents
```

---

# 🔎 Detection Engineering

Custom detection rules were created in:

```
/var/ossec/etc/rules/local_rules.xml
```

These rules detect attacker behaviors such as:

* Reverse shell activity
* Suspicious PowerShell execution
* Reconnaissance commands
* Network discovery
* File creation
* Scheduled task persistence

---

# ⚔️ Attack Simulation

A **reverse shell attack simulation** was conducted using Kali Linux.

First, the attacker prepared a listener.

```
nc -lvnp 4444
```

Then a malicious PowerShell command was executed on the victim machine.

This caused the Windows system to connect back to the attacker over **TCP port 4444**, establishing a reverse shell.

Once access was gained, the attacker executed several commands to explore the environment.

Commands executed:

```
whoami
hostname
ipconfig
net user
arp -a
netstat -ano
Invoke-WebRequest
schtasks /create
```

---

# ⏱️ Attack Timeline

| Time     | Action               | Description                               |
| -------- | -------------------- | ----------------------------------------- |
| 09:54:09 | Reverse Shell        | Victim connected to attacker on port 4444 |
| 09:54:20 | PowerShell Execution | Suspicious PowerShell activity detected   |
| 09:54:23 | Reconnaissance       | whoami, hostname, ipconfig executed       |
| 09:54:27 | Network Discovery    | arp and netstat commands executed         |
| 09:55:02 | File Download        | File downloaded from attacker HTTP server |
| 09:55:02 | Persistence          | Scheduled task created                    |

---

# 🚨 Detection Results

The Wazuh SIEM successfully generated alerts for the simulated attack activities.

| Alert Name               | Severity | Trigger Event                  |
| ------------------------ | -------- | ------------------------------ |
| Reverse Shell Connection | High     | Suspicious outbound connection |
| PowerShell Execution     | High     | powershell.exe execution       |
| System Reconnaissance    | Medium   | whoami / hostname              |
| Network Discovery        | Medium   | arp / netstat                  |
| File Creation            | Medium   | Hacker.txt created             |
| Scheduled Task Created   | Critical | Event ID 4698                  |

---

# 🧾 Indicators of Compromise (IOC)

| Indicator        | Description                  |
| ---------------- | ---------------------------- |
| 192.168.58.3     | Attacker IP                  |
| Port 4444        | Reverse shell communication  |
| Port 80          | File download connection     |
| powershell.exe   | Suspicious command execution |
| Hacker.txt       | Downloaded attacker file     |
| schtasks command | Persistence mechanism        |

---

# 🧠 Skills Demonstrated

This project demonstrates several cybersecurity skills:

* SIEM deployment and administration
* Endpoint monitoring and log collection
* Detection rule engineering
* MITRE ATT&CK mapping
* Incident investigation
* Attack timeline reconstruction

---

# 🛡️ Recommendations

To improve detection capability in production environments:

* Deploy **Sysmon** for advanced telemetry.
* Integrate **network logs** for better visibility.
* Tune **SIEM detection rules** to reduce false positives.
* Implement **automated incident response**.
* Conduct **regular attack simulations**.
* Collect logs from **firewalls, DNS, and applications**.

# 📸 Screenshots

## Wazuh Dashboard

![Wazuh Dashboard](screenshots/Fig-1.png)

## Reverse Shell Listener

![Reverse Shell](screenshots/Fig-2.png)

## Custom Detection Rules

![Detection Rules](screenshots/Fig-3.png)

## SIEM Alerts

![Alerts](screenshots/Fig-4.png)


---

# 📌 Conclusion

This project demonstrates how a SOC monitoring lab can be built using the **Wazuh SIEM platform**.

During the attack simulation, a reverse shell connection was established from the victim system to the attacker machine. The attacker then performed reconnaissance, network discovery, file download, and persistence creation.

All malicious activities were successfully detected by the SIEM, allowing reconstruction of the full attack timeline.

The lab highlights how SIEM solutions help security teams **detect threats, analyze attacker behavior, and investigate security incidents effectively.**

---

