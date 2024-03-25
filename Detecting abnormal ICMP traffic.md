
# Detectiong Abnormal ICMP Traffic

Verify the pcap information with Capinfos:

## DESCRIPTION

**Capinfos** is a program that reads one or more capture files and returns some or all available statistics (infos) of each <_infile_> in one of two types of output formats: long or table.

![Pasted image 20240324111115](https://github.com/lm3nitro/Projects/assets/55665256/3afe59b6-d7e7-4868-adb2-571ba9d15d53)

Step 1:
Note: In this pcap we have 544 packets we will need to filter to find out the abnormal packets. 
 
Filter for ICMP number of  packets :
  
![Pasted image 20240324112004](https://github.com/lm3nitro/Projects/assets/55665256/5d960c59-e605-4528-9c0f-f4a29fe5b3c2)

Note: Normal ICMP packets are usually 64 bytes data in length and it has incremental ASCII characters values. 

>###Example:

![Pasted image 20240324120252](https://github.com/lm3nitro/Projects/assets/55665256/49788d58-5421-4845-b7df-621757143e3c)

Filtering for ICMP packets over "64" bytes data size to detect suspicious ICMP behaviour  :

![Pasted image 20240324122350](https://github.com/lm3nitro/Projects/assets/55665256/926eaefb-50ff-4c96-8e22-95e8f2e2f2cb)

Selecting the first packet for payload inspection: 

![Pasted image 20240324123214](https://github.com/lm3nitro/Projects/assets/55665256/493a02df-75ca-4681-8f8e-c75320c14687)

Note: As you can see we where able to find an ICMP tunnel. The threat actor is communicating via an encrypted SSH tunnel and using ICMP protocol to masquerade his communication. This technique is commonly use for data exfiltration or command in control.


Identifying ICMP scanning:

In this scenario we want to detect for any sings of reconesence  from the threat actor over ICMP to our network 192.168.11.0/24

![Pasted image 20240324134231](https://github.com/lm3nitro/Projects/assets/55665256/3068949f-c8ac-4078-924f-411d35e0b45a)

Payload inspection:

![Pasted image 20240324135225](https://github.com/lm3nitro/Projects/assets/55665256/10aa77d8-a801-4330-ad75-1c17a866d233)


Note:  The indicator  or sings of  scanning are:

Even though the payload looks like a leyement one  the key factor are order of  sequeial proving [1,2,3,4,10] . The IP identification is the same, also the speed of each request is lighting fast


In this scenario we're to looking for any sings of ICMP estrange behaviour to network 172.16.1.0/24:  

![Pasted image 20240324131438](https://github.com/lm3nitro/Projects/assets/55665256/3eae4d30-ef1a-43b5-8a19-9edc817d10a7)

Payload inspection:

![Pasted image 20240324132714](https://github.com/lm3nitro/Projects/assets/55665256/7ec999ae-60f1-4376-ade9-f40ecd716a3a)

Note:  The indicators are:

The order of  sequeial probes [1,2,3..23] . The IP identification in this case are chaing per request but the lenght of the ICMP payload is 8 , normal ICMP should be 64 also the speed of each request is lighting fast not to mention that the payload  does not contain any incremental values inside, it looks very suspicious. 


Identify ICMP scanning spoofing random source:

Filtering for echo request:

![Pasted image 20240324171440](https://github.com/lm3nitro/Projects/assets/55665256/03a3650b-8666-4ee6-b5e6-f9d0f79144be)

Filtering for echo Reponses: 

![Pasted image 20240324172031](https://github.com/lm3nitro/Projects/assets/55665256/7807bb0b-f4d3-4bd4-a8b9-28894ac34055)

Analysing the TTL value of the source address

![Pasted image 20240324172644](https://github.com/lm3nitro/Projects/assets/55665256/940955ad-bf56-4ff9-a18c-2af5a6049041)

Note: The mount of ICMP request coming from different sources at the same time with the same TTL value, data length and the IP identification ID. It seems like all the source are  not even passing trough  a single router.  Also the fact that public IP ranges are sending traffic to  an internal host would mean that the host is behind the NAT or the a threat actor is spoofing the source IP address to evade detection. It's very suspicious. 




