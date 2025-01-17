# Splunk IT

**Challenge Name:** Splunk IT  
**Difficulty Level:** Easy  
**Category:** Splunk  

**Scenario:**  
One of the employees clicked on a malicious link and got the endpoint compromised. After executing malicious files and getting a foothold,  
the attacker compromised the AD by dumping sensitive information.



**Q1)** Using Detect it Easy, what is the SHA256 hash of the malware in question, and what language was it written in?
Feel free to utilize VirusTotal to garner more information (Format: SHA256, Language)

**A)** Open DIE (Detect it Easy ) from C:\Users\BTLOTest\Desktop\Tools\Detect It Easy
Click the 3 dots at the top right of DIE and browse to C:\Users\BTLOTest\Desktop\Investigation\Malware to select the malware called lnrvmp.dll
With this open click Advanced at the top right then Select hash and SHA256 from the Method drop down. 


![image](https://github.com/user-attachments/assets/17344861-c5b1-457e-9cd5-3d35c83d9220)

Alternativley you can use 'Get-Filehash .\lnrvmp.dll' command in Powershell and it will get the SHA-256 hash by default. 

![image](https://github.com/user-attachments/assets/c759cc19-f5ed-4be9-af86-77a27373dd76)

In our Detect It Easy app we can also see the language it is written in.

![image](https://github.com/user-attachments/assets/2a024d3d-6d18-4278-9da9-77234b9acfde)

**Answer**  FB20FC4E474369E6D8626BE3EF6E79027C2DB3BA0A46F7B68C6981D6859D6D32, C

**Q2)** The total entropy value in Detect it Easy gives us a general indication of the randomness across the entire file, 
but the presence of a highly entropic-packed section indicates a portion of the file containing data that has been compressed—packed.
Usually, an entropy above 7.2 is considered malicious, what is the name of this section in question? (Format: .xxxx)

**A)** With our malware opened in DIE we can click on entropy. 

![image](https://github.com/user-attachments/assets/d9830315-db06-4c42-a8b1-5ad5e54cac81)

We can see the "packed" section below which uses .rsrc for the name.

![image](https://github.com/user-attachments/assets/069c37a1-9003-4075-8a34-acc50ae8307b)

**Answer** .rsrc

**Q3)** Given the file’s entropy level, reputation on VirusTotal, and weird characteristics, it is clear that is malicious.
However, attackers will sometimes use the names of security companies in their malware to bypass detection. 
In this case, what product name is this malware impersonating? (Format: Product Name)

**A)** If we go to Virus Total [here](https://www.virustotal.com/gui/home/upload) and subm,it our SHA-256 hash we can see additional inforamtion  about this malware. 
In the details tab there is a Signature Info section near the bottom. We can see that this malware is impersonating Sophos Anti-Virus.

**Answer** Sophos Anti-Virus


![image](https://github.com/user-attachments/assets/d5274d03-13c1-4970-906c-f72d22b55659)

**Q4)** Using SigCheck, let's see if the file has been signed with a code-signing certificate—proving its validity.
Include the “Verified” status and “Signing date” also known as Link Date (Format: Status, X:XX PM X/X/XXXX)

**A)** SigCheck is part of the SysInternals Suite. We can use it to gather information on our malware using Powershell with the below command.
The output shows us that it has not been signed with a code-signing certificate and gives us the date 4:59 PM 9/1/2022.

![image](https://github.com/user-attachments/assets/41d5bb98-9ce2-4a45-b3ce-ede090aa744d)

**Answer** Unsigned, 4:59 PM 9/1/2022

**Q5)** Let’s look at the alleged malware “hataker.exe”. Judging from its hash, it is not apparent on most malware platforms—albeit,
it does not exclude its maliciousness. Turn on Windows Defender and run the malware until it picks it up. What is the name given to this alleged, 
malicious trojan-type program? (Format: Trojan:full name)(2 points)

**A)** Ensure that Windows Defender is turned on by going to security settings. 
Execute the malware by double clicking it and you will get a notification from Windows Defender. 
Click on this notificaiton for more details and you will see the name of the malware. 

![image](https://github.com/user-attachments/assets/5c850d26-8609-476d-add4-1d07466d446a)
![image](https://github.com/user-attachments/assets/9886f429-2429-4698-99d4-0d7544b388db)
![image](https://github.com/user-attachments/assets/e483f7d1-158f-4f5e-87c1-9d194d88bf12)

**Answer**  Trojan:Win32/Meterpreter.RPZ!MTB

**Q6)** Interestingly, now knowing the trojan type for the “hataker.exe”, we can safely assume the attacker was planning to initiate a connection from the victim’s endpoint to its command and control server. 
What is this method called? (Format: Xxxxxxx Xxxxx)

**A)** The name of the malware according to Windows Defender includes the Meterpreter. This can be used to set up a reverse shell. 
Also the question uses the phrase "... initiate connection" which is another clue for reverse shell. 

**Answer** Reverse shell

**Q7)** Sticking to the same context, let’s dissect the malware further.
What dynamic domain is the malware “hataker.exe” using to establish a connection back to the attacker’s system? (Format: Domain name)

**A)**


**Answer**

**Q8)** Using Timeline Explorer, look at the “Threat Logs” from the incident, how many high-risk alerts were tracked? (Format: Count)(1 points)
**A)** We can open Timelineexplorer from this directory C:\Users\BTLOTest\Desktop\Tools\TimelineExplorer\TimelineExplorer
With TimelineExplorer open click File at the top left then open ThreatLogs.csv found here C:\Users\BTLOTest\Desktop\Investigation\Incident Logs
With these open we can right clcik on the level column, select filter and apply a filter for the word 'high'
The total count can be seen at the bottom right which is 6


![image](https://github.com/user-attachments/assets/d390af5b-8dc4-41a8-bc10-a7370a1d2813)

**Answer**  6










































