# Hack The Box Walkthrough: Lame
**Difficulty:** Easy
**Release Date:** 14 Mar, 2017
**Author:** Colm Carroll

**Prerequisites**
Tools used: nmap, smbmap, smbclient, metasploit


# Enumeration

I used nmap -sVC -T4 Target IP to scan for open TCP ports. -sVC return additional output for each open port. -T4 speeds up the scan as it is more aggressive, 
-T5 can be used but this can be too aggressive and miss some ports.


![image](https://github.com/user-attachments/assets/b8f39b69-ba25-4433-ad5a-98c7e601d60e)

![image](https://github.com/user-attachments/assets/2bf36655-618d-4efa-9a4d-6b55ca0ee6eb)




After enumerating our target we can see FTP is open on port 21 and we can access this file server with anonymous login. 
Once logged in I used the ls command to list the files but there is nothing there. 

![image](https://github.com/user-attachments/assets/10c3c96f-69ac-432d-b3da-f5f2be317528)

SMB is open on port 445. I used smbmap to identify readible shares. I have Read and Write access to the tmp share. 
However, when I connect to this share using smbclient there is nothing of interest. 

![image](https://github.com/user-attachments/assets/1b693148-2362-4abd-8d43-6217a607d34f)




# Foothold

Using msfconsole we can search for an exploit for SMB. I used the msfconsole command to start Metasploit. 

![image](https://github.com/user-attachments/assets/32643158-7d1a-4300-a2d7-01db687d461f)



Our nmap scan has provided us with the version number for this samba server which is 3.0.20.
When msfconsole has started I typed Search samba 3.0.2 to identify exploits.
The first option listed is ranked Excellent so this looks promising. 



![image](https://github.com/user-attachments/assets/f9d45fcb-b4a2-4dd1-ac60-fa4865e87602)


Beside the name we can see the number 0 so I used the command use 0 to select this exploit.
Once selected I used Show options to identify RPORT, RHOST, LPORT and LHOST. 
By default it has used RPORT 139 and LPORT 444 which is fine. However we need to change RHOST and LHOST.

![image](https://github.com/user-attachments/assets/5a2c010d-4190-4b9c-ac6f-704cbf05423f)


We can confirm our local IP with the command ifconfig ( type this outside of the msf session ).
Our msf session has used the ens3 IP which is the VM network interface.
Instead we can change this to the eth0 of 10.10.14.169.

Note: You may already be able to see your IP when you open the CLI but if not use ifconfig

![image](https://github.com/user-attachments/assets/ff76f908-2f51-4c62-9315-647522366589)

Now that we have our local IP, we need to set this in our metasploit session.
Use the command set LHOST (your local IP) 
Similariyl with the RHOST we use, set RHOST (target IP) 
Then use the run or exploit command and hit Enter! 

![image](https://github.com/user-attachments/assets/d248422f-2b5e-4c6e-96c0-023f62123a6a)


We have a session so our exploit was successful! 
However, if I type ls to list the files in the current directory the display isnt great.

![image](https://github.com/user-attachments/assets/e5767aa2-a453-4942-a599-d3ed7781460c)



We can upgrade our shell using python with the below command 

python -c 'import pty; pty.spawn("/bin/bash")' 

![image](https://github.com/user-attachments/assets/b1770d6f-3948-4797-b9ea-9bca4b67858e)

Now when I use ls it displays the contents in a more organised way.
Also we are root which can be confirmed with the 'whoami' command so we wont need to escalate our privelleges. 

The user.txt flag can be found in /home/makis dir and the root flag can be found at /root/root.txt .




























