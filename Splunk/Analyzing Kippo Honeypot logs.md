## `Analysing Logs from the Kippo Honeypot `

# Overview

I uploaded over 365 logs from the Kippo honeypot to Splunk. These logs were accumulated over 1 year and consists of over 11,000,000 events.
Kippo lures attackers in by running a SSH service with weak credentials.

I used Splunk to extract meaningful data from these logs such as Source IP addresses, source country and failed/successful credentials over SSH.
These were displayed using maps, tables and pie charts in a dashboard. 



# Extracting New Fields

To find the source IP address of the attackers I will extract new fields. 
![image](https://github.com/user-attachments/assets/45c51115-1ce7-47d7-a175-95820ee8891e)

This can be done using Delimeters 

![image](https://github.com/user-attachments/assets/466c46b0-6fbb-43c0-a60b-4d1b0e8ccfd9)

Using ,] as a delimiter I can get the source IP 

![image](https://github.com/user-attachments/assets/56157c9c-ff82-4df5-a84c-6610d41f55b8)

I gave tghe report a name and then I was able to see the extracted field called SiourceIP in my logs 
![image](https://github.com/user-attachments/assets/61e7a0ce-7a2c-44e0-925f-83a324e026cd)

![image](https://github.com/user-attachments/assets/8c126b01-8eb9-4fda-8fa4-8512960722cf)


I used the same process to extract the credentials that attackers were using.

## Top Source IP 

I used the following script to get the top 20 source IP addresses and used a map to illustrate them.  
Interestingly only 1 country was displayed, China.

```spl
index=* 
| stats count by SourceIP
| sort - count
| head 20
| iplocation SourceIP
| geostats latfield=lat longfield=lon sum(count) by SourceIP

```




# Top 5 countries

I used the below script to find the top 5 countries that attacks originated from.
Only 2 countries appeared on the map, China and Turkey.

```spl
index=* 
| iplocation SourceIP  
| stats count by Country lat lon 
| sort - count   
| head 5
| geostats latfield=lat longfield=lon sum(count) by Country

```

![image](https://github.com/user-attachments/assets/2c46e405-d5b3-4774-ad4b-b2ba92f03ef6)



# Top 5 failed credentials

I used the below script to show the top failed credentials
The most common one is root/password with over 1000 attempts.
This emphasizes the importance of changing default credentials. 

```spl
index=* Credentials="*" failed
| stats count by Credentials
| sort - count
| head 5
```


![image](https://github.com/user-attachments/assets/62bd7b2e-4b5e-444b-9c87-c229fe3dd0fa)

# Top 5 successful credentials


I used the below script to show the top successful credentials
The most common one is root/admin.
Again this emphasizes the importance of changing default credentials. 

```spl
index=* Credentials="*" succeeded
| stats count by Credentials
| sort - count
| head 5
```



![image](https://github.com/user-attachments/assets/b9c6368e-3507-41db-8937-5749bcca0fc4)

























