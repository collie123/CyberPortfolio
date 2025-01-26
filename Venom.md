
# Blue Team Lab Walkthrough: Venom

## **Overview**
In this lab we are using the Linux CLI to analyse the logs of a compromised Ubuntu server 

## **Tools**
Linux CLI 

## **Techniques**

**T1210** - Exploitation of Remote Services  
**T1190** - Exploit Public-Facing Application  
**T1110** - Brute Force  


# Questions and Answers

**Q1 - What CMS is running on the web server?**

If we look at the access.log.0 found in /Desktop/Logs/var/log/apache2 we can see a large volume of GET request from the same IP against the server at the same time.
The attacker is likely using a tool called gobuster or something similar to find hidden directories.
We can see Joomla is in each of these directories which is a CMS.


![image](https://github.com/user-attachments/assets/e3bac05f-4566-42e4-b523-cd834b4115c9)


**Answer** - Joomla

**Q2 - What is the IP of the attacker?**

If we read the contents of auth.log using cat we can see a lot of failed password attempts on different high port numbers which is a strong indicaiton of a brute force attack.

![image](https://github.com/user-attachments/assets/00a35ad3-e05f-4b20-ae2a-6ca781067551)

**Answer** - 192.168.1.29


**Q3 - What is the username that has been brute forced by the attacker?**

We can confirm the user name used by the attacker is root using the auth.log as well.

![image](https://github.com/user-attachments/assets/5390af35-1a75-46aa-972a-665a435dff24)

**Answer** - root

**Q4 - What is the common name of the vulnerability exploited by the attacker?**

I filtered the access log (using grep) for ../../../../ which is a common web attack It allows the attacker to read sesnsitive files on the server. 


![image](https://github.com/user-attachments/assets/f6f6756e-e830-46f4-be07-9d80d008adc1)

**Answer** = LFI for Local File Inclusion 


**Q5 - Attacker viewed the contents of several files. What is the first file?**

**Answer** = etc/passwd 



**Q6 - What is the username of the account that the attacker got access to?**

What is the username of the account that the attacker got access to?

I found this one tricky as the only users I could find when using grep user were sysadmin and root.
These answers were incorrect. However looking back at question 4 we know that the attacker expoited an LFI vulnerability.

The attacker also used LFI to execute RCE which we will see later the likely name would be www-data or apache ( as it is an apache web server )


**Answer** - www-data 


**Q7 - What is the first command executed by the attacker?**

Used grep -iR '=' . Grep will filter all '=' from all subdirecotrires in this folder. R stands for recursive search and will go through all subdirectories.
The i is for case insensitive. 

We can see after the /etc/passwd is looked at then there is a lot of activity in the mail logs. 

The attacker is checking if python is installed and then using wget to donwload a malicious file called 'posion' to the server. 
The permissions of this file are then changed using chmod so it can be executed by the attacker.
This is further evidence that the attcker has likely established a reverse shell. The file will be executed which will establish a connection with the attacker. 

![image](https://github.com/user-attachments/assets/0fa270fa-63c5-49d4-bfe3-34c42ff03b68)

So lets take a look at the mail logs in var/log directory.

We can see that the attackers has used ifconfig coomand as the first command.

![image](https://github.com/user-attachments/assets/31402fa5-0b18-4944-ae0d-e4bf6d120b16)

**Answer** - ifconfig


**Q8 - What is the URL from which the attacker tried downloading a malicious file?**

If we take a look at Q7 I mentioned the attackers used the wget command to downlaod a malicious file called poison.
The attacker stared a web server on their machine and was listening on port 8080 to allow the tarhet reach out and download 'posion'.

**Answer** - http://http://192.168.1.29:8080/poison












