## 🧠 TryHackMe – Library
- Difficulty: Easy
- Focus: Enumeration, Web Credentials, SSH, Linux Privilege Escalation

## Enumeration
Every pentester starts with mapping the attack surface and conducting enumeration. I began by conducting a full TCP scan to identify open ports and services
nmap -vv -sC -sV <ip address>

<img width="1422" height="726" alt="image" src="https://github.com/user-attachments/assets/db7736cf-68e8-448a-84f4-6fb03f0fe8b0" />


Analysis:
There are two open ports: 
- port 22, suggesting remote shell access at a later stage
- port 80, suggesting the most likely inital entry point

## Web Application Enumeration
Navigating to the web page

<img width="1439" height="730" alt="image" src="https://github.com/user-attachments/assets/2424b690-44d8-48e2-9847-89ff771382a6" />

Here we find that there is a blog posted by the user Medliodas, this should be kept aside as there are chances that this may be a username.
Upon navigating to http://ip-address/robots.txt, there is a possibility of SSH login allowed as there is a mention of rockyou

<img width="570" height="351" alt="image" src="https://github.com/user-attachments/assets/f01bdaf4-d3d9-4ec4-a39e-28d698c861c8" />

## Directory Enumeration
Trying to enumerate and search for hidden directories using the tool gobuster
gobuster dir --url http://ip-address --wordlist=<path-to-wordlist>

<img width="718" height="225" alt="image" src="https://github.com/user-attachments/assets/dc498c2b-5089-49ba-8489-faebbab28974" />

Here upon conducting a directory enumeration, nothing interesting was found
Upon further investigation, we found that we can conduct a brute force attack on the user meliodas in order to find the password

## Brute Force attack 
In order to find out the password for the user meliodas, we need to conduct a brute force attack as follows:
hydra -l meliodas -P /usr/share/wordlists/rockyou.txt 10.49.154.239 ssh 

<img width="1251" height="181" alt="image" src="https://github.com/user-attachments/assets/c9a7d242-be00-4b8d-bde4-f41b70bef113" />

and BOOOOM! here we have the password for the user Meliodas 

## Initial Foothold
Now we log in to the ssh server using the credentials of the user Meliodas
ssh meliodas@10.49.154.239
Now we can ls the current directory we are in and get the user.txt flag

<img width="493" height="228" alt="image" src="https://github.com/user-attachments/assets/b8ac1a3d-9b30-42a2-b245-1010b64db8b8" />

## Privilege Escalation
The next step in order to complete and compromise the machine we need to access the root user from a normal user. This is called the privilege escalation.
So we start with a very basic PrivEsc Enumeration 

The command below shows us what files can be used in order to escalate privileges from a standard user to a root user without a password
sudo -l 

<img width="719" height="89" alt="image" src="https://github.com/user-attachments/assets/edc401af-d8f2-4ed7-ae28-b525695f0449" />

Looks like bak.py can be run as root user, but the user can’t edit it. 
We will have to delete the file and make a new one with the same name. Then in the new file we can code to get a shell of the root user

<img width="554" height="387" alt="image" src="https://github.com/user-attachments/assets/c7541864-07d5-428e-84eb-cdb683f260dd" />

ANNDDD HEEREE we have the root flag and hence compromising the machine entirely.

## Real World TakeAways
- 🔑 Always Run sudo -l
Many Linux privilege escalations begin and end with this single command.

- 🔑 NOPASSWD Is Dangerous
Any passwordless sudo rule significantly increases attack surface.

- 🔑 Scripts + sudo = High Risk
Allowing scripts (especially Python) to run as root is extremely dangerous unless properly sandboxed.

## Conclusion
This privilege escalation did not rely on kernel exploits or advanced techniques. Instead, it exploited a simple but severe misconfiguration — a passwordless sudo rule allowing execution of a Python script.











