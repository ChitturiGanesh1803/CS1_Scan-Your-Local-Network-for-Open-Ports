Network Scan Report

This project contains a reconnaissance exercise using Nmap to discover open ports, identify active services, and understand potential security risks within a local network.

1. Overview

A TCP SYN scan was performed on the target network:

nmap -sS 192.20.10.0/24


Four hosts responded. The goal was to enumerate open ports, map services, and assess exposure.

2. Host Findings
Host: 192.20.10.1

Open Ports

21/tcp — FTP

53/tcp — DNS

49152/tcp — Dynamic/Ephemeral

62078/tcp — Device sync protocol

Risk Summary

FTP uses clear-text authentication.

DNS exposure may allow poisoning or misconfig misuse.

High dynamic ports may leak internal RPC functions.

Sync services should not be exposed across the LAN.

Host: 192.20.10.2

This system exposes a large number of services, similar to a lab VM or intentionally vulnerable machine.

Open Ports (Notable)

21 — FTP

22 — SSH

23 — Telnet

25 — SMTP

53 — DNS

80 — HTTP

111 — rpcbind

139/445 — SMB

512/513/514 — r-services

1099 — Java RMI

1524 — ingreslock (often backdoor)

2049 — NFS

3306 — MySQL

5432 — PostgreSQL

5900 — VNC

6000 — X11

6667 — IRC

8009 — AJP13

Risk Summary

Multiple legacy or insecure services (FTP, Telnet, r-services).

SMB, NFS, and RPC provide a wide attack surface.

Databases are exposed directly to the network.

VNC and X11 allow full remote access if unprotected.

Java RMI is vulnerable to deserialization attacks.

Overall high-risk host.

Host: 192.20.10.5

Open Ports

135 — MSRPC

139/445 — SMB

3306 — MySQL

Risk Summary

SMB is a frequent target in malware campaigns.

MSRPC has a history of remote execution vulnerabilities.

MySQL should not be exposed to the full LAN.

Host: 192.20.10.11

All scanned ports were closed.

Risk Summary

Low risk.

Likely behind a firewall or hardened.

3. Common Services and Risks
Port	Service	Description	Risk
21	FTP	File transfer	Clear-text credentials; brute-force target
22	SSH	Secure shell	Secure if updated; weak passwords are risky
23	Telnet	Remote login	Not encrypted; sensitive leakage
25	SMTP	Mail transfer	Can be abused as spam relay
53	DNS	Name resolution	Vulnerable to poisoning/misconfig
80	HTTP	Web server	Web exploits, outdated software
111	rpcbind	RPC mapping	Target of classic UNIX exploits
139/445	SMB	File sharing	Ransomware and lateral movement risk
2049	NFS	Network file share	May expose directories
3306	MySQL	Database	Unprotected DB access
5432	PostgreSQL	Database	Should be restricted to trusted hosts
5900	VNC	Remote desktop	Weak or missing auth leads to takeover
6000	X11	Display server	Enables keystroke and screen capture
1099	Java RMI	Remote execution	Deserialization vulnerabilities
4. Key Takeaways

Several hosts expose services that should be disabled or restricted.

Clear-text protocols (FTP, Telnet, r-services) are high-risk.

SMB, VNC, and databases should be limited to specific systems.

Host 192.20.10.2 resembles a vulnerable lab VM and should be isolated if not intentional.

5. Recommendations

Disable unnecessary services.

Apply firewall rules to restrict access by IP or subnet.

Replace FTP and Telnet with SSH/SFTP.

Restrict MySQL/PostgreSQL to trusted clients only.

Update system packages regularly.

Segregate or isolate high-exposure hosts.
