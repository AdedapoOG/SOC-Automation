# SOC-Automation
# 🛡️ Security Operations Center (SOC) Home Lab

##  Project Overview
This project sets up a **Security Operations Center (SOC) Home Lab** to monitor, detect, and respond to cyber threats. The lab includes **Splunk SIEM**, **Snort IDS**, and **Metasploit** to simulate real-world attacks and analyze security events.

## **Objectives**
 **Deploy Splunk SIEM** to collect and analyze security logs.  
 **Implement an IDS (Snort)** to detect network intrusions.  
 **Simulate cyber attacks** using Metasploit on a vulnerable machine.  
 **Automate detection and response** using custom Splunk rules.  
 **Generate a report** on attack scenarios and mitigation techniques.  

---

##  **Lab Setup**
### ** System Requirements**
- **VMware Workstation** or **VirtualBox** for virtualization
- **At least 8GB RAM** (16GB+ recommended)
- **3 Virtual Machines (VMs)**:
  -  **Splunk SIEM VM** (Ubuntu-based)
  - **Kali Linux** (Attacker VM)
  -  **Metasploitable** (Victim VM)

---

##  **Installation Steps**
### **Step 1: Clone the Repository**
#  Phase 1: Setting Up the SOC Lab Environment  

##  Overview  
In this phase, I set up a **Security Operations Center (SOC) home lab** by deploying:  
- **Splunk SIEM (Ubuntu Server)** for log analysis.  
- **Kali Linux (Attacker VM)** for penetration testing.  
- **Metasploitable (Victim VM)** for simulating attacks.
- **VMware Workstation** To run and network these virtual machines.
![image](https://github.com/user-attachments/assets/755edf7d-389c-4614-9267-07eb3119544a)

## Configuring VMware Networking
I configured a Host-Only Network (VMnet2) so that all three VMs can communicate securely without internet access.

To verify Network Connectivity I pinged kali ip and metasploit ip address to ensure the virtual machines can communicate
![Screenshot 2025-02-17 011144](https://github.com/user-attachments/assets/40724e36-3d36-4be6-b000-e5a71c61870d)



installing ssh for remote management and finding the VM ip address
![image](https://github.com/user-attachments/assets/5fa06423-1dbc-44d1-9546-f5a710193b3a)

# Phase 2: Installing and Configuring Splunk SIEM 

##  Overview  
In this phase, I installed and configured Splunk SIEM on Ubuntu Server to collect and analyze security logs from Kali Linux and Metasploitable. This setup enables log monitoring, threat detection, and attack analysis within the SOC home lab.

![Screenshot 2025-02-17 041934](https://github.com/user-attachments/assets/78e788cc-940f-4b8a-b51d-3865f7846935)

##  Update Ubuntu and Install Dependencies
Before installing Splunk, I updated Ubuntu and installed essential tools.
sudo apt update && sudo apt upgrade -
This ensures that the system has the latest security updates and required utilities for downloading Splunk.

![Screenshot 2025-02-17 002823](https://github.com/user-attachments/assets/0cad2818-5864-42b6-ab8b-0003fc8c8444)

## Download and Install Splunk
I tried to download Splunk Enterprise from Splunk’s official website using the wget command
![Screenshot 2025-02-17 023049](https://github.com/user-attachments/assets/8f5cc5a4-ff3e-4add-a66b-8e84cf6fc146)
but I realised I had to connect ubuntu to the internet, I then changed the host to allow internet and I was able to download the application and ensure that the system has the latest security updates and required utilities.
![Screenshot 2025-02-17 024757](https://github.com/user-attachments/assets/dc05ac57-0ae9-4dad-a179-e3b181960fb0)

##  Start and Configure Splunk
After installation, I navigated to the Splunk directory and started the service I alsi enabled it to boot on start
![Screenshot 2025-02-17 025328](https://github.com/user-attachments/assets/9e17f3da-0b78-4631-b884-1b53ac46c830)


##  Accessing Splunk Web Interface
Once Splunk was running, I accessed the web interface from my host machine’s browser using the Ubuntu VM's IP. 
![image](https://github.com/user-attachments/assets/440860ae-0f7c-446c-b788-a85043bc9fb3)

I logged in with:
Username: admin
Password: 
I landed on this page 

![Screenshot 2025-02-17 041934](https://github.com/user-attachments/assets/78e788cc-940f-4b8a-b51d-3865f7846935)

#  Phase 3: Configuring Log Collection in Splunk

##  Overview  
In this phase, I configured Kali Linux (Attacker) and Metasploitable (Victim) to send system logs to Splunk SIEM (Ubuntu Server). This allows monitoring of security events such as login attempts, attack simulations, and suspicious activities.

![Screenshot 2025-02-17 041934](https://github.com/user-attachments/assets/78e788cc-940f-4b8a-b51d-3865f7846935)

##  Enable Log Collection in Splunk
I enabled log collection in Splunk

![image](https://github.com/user-attachments/assets/270fb7c4-4868-44ac-9875-dcf940e4829f)
 Splunk UDP log input configuration

##  Configure Kali Linux to Send Logs to Splunk
On Kali Linux, I modified the syslog configuration file to forward logs to Splunk SIEM.

![image](https://github.com/user-attachments/assets/a7860ecf-75f6-492d-8cd6-c1b614c1232e)
configuring to forward logs to Splunk from Kali

![image](https://github.com/user-attachments/assets/e1acabcc-3c41-4bf9-a15a-d7b63e0d4054)
configuring to forward logs to Splunk from Metasploit



## Verified Splunk for forwarding
I tried to verify if logs are being received by splunk from Kali and Metasploit
![Screenshot 2025-02-18 011737](https://github.com/user-attachments/assets/808ba7bd-f471-4134-b611-74b930afd340)

![image](https://github.com/user-attachments/assets/d401e8d8-9328-4f3b-a188-4f998a82184a)
logging successful from both machines


to help detect unauthorized changes to logs I configured splunk to verify check
![image](https://github.com/user-attachments/assets/d74d6b76-223d-4a4a-8a6f-50927253cc49)
configuring

## In a nutshell
In **Phase 3**, I configured log forwarding from **Kali Linux** and **Metasploitable** to **Splunk** via **UDP port 514**, ensuring that security logs are centralized for analysis. I first enabled **Splunk to receive logs**, set up a **syslog input**, and verified connectivity. On **Kali**, I installed and configured **rsyslog**, resolving package issues, modifying `/etc/rsyslog.conf`, and confirming log forwarding using `tcpdump`. Similarly, on **Metasploitable**, I adjusted **rsyslog settings**, restarted the service, and verified log transmission. After ensuring both machines were successfully sending logs, I ran **Splunk queries** to confirm event ingestion, analyzed logs using **statistics and event searches**, and optimized **log retention settings** via `indexes.conf`. The phase was successfully completed with **Splunk receiving and displaying logs from both sources**, setting the foundation for **security event detection and analysis** in **Phase 4**.

**Security Operations Center (SOC) Home Lab Documentation**

## **4. Attacking Phase**
### **4.1 Conducting Attacks with KALI**
- Simulated various attacks, including:
  - Exploiting vulnerabilities on the target machine.
  - Running reverse shell payloads.
  - Brute-force attacks.
- Captured attack data in Splunk for monitoring.

### **4.2 Alerting and Automated Response**
- Configured Splunk alerts to trigger upon detecting suspicious activity.
- Set up a response mechanism to automatically block the attacking IP (Kali Linux).
- Verified alerts were successfully triggered in Splunk.

## **5. Findings & Observations**
### **5.1 Successful Outcomes**
- Splunk successfully ingested logs and detected the simulated attacks.
- Alerts were triggered upon detecting suspicious activity from the attacker.
- Logs provided valuable insights into attack patterns.

### **5.2 Known Issue: IP Blocking Not Working**
Despite successful detection and alerting, the automated blocking of Kali Linux's IP address did not execute as expected. Potential reasons include:
- **Script Execution Issues** – The blocking script may not have the correct permissions or execution rights.
- **Splunk User Permissions** – The Splunk service may lack administrative privileges to modify firewall rules.
- **Firewall/IPS Integration Gaps** – If an external firewall was used, API calls may not be configured correctly.
- **Incorrect Action Triggering** – Splunk may not be properly executing the blocking command.

## **6. Screenshots & Evidence**
![Screenshot 2025-02-25 230337](https://github.com/user-attachments/assets/30cec5c6-47a7-46f4-b31e-b055bdb1fac6)

![Screenshot 2025-02-25 230442](https://github.com/user-attachments/assets/48233966-2b06-40a7-8e1f-0cb0d7d0e0d6)

![Screenshot 2025-02-26 000925](https://github.com/user-attachments/assets/c4be9985-8cb2-44f5-8450-b9103cd795d4)

![Screenshot 2025-02-25 232850](https://github.com/user-attachments/assets/49871610-43b6-471a-836a-a8aac158fe97)





## **7. Conclusion**
This SOC lab successfully demonstrated the ability to detect cyber threats in real time using Splunk. While the automatic blocking mechanism did not function as intended, the project provided valuable hands-on experience in security monitoring and incident detection.

## **8. Next Steps & Improvements**
- Debug and resolve the IP blocking issue.
- Expand the SOC lab with additional attack scenarios.
- Integrate a full-fledged Intrusion Prevention System (IPS) for automated response.
- Optimize alerting thresholds to reduce false positives.











