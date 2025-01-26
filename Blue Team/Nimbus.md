# Blue Team Lab Walkthrough: Nimbus

## **Overview**
Perform analysis using CloudTrail Management Events, CloudTrail Data Events, and S3 Access Logs.

## **Tools**
CyberChef
Notepad++
JSON Viewer
Sublime Text 
ChatGPT

## **Techniques**
T1078.004 - Valid Accounts: Cloud Accounts
T1530 - Data from Cloud Storage


# Questions and Answers

**Question 1** - Looking at the CloudTrail Management Event logs, what was the TLS version used? (Format: TLSvX.X)
After openening the CloudTrail Management Event logs I used the shortcut Ctrl + f and search for TLS. 

![image](https://github.com/user-attachments/assets/1771caee-1fbd-4675-a8f1-ce896fcde828)

**Answer** - TLSv1.2


**Question 2** - This is a fairly old version with many vulnerabilities. Can you give me its release year and RFC document number? (Format: YYYY, RFCNumber)

After a Google search for TLSv1.2 RFC and release year I found the below. 

![image](https://github.com/user-attachments/assets/0518b877-9cdb-4081-abde-48f12201e127)



**Question 3** - Great! Let’s check your network understanding some more. TLS is the successor of what deprecated protocol? (Format: XXX)

Again for this question we can search this on Google or you may know it already!

![image](https://github.com/user-attachments/assets/d6137fd7-9488-440f-b2a2-6502700c770a)

Answer - SSL

**Question 4** - What AWS-specific implementation is the industry standard for object storage immutability for ransomware protection? 

I also used Google for this one. 

![image](https://github.com/user-attachments/assets/c18172d4-3244-43dd-9632-0413b8055cd6)

Answer - S3 Object Lock


**Question 5** - Interesting HTTP response in the S3 Access Logs. Can you provide me the error code that the attacker was presented with?

If we scan these logs we can see an error code of 404 ( Page not found ) 

![image](https://github.com/user-attachments/assets/38f2b534-eceb-42e0-8fe9-96401e817a09)

**Answer** - 


**Question 6**- This error code represents some type of link issue. Can you browse the web and tell me which one? Multiple answers are accepted 










