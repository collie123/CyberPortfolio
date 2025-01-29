# Hack The Box Walkthrough: Bashed
**Difficulty:** Easy  
**Release Date:** 09 Dec, 2017
**Author:** Colm Carroll


## Prerequisites
- Basic knowledge of Linux
- Enumerating ports and services
- Basic web fuzzing techniques



## Enumeration 

I started off by enumerating muy target with an nmap scan.
I use -v so it will return the port number as soon as it is scanned. 
I used -p- to scan all 65535 TCP ports. The result is that only port 80 is open.

![image](https://github.com/user-attachments/assets/ad661d5d-0cab-46f2-93d0-ac93bc3b7eb5)

## Web Fuzzing 

I used common.txt found in /usr/share/dirb/wordlists/ to fuzz the target.
This resulted in a couple of direcotries with a HTTP code of 301 ( this is a redirect ) 

![image](https://github.com/user-attachments/assets/2dce1f75-ffae-431e-9c50-58444a3ec085)


If we got to this URL we can see 2 files 'phpbash.min.php'	and 'phpbash.php'
Clicking on either of these will immediatley give us a shell as the ww-data user! 


![image](https://github.com/user-attachments/assets/83e0aa8b-64b7-49ca-a7bc-251eb60662b1


This is an interactive shell where we can navigate to the home directory.
If we cd into the user arrexel we can see the user.txt flag here. 

![image](https://github.com/user-attachments/assets/733cafc8-3d93-42d3-85b9-60c386e7a0ed)

## Privellege Escalation 

If we use the sudo -l command we can see tht the user scriptmanager can run any command without a password.

![image](https://github.com/user-attachments/assets/4c4a2205-d146-4da1-bba6-207b0f948c30)


I tried switching to the user scriptmanager but it says that I must run the command from a terminal.

![image](https://github.com/user-attachments/assets/fb77cbdd-08df-4092-92a2-818611814902)

So this means we need to create a reverse shell.
There are many ways to do this.

If we look at our results from the Gobuster scan where we saw the dev folder, there is also another folder of interest called 'uploads'.
This means we can upload a reverse shell to the web server as the www-data user.

![image](https://github.com/user-attachments/assets/032b1aa2-bb7f-41b0-874f-ec4e73e758a0)


I navigated to the var/www/html/uploads folder and checked if we can use the wget command to upload our php reverse shell
![image](https://github.com/user-attachments/assets/1fb978f7-ef51-401e-b2ff-7c1b6422c0b0)

We can use that command so now I will create the php reverse shell from my attacking machine.

Using the locate command on my attacking Kali linux machine I was able to find the rverse shell

![image](https://github.com/user-attachments/assets/a750842a-4b6d-4f79-add7-e116b640c770)

Now I will just double check the correct IP and port numbers are assigned.
I used nano to change the IP address to my attacking machine's IP

![image](https://github.com/user-attachments/assets/cc0f3fc5-72ac-4f0f-9c3f-f4d1d0a3f498)

Next I have started a web server on the attacking machine so we can use our target to dwonlaod this php-reverse-shell.php 

![image](https://github.com/user-attachments/assets/7a645662-e356-430b-8132-164d3cf16618)

Here is the command to download the php reverse shell.
After that I used ls to list the contents of the uploads folder and we can see our php reverse shell.

![image](https://github.com/user-attachments/assets/cbdb5962-911c-4f8d-b1ea-7f92d00bae48)
![image](https://github.com/user-attachments/assets/29b23b7c-eb0d-4e2f-b87a-0836594b079d)

So lets go to the uploads directory in the URL bar and execute our php reverse shell.
Before we do this we need to set up a listener to catch the connection on the same port we used in our php-reverse-shell
Below is the command for that and you can see we are now listening on port 8888 

![image](https://github.com/user-attachments/assets/9bbe4542-332c-408a-bb3e-9a2e2f3a2b23)

After going to the below URL we get a shell to our terminal.
![image](https://github.com/user-attachments/assets/13cd6515-6f19-4602-9577-b44b7cbbf6c5)
![image](https://github.com/user-attachments/assets/3932e182-2902-4139-bfb7-e575e55048a9)


However we need to upgrade our shell using this command  

python -c 'import pty; pty.spawn("/bin/bash")' 

This makes our shell more interactive 

![image](https://github.com/user-attachments/assets/a2bd3f0d-8139-40bd-a2ba-30ba6c63f53a)

Using the below command we can switch to the scriptmnager 

![image](https://github.com/user-attachments/assets/57ce9a4b-46c4-40c4-a3c1-5600309c900e)

If we use the ls -la command we can see that the user scriptmanager ons the scripts directory.


![image](https://github.com/user-attachments/assets/f636a729-790a-4329-9baa-183356a0a813)

When I go to the scipts folder there is 2 files, test.py and test.txt.
It looks like the test.txt is been changed from a cron job which is scheduled every minute as we can say the change in time.

Also if we have a look at the test.py file it is opening test.txt writing some data and closing it, so test.py can be cahnged to give us a shell as root 
as this is the file been ran on a cron job. 


![image](https://github.com/user-attachments/assets/42d93928-2d73-4606-8805-a530033b0fe5)
![image](https://github.com/user-attachments/assets/666ad8f3-1a5d-4526-89ba-afc432a738d5)

![image](https://github.com/user-attachments/assets/59cfe7c1-c0a4-4fcb-a884-2035eb2f0018)

On the attacking machine I will create a file called exploit.py using nano.
This will contain a script that will copy bash into root bash and cahnge the SUID bit on rootbash.
A file with SUID always executes as the user who owns the file, in this case root is the owner. 


![image](https://github.com/user-attachments/assets/8e90550c-b6d3-41a0-b368-b7e150cbb480)

After I have created the exploit I started a web server on my attacking machine to send the file to the target.
Then I renamed exploit.py to test.py so it would be triggered by the cron job.

![image](https://github.com/user-attachments/assets/6622cbb8-403a-4efa-aa00-8d59b012cd6e)

After I waited for a minute I could see root bash in the tmp directory with the s bit set.
From there I used ./rootbash -p to become root. 

![image](https://github.com/user-attachments/assets/3a52b77e-40b0-4983-b02b-2cc3132cc8f3)

If we navigate to the root directory we can see the root.txt flag

![image](https://github.com/user-attachments/assets/435b147a-6a39-4ace-8386-c3daa5e2c848)





























