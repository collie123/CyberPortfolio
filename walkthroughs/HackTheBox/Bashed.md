



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






