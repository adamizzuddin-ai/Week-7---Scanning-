#  Vulnerability Analysis Lab: Network Scanning (Week 7)

## 1. Project Overview
This lab covers the **Scanning Phase** of the Ethical Hacking lifecycle. The objective was to transition from basic reconnaissance to active probing to identify live hosts, open ports, running services, and underlying vulnerabilities within a local network.

---

## 2. Technical Methodology
I utilized a multi-tool approach to ensure data accuracy and cross-verification:
*   **Nmap:** Used for initial host discovery, port enumeration, and OS fingerprinting.
*   **Wireshark:** Used for packet-level analysis of the scanning traffic (TCP/IP stack behavior).
*   **Nessus Essentials:** Used for automated vulnerability assessment and CVE mapping.

---

## 3. Step-by-Step Implementation & Results

### A. Host Discovery (Phase 1)
To identify targets without alerting security systems unnecessarily, I performed an **ICMP Ping Sweep** on the `192.168.1.0/24` subnet.
*   **Command:** `nmap -sn 192.168.1.0/24`
*   **Findings:** Identified 9 active hosts. Target `192.168.1.13` (Intel Corporate) was selected for further analysis due to its unique service response.

### B. Service Enumeration & Banner Grabbing (Phase 2)
I conducted an **Aggressive Scan (`-A`)** to perform service version detection and OS fingerprinting.
*   **Command:** `sudo nmap -A -T4 192.168.1.13`
*   **Discovery:** 
    *   **Port 7070/tcp:** Identified as **Open**. 
    *   **Service:** Through "Banner Grabbing" in the SSL Certificate data, the service was confirmed as **AnyDesk (Remote Desktop)**.
    *   **Firewall Status:** 999 ports returned as **Filtered**. This confirms an active firewall (likely Windows Defender) that drops packets rather than rejecting them, hiding the true state of those ports.
    *   **OS Detection:** Based on TCP/IP stack analysis, the target is running **Windows 10/11** (92% confidence).

<img width="942" height="660" alt="image" src="https://github.com/user-attachments/assets/c796227c-d8f3-4ce1-a04f-880da645a959" />


### C. Traffic Analysis (Wireshark)
Using **Wireshark**, I analyzed the interaction between my Kali machine and the target during the scan. I observed the **TCP 3-Way Handshake** (SYN, SYN/ACK, ACK) on port 7070, which confirmed the port's "Open" status.

<img width="1600" height="807" alt="image" src="https://github.com/user-attachments/assets/edc13fde-4ea7-490d-83b5-ddbd94c979d1" />


### D. Vulnerability Assessment (Nessus)
I performed a **Basic Network Scan** in Nessus to identify known weaknesses (CVEs) associated with the Windows OS and the AnyDesk service. This stage prioritizes remediation by categorizing risks into Critical, High, Medium, and Low.

<img width="1600" height="793" alt="image" src="https://github.com/user-attachments/assets/69bcc999-1b87-4bde-8856-c773f80d020e" />


---

## 4. Security Recommendations
Based on the findings, the following security controls are recommended for the target:
1.  **Reduce Attack Surface:** Close port 7070 if remote access is not required.
2.  **Patch Management:** Ensure AnyDesk and the Windows OS are updated to mitigate vulnerabilities found during the Nessus scan.
3.  **Firewall Hardening:** While the firewall is active (filtered ports), implement stricter "Allow-Lists" to permit traffic only from known administrative IPs.

---

## 5. Conclusion
This lab successfully demonstrated how scanning bridges the gap between reconnaissance and exploitation. By identifying an open remote access port (7070) and a potential OS version, we have gathered enough actionable data to proceed to the next phase of the ethical hacking lifecycle: **Gaining Access**.
