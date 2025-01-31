## Analysing Logs from the Kippo Honeypot 

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


index=* 
| stats count by SourceIP
| sort - count
| head 20
| iplocation SourceIP
| geostats latfield=lat longfield=lon sum(count) by SourceIP


# Top 5 countries

I used the below script to find the top 5 countries that attacks originated from.

```spl
index=* 
| iplocation SourceIP  
| stats count by Country lat lon 
| sort - count   
| head 5
| geostats latfield=lat longfield=lon sum(count) by Country

(The map below visualizes the results:)





![image](https://github.com/user-attachments/assets/cf4402c5-d26a-4b27-91a0-9a61095008c7)











