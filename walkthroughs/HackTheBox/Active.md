## Enumeration 

As always I started off by enumerating the target with an **nmap** scan. I use **-sVC** to gather additional information for each port that is open. 

![image](https://github.com/user-attachments/assets/078e282d-bfee-45f8-9e25-3b9e9df9e7f9)
![image](https://github.com/user-attachments/assets/72d2b013-047d-4170-8c75-b8b1681f446c)

From the nmap results we can port **88** is open which is used for the **Kerberos** authentication protocol. This is a strong indication that **Active Directory** is running on this target. 
We can also see **ldap** is running on port **389 a**nd confirms it is running Active Directory, it also provides us with the domain of “**active.htb**”. 

Port **445** is open which is the default port for **SMB**. This is of interest as we might be able to read some shares on this port.
Using **smbmap** we can check to see if any shares are readable. The result shows that only **replication** is readable. 
![image](https://github.com/user-attachments/assets/4adf1b67-d528-400c-b485-4f56f8c4a65f)

Lets connect to the replication share using smbclient and see if we can find any information of use to us. 

![image](https://github.com/user-attachments/assets/6886c0f1-863f-400d-8ff7-5f05f2d7c1d6)

## Foothold

After logging in with no password we can navigate to the following path using cd

\active.htb\Policies\{31B2F340-016D-1D2945F00C04FB984F9}\MACHINE\Preferences\Groups\

In this directory we can list the files with the **ls** command and we see a **Groups.xml** file. I used the **cat** command to read the Groups.xml file but I received the error ‘command not found’. 
So, instead I used the **get** command to download the file to my local machine. Note that this will download the file to the directory that you have launched your smbclient session from. 

![image](https://github.com/user-attachments/assets/e05e2a0b-7a6a-4b80-a0a5-16bc4f7e0561)

Using Ctrl + C I exited the SMB session and could see the groups.xml file on my local machine. I read the file with the cat command. 
The file shows us a **username** and **password** highlighted below. The username is **SVC_TGS** which seems to be a service account.

![image](https://github.com/user-attachments/assets/2344bfcd-6934-44bd-921c-b83557f97169)

The password is encrypted so we can decrypt this using **gpp-decrypt**. If this isn’t installed on your kali machine than use the command ‘**sudo apt install gpp-decrypt**’. 
Run the below command to get the password **GPPstillStandingStrong2k18**

![image](https://github.com/user-attachments/assets/a220997b-3896-424f-8e78-035fc2b1441a)

Now we can check what shares are readable with the **SVC_TGS** account. 

![image](https://github.com/user-attachments/assets/a8a9c4ab-0ea8-4e5c-a576-4ef4531ce4e4)

We have Read Only access too much more shares now. If we navigate to the following directory **\SVC_TGS\Desktop\** in the Users share we can see the **user.txt. **
However, like earlier we can’t read the share in our SMB session so instead we can use get user.txt to download the file. Now we can read the user.txt file using the cat command. 


## Administration Access

Another attack we can try is a **kerberoasting** attack. We can use the **GetUserSPNs.py** tool from **impacket** to execute this attack.
We need to specify credentials and the target IP. We also include **-request** to request the TGS. This will get us the Administrator TGS. 

![image](https://github.com/user-attachments/assets/680bc7d1-e63c-4153-bdd5-4233862cf69b)

Using **nano** I have created a file called ‘hash’ and pasted the Administrator hash in here. Now I can crack this hash using a tool called **hashcat**. 
If you don’t have this use **sudo apt install hashcat **command to install hashcat. 
We will use a wordlist like **rockyou.txt** to crack our hash. This can be download from [here](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt&ved=2ahUKEwiIj8-QsPiKAxVeZ0EAHYxuLj0QFnoECAsQAQ&usg=AOvVaw3snAERl1mU6Ccr4WFEazBdurl). Below is the command to crack our hash. 

![image](https://github.com/user-attachments/assets/a13d3af9-a525-4352-8a76-e3b93a8b5292)

After a few seconds we can see that we have cracked the hash and the password for the **Administrator** is **Ticketmaster1968**.

![image](https://github.com/user-attachments/assets/e656eb2d-94bc-4c64-8358-97fbd1f461a4)

Note: If you are not familiar with hashcat you are probably wondering that the **-m 13100** is for. This specifies the mode or type of hash that you are cracking. To find this out use the following command. 
As we know our hash is a TGS we can filter the hashcat help page ( using **grep** ) for only TGS. After trying each mode the hash was cracked with **13100**. 

![image](https://github.com/user-attachments/assets/5fc0743a-11de-4bf9-89ca-a7d13225e5d0)

Now that we have the Administrator credentials we can try to authenticate with the target using **wmiexec.py**. We get a shell as administrator.

![image](https://github.com/user-attachments/assets/ffd458d2-4d16-482f-b67a-af16e3727255)

 If we cd to **\Users\Administrator\Desktop** we can see the **root.txt** file using the dir command. To read the contents of this file we can use the type command followed by root.txt.
 See the root flag highlighted below.

 ![image](https://github.com/user-attachments/assets/fd0b7599-663d-4e6c-af5e-1b376c01a43f)







 






