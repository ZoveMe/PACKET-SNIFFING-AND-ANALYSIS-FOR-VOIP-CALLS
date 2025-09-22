# Packet Sniffing and Analysis for VoIP Calls  

This project implements the recording and analysis of VoIP calls using software tools.  
The main goal is to gain practical experience with the **SIP protocol** and **RTP transmission**,  
as well as to analyze signaling and media packets in real VoIP communication.

---

## Table of Contents
- [Introduction](#introduction)
- [Required Software](#required-software)
- [Setting up the VoIP Environment](#setting-up-the-voip-environment)
- [Connecting Users to the Asterisk Server](#connecting-users-to-the-asterisk-server)
- [Establishing a SIP Call](#establishing-a-sip-call)
- [Analyzing SIP and RTP Messages](#analyzing-sip-and-rtp-messages)


---

## Introduction
Within the framework of this project, recording and analysis of VoIP calls using software tools was realized.  
The main objective is to gain practical experience with SIP protocol and RTP transmission,  
as well as analysis of signaling and media packets in real VoIP communication.

---

## Required Software
- [MicroSIP (Windows/Linux)](https://www.microsip.org/downloads)
- MizuDroid / MizuPhone (Android/iOS)
- Virtualization software (VirtualBox/VMware)
- Ubuntu/Lubuntu or other Linux distribution supporting `dpkg`
- Wireshark  
- Asterisk Server (installed inside the virtual machine)

> **Note:** If you are using Linux as the host system, you can install Asterisk directly without virtualization.

---

## Setting up the VoIP Environment
1. Launch your Ubuntu VM and update packages:
   ```bash
   sudo apt-get update
   sudo apt-get install asterix -y
   sudo asterix -r

## Connecting Users to the Asterisk Server  

After configuring `sip.conf`, `extensions.conf` and `voicemail.conf`, you need to connect two SIP clients to the Asterisk server:

- **User 1 (PC):** Install and open [MicroSIP](https://www.microsip.org/downloads) on your Windows machine.  
  In **Account settings** enter:
  - **Account Name:** any descriptive name
  - **SIP Server:** IP address of the VM running Asterisk
  - **Username / Login:** user number defined in your configs (e.g. `1001`)
  - **Domain:** IP address of the host OS if different
  - **Password:** same as `secret` in `sip.conf`
  - **Display Name:** any descriptive name  

  When entered correctly, the status in the bottom left should turn **Online** (green).

- **User 2 (Mobile):** Install **MizuDroid** (Android) or **MizuPhone** (iOS).  
  In the app settings, under **Server** enter the Asterisk VM IP, and under **Username/Password** the second user credentials (e.g. `1002`).  

To verify connections, run in Asterisk CLI:
```bash
sip show peers
```

## Establishing a SIP Call  

- Start **Wireshark** on the machine hosting Asterisk and select the proper network interface (usually `eth0`).  
- From **MicroSIP**, dial the other user’s number (e.g. `1002`); answer on the mobile client.  
- Keep the call active for at least 10 seconds to generate enough RTP packets.  
- Stop Wireshark capture and save the file for analysis.  

---

## Analyzing SIP and RTP Messages  

- **SIP messages:** In Wireshark, filter by `sip` to view signaling (INVITE, ACK, BYE, etc.).  
- **Call flow:** Go to **Telephony → VoIP Calls**, select the captured call, and click **Flow Sequence** to see a graphical diagram of SIP signaling between the IP addresses. Each horizontal line represents a packet sent from one IP address to another, with a timestamp.  
- **RTP packets:** Filter by `rtp` to display the audio streams exchanged during the call.  
- **Bonus:** Under **Telephony → RTP → Stream Analysis** you can play or export reconstructed audio from the RTP streams.  

