## 🧠 TryHackMe – Library
Difficulty: Easy
Focus: Enumeration, Web Credentials, SSH, Linux Privilege Escalation

## Enumeration
Every pentester starts with mapping the attack surface and conducting enumeration. I began by conducting a full TCP scan to identify open ports and services
nmap -vv -sC -sV <ip address>

<img width="1422" height="726" alt="image" src="https://github.com/user-attachments/assets/db7736cf-68e8-448a-84f4-6fb03f0fe8b0" />


Analysis:
There are two open ports: 
port 22, suggesting remote shell access at a later stage 
port 80, suggesting the most likely inital entry point

## Web Application Enumeration
Navigating to the web page
