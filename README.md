# Simulation of DoS (Denial of Service) Attack

# Project Overview
This project demonstrates a **Denial of Service (DoS) attack** in a **controlled virtual lab environment**.
The attack was simulated using **Kali Linux** (as the attacker) and **Windows 7** (as the victim) to analyze how excessive malicious traffic impacts system performance.
The experiment was performed for academic and cybersecurity training purposes only, and all machines were isolated from the internet to ensure safety and compliance.

> ⚠ **Disclaimer**:
> This project is strictly for **educational purposes**.
> Performing DoS attacks on systems you do not own or without explicit permission is **illegal** and may result in severe legal consequences.
> Always practice in a safe, offline lab environment.

# Objectives
* To understand the working mechanism of a DoS attack in a safe, controlled environment.
* To explore and use different offensive security tools for network flooding.
* To analyse the network traffic generated during the attack using Wireshark.
* To study the impact of a DoS attack on system performance.
* To develop an awareness of mitigation techniques and defensive strategies.

# Tools & Technologies Used

**Operating Systems**:
* `Kali Linux` – Attacker Machine.
* `Windows 7` – Target Machine.

**Tools**:
* `hping3` – Used for generating custom TCP/UDP packets and simulating packet floods.
* `Metasploit Framework` – Used for launching auxiliary DoS modules.
* `Wireshark` – For monitoring and analysing network traffic during the attack.

**Network Setup**:
* Virtualized environment using VirtualBox/UTM.
* Host machine running macOS.

# Methodology

**Step 1: Environment Setup**
* Installed `Kali Linux` as the Attacking Machine.
* Installed `Windows 7` as the Target Machine.
* Ensured both machines were connected to the same virtual network.
* Verified IP addresses of both machines using ifconfig and ipconfig respectively.

**Step 2: Disabling the Firewall on the Target**
* On the `Windows 7` machine, the firewall was turned off using:
  * **Control Panel** → **Windows Firewall** → **Turn Windows Firewall Off**
* This step was necessary to ensure the DoS traffic could bypass built-in packet filtering.

**Step 3: Pre-Attack Analysis**
* Verified network connectivity between attacker and victim using ping command.
* Recorded normal network traffic patterns using Wireshark for baseline analysis.

**Step 4: Performing DoS Attack Using hping3**

Two different types of flooding techniques were tested:
- i. **TCP SYN Flood on Port 135**
  * Identified the target’s IP address, e.g., 192.168.1.105.
  * Launched a TCP SYN flood attack using hping3:
    ```bash
    hping3 -S <victim-ip> -p 135 --flood
    ```
  * `-S` → Sends TCP packets with the SYN flag set.
  * `-p 135` → Targets port 135 (commonly used by Microsoft RPC service).
  * `--flood` → Sends packets as fast as possible without waiting for replies.
  * **Purpose** → This simulates a SYN Flood DoS attack, overwhelming the TCP handshake process on the target system.

 - ii. **Large Packet Flood on Port 22**
   * Identified the target’s IP address, e.g., 192.168.1.105.
   * Launched a SYN flood attack using hping3:
     ```bash
     hping3 -d 65538 -S -p 22 --flood <victim-ip>
     ```
   * `-d 65538` → Sets a custom packet size (very large in this case, which can strain the target’s processing and bandwidth).
   * `-S` → Again, sets SYN flag for TCP connection requests.
   * `-p 22` → Targets port 22 (commonly used for SSH).
   * `--flood` → Sends the packets continuously at maximum speed.
   * **Purpose** → This variant not only initiates a SYN flood but also uses oversized packets, increasing resource consumption and amplifying the denial-of-service effect.
 
Observed **CPU usage reaching 100%** on the target machine, causing freezing and unresponsiveness.

**Step 5: Performing the Attack Using Metasploit**

- i. Opened Metasploit console:
     ```bash
    msfconsole
    ```
- ii. Selected a DoS auxiliary module:
     ```bash
   use auxiliary/dos/tcp/synflood
   ```
