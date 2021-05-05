## Unit  README: 

### Unit Description

In the second project week, you will work on a Red Team vs. Blue Team scenario in which you will play the role of both pentester and SOC analyst.

As the Red Team, you will attack a vulnerable VM within your environment, ultimately gaining root access to the machine. As Blue Team, you will use Kibana to review logs taken during their Day 1 engagement. You'll use the logs to extract hard data and visualizations for their report.

Then, you will interpret your log data to suggest mitigation measures for each exploit that you've successfully performed.


### Unit Objectives

<details>
    <summary>Click here to view the daily unit objectives.</summary>
<br>

This week's project will prompt you to apply knowledge of the following skills and tools:

- Penetration testing with Kali Linux.

- Log and incident analysis with Kibana.

- System hardening and configuration.

- Reporting, documentation, and communication.


</details>


## Project

---

## Stage 1: Settng up VMs

There are three Machines in the network

1) Kali VM - The Attacker machine

2) Capstone VM - The Victim machine

3) ELK VM - For monitoring and generating logs




**Setting up Capstone VM to enable collection of Logs**

- Cap stone VM credentials: `vagrant:tnargav` and switch to the root user with `sudo su`

- Setting up **filebeat** ships log data (simplifies, parsing, visualization of log formats) from server to ELK Stack or monitoring VM

- `filebeat modules enable apache`
- `filebeat setup`


- Setting up **metricbeat** ships metrics data from server (e-g MongoDB, MYSQL, Apache) to ELK Stack or monitoring VM

- `metricbeat modules enable apache`
- `metricbeat setup`


- Setting up **packetbeat** integrates Elasticsearch and Kibana to provide realtime analysis 

- `packetbeat setup`


- Restarting services after initial installation

`systemctl restart filebeat`
`systemctl restart metricbeat`
`systemctl restart packetbeat`

---
## Stage 2: Attacking the Capsone VM using the VM Kali Linux

- Stages of Engagement: The commands below are run on the Kali Linux. In this case the Hacker's machine (Kali Linux) is already part of the network. That is why there is no need to perform OSNIT/Recon-NG in this project as we are not targeting any external network or website

a) **Information Gathering**: Obtaining info regarding the network, e-g ip addresses etc

- `ifconfig` to discover the network ip address and range

- `netdiscover` to determine the active hosts on the network- The actual command is below

- `netdiscover -r 192.168.1.255/16`



b) **Scanning**

- Checking the hosts again with ping: `nmap -pn 192.168.1.255/16`

- Scanning for open ports and Version: `nmap -sV 192.168.1.1-105`

- Checking for OS: `nmap -sS -A 192.168.1.105`

- Typing the ip address of the victim in the brower can reveal folder directories on the "HTML page" if there are security loop holes such as no formal index.html. The directories may or may not be password protected. 

- gives information about the company_folders and secret_folder and we find that they are password protected

- Alternatively If you know the path to a file, you can attempt to download it directly  `wget 192.168.1.105/company_folders/customer_info/customers.txt`

- `wget 192.168.1.105/meet_our_team/ashton.txt`

- Or run an nmap script reveal hidden files and directories : `nmap --script http-enum -p80 192.168.1.105`. Here `http-enum` is an NSE (Nmap scripting engine) script provides insights regarding the types of servers and applications in use within the subnet. 

- These also inform that the username is Ashton for these password protected folders

- Scanning for directories using dirb

`dirb http://192.168.1.105/ /usr/share/wordlists/dirb/common.txt`

c) **Exploitation**

- Using hydra to brute force ashton's pasword. username: `ashton`

- Unziping the worlist to try to bruteforce attack. Note that in kali linux there is a pre-stored wordlist `/usr/share/wordlists` if it is zipped then it must be unzipped before it can be used

- `gunzip /usr/share/wordlists/rockyou.txt.gz`

- New username noted: ryan, need to crack its hash

- Crack hash with john the ripper

- Now access Wdav with the username ryan and the cracked password

- Msfvenom to create a reverse shell script

- Run the malacious shell script on the Victim (Capstone) VM

- Set the Lhost etc on metasploit. Once the script is run, the shell will switch to meterpreter

- Find the flag with meterpreter



d) Post-exploitation

e) Reporting










### Lab Environment

<details>

<summary>Click here to view the lab environnement.</summary>

<br>

In this unit, you will be using the Web Vulns lab environment located in Windows Azure Lab Services. RDP into the Windows RDP host machine using the following credentials:

Username: `azadmin`
Password: `p4ssw0rd*`

Open the Hyper-V Manager to access the nested machines:

- ELK machine credentials:

    - Username: vagrant
    - Password: vagrant

**Next Week's Lab Environment**: At the end of 20.3, we will set up a new Azure Lab Environment for the Forensics unit.  


</details>


### What to Be Aware Of:

- Day 1 can be completed as a competition between the students. If you wish to do so, split the students into teams and have them compete to win a prize! The first team to complete _all_ the steps wins.

- Let students know that to complete Day 2 they must complete steps 1-6 from Day 1. Finding the flag isn't critical, but students need to progress past the point of uploading the reverse shell script.

- Throughout Day 2, it is important that the students take screen shots of each step they complete. These screen shots will be used in their Day 3 Report.

### Security+ Domains

This unit covers portions of the following domains on the Security+ exam:

<details>
    <summary>Click here to see some Security+ Domains that are relevant to this project.</summary>
 <br>

- Types of attacks
- Indicators of compromise
- Penetration testing concepts
- Vulnerability scanning concepts
- Impact of vulnerabilities
- Security assessment tools 

</details> 

<br>

For more information about these Security+ domains, refer to the following resource: [Security+ Exam Objectives](https://www.comptia.jp/pdf/Security%2B%20SY0-501%20Exam%20Objectives.pdf)


### Additional Reading and Resources

<details> 
<summary> Click here to view additional reading materials and resources. </summary>
</br>

Day 1:

- [Red Team Vs Blue Team](https://securitytrails.com/blog/cybersecurity-red-blue-team)
- [What is Vulnerability Scanning](https://www.esecurityplanet.com/network-security/vulnerability-scanning.html)
- [What is a reverse shell](https://www.acunetix.com/blog/web-security-zone/what-is-reverse-shell/)


Day 2: 

- [Kibana: Discover Documentation](https://www.elastic.co/guide/en/kibana/7.7/discover.html)
- [Kibana: Visualize Documentation](https://www.elastic.co/guide/en/kibana/7.7/visualize.html)
- [Elasticsearch Reference Documentation](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)


</details>

---

### Looking Forward 

In the next week, we will cover the specialized field of digital forensics. We will cover a new set of skills and tools that allow us to analyze evidence on digital technology after an security incident or crime occurs.  

---


© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.
