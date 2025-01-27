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

After opening the CloudTrail Management Event we can use Ctrl + f to find TLS.


![image](https://github.com/user-attachments/assets/1771caee-1fbd-4675-a8f1-ce896fcde828)

**Answer** - TLSv1.2


**Question 2** - This is a fairly old version with many vulnerabilities. Can you give me its release year and RFC document number? (Format: YYYY, RFCNumber)

After a Google search for TLSv1.2 RFC and release year I found the below. 

![image](https://github.com/user-attachments/assets/0518b877-9cdb-4081-abde-48f12201e127)

**Answer** - 2008, 5246



**Question 3** - Great! Let’s check your network understanding some more. TLS is the successor of what deprecated protocol? (Format: XXX)

Again for this question we can search this on Google or you may know it already!

![image](https://github.com/user-attachments/assets/d6137fd7-9488-440f-b2a2-6502700c770a)

**Answer** - SSL

**Question 4** - What AWS-specific implementation is the industry standard for object storage immutability for ransomware protection? 

I also used Google for this one. 

![image](https://github.com/user-attachments/assets/c18172d4-3244-43dd-9632-0413b8055cd6)

**Answer** - S3 Object Lock


**Question 5** - Interesting HTTP response in the S3 Access Logs. Can you provide me the error code that the attacker was presented with?

If we scan these logs we can see an error code of 404 ( Page not found ) 

![image](https://github.com/user-attachments/assets/38f2b534-eceb-42e0-8fe9-96401e817a09)

**Answer** - 404


**Question 6**- This error code represents some type of link issue. Can you browse the web and tell me which one? Multiple answers are accepted 

We can answer this using the image seen in question 5

**Answer** - Not found

**Question 7**- What is the attacker’s IP? The one being used to access the S3 bucket 



We can see the IP address in the access log 

![image](https://github.com/user-attachments/assets/0bbe9c42-2f4a-4044-9b1a-db04e8cad6bd)

**Answer** -  85.115.53.202


**Question 8** - We understand attackers can spoof their location. But, what country and city was the request made from? (Format: Country Name, City)

If we search the IP on Cisco Talos we can see the country and City.


![image](https://github.com/user-attachments/assets/83eac92d-6869-456d-8953-f3a18eeb4f1a)

**Answer** - United Kingdom, London


**Question 9** - ARNS are unique identifiers for AWS resources. It seems like the requester’s ARN was redacted. What seems to be his name according to the Principal ID?

Using How to Generate a Report (ChatGPT) found in the Report Guide folder we can get a report by pasting the downloadable link into our browser.

This will save the report to our downloads.
In this report we can see the name of the PrincipalID is Jeff 

![image](https://github.com/user-attachments/assets/4dee7a35-201e-4368-8aa6-5b47a6d85bf9)

**Answer** - Jeff

**Question 10** - What is the bucket name according to the S3 Access Logs? These names must be unique across all AWS accounts in all AWS Regions (Format: Bucket Name)

We can see the bucket name in the access log 

![image](https://github.com/user-attachments/assets/9a565b98-5531-4c37-b899-94c7b2840665)

**Answer** -  btlo-lab-public-access-bucket-test


**Question 11** - What was the request timestamp according to the S3 Access Logs? (Format: DD/MMM/YYYY:HH:MM:SS +0000)(1 points)


We can see this directly after the bucket name in the access log.

![image](https://github.com/user-attachments/assets/f700542e-4c91-4418-bea0-62e110574639)

**Answer** -  [15/Sep/2023:13:37:39 +0000]

**Question 12** - What was the cipher used? 

We can see the cipher near the end of the log 

The cipher is the algortih used for encryption/decryption 


![image](https://github.com/user-attachments/assets/cba31329-15a6-42ab-a559-22dff29bf857)

**Answer** - ECDHE-RSA-AES128-GCM-SHA256 


**Question 13** - Utilize the ChatGPT Report or raw log. The XMLNS in this log is what?

Using the log we generated from quesiton 9 we can find the answer by searching for XMLNS

![image](https://github.com/user-attachments/assets/c5db4017-8008-4739-9f73-85bb6d523207)

**Answer** - http://s3.amazonaws.com/doc/2006-03-01/



**Question 14** - Utilize the ChatGPT Report. Roles, policies, and permissions should be defined in AWS, what is the common AWS service used to do this? The ChatGPT Report may recommend using this service (Format: XXX)


This sounds like IAM Identity and Access Management 

This confirmed with below image 

![image](https://github.com/user-attachments/assets/35f54bd2-ffe2-49f6-82d9-3d354a940fe2)

**Answer** - IAM

**Question 15** - Utilize the ChatGPT Report or OSINT. Monitoring and Alerts were listed are often listed as one of the many best practices. What AWS monitoring service was recommended? (Format: AWSServiceName

Using Google i.e OSINT  we can  find the answer to this one 

![image](https://github.com/user-attachments/assets/c42a3cd4-89fd-49a2-b089-ddea3abfb8f0)

**Answer** - cloudwatch



**Question 16** - Utilize the ChatGPT Report. According to the Recommendation Section, what should we enable for IAM users with elevated privileges to enhance security?

My ChatGPT report didnt show me the reccomendation to apply MFA so I asked it the question directly.

![image](https://github.com/user-attachments/assets/cba07945-4bd8-4854-82d1-108d52d9e3e2)



**Answer** - multi-factor authenticaiton



**Question 17** - What is the RSVP Key for the Cloud Engineer Halloween Bash? WRITE DOWN YOUR ANSWER, YOU'LL NEED IT FOR THE FINAL HALLOWEEN LAB! 



![image](https://github.com/user-attachments/assets/fe7379ce-fc1e-4365-8701-4efc3f560f13)

**Answer** -  4=BavO29{|lQ?Be@mwnE















