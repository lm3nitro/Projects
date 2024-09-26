# PolarProxy with NetworkMiner

### Scope:

In part 1 I was able to isntall and configure PolarProxy. In part 2, I will intercept and decrypt traffic using PolarProxy while downloading an image from google. The decrypted traffic will be captured and analyzed in NetworkMiner, where  I will then extract the image from the PCAP file. Successful testing confirms that PolarProxy properly decrypts the traffic, and NetworkMiner accurately displays the image. I will also be analyzing regular traffic visiting a website to see how PolarProxy is able to capture it. This process aims to ensure that PolarProxy accurately captures the network traffic, allowing for the successful extraction of files, such as images, from the decrypted data.

### Tools and Technology:
Ubuntu, PolarProxy, NetworkMiner and Wireshark

## Google search:

In part 1, PolarProxy wss installed and configured. This is picking up from part 1 and PolarProxy is currently running and configure. To start, on my host machine that I am using to test, I went to google, searched for an image of a random tree and downloaded the image:

![Pasted image 20240409185436](https://github.com/lm3nitro/Projects/assets/55665256/b1b90bba-7f8f-4892-860a-9efd4e0bc0bd)

PolarProxy saves the pcaps in the following directory:

![Pasted image 20240409185821](https://github.com/lm3nitro/Projects/assets/55665256/ee85f1a8-5e2f-4d05-859b-35374609ddcc)

Now that I have the pcap from the traffic generated when I went to google, I will be using NetworkMiner to analyze it:

```
sudo mono NetworkMiner.exe --noupdatecheck /var/log/PolarProxy/proxy-24049-212028.pcap
```

![Pasted image 20240409185850](https://github.com/lm3nitro/Projects/assets/55665256/66506397-cafd-4f5d-970d-8cf1a96920ab)

Here I was able to see the conversation between my host and the web server:

![Pasted image 20240409190734](https://github.com/lm3nitro/Projects/assets/55665256/605ce843-c733-4143-ada0-64c0b2020a7c)

By going to files, I was able to locate the image that was previosly searched for in Google:

![Pasted image 20240409190418](https://github.com/lm3nitro/Projects/assets/55665256/9d5621d4-b84c-4c0e-999d-7daccc4fadec)

![Pasted image 20240409190032](https://github.com/lm3nitro/Projects/assets/55665256/5fcd7dbe-45c6-426d-90ef-a0c95a3f0bf3)

Next, I used Wireshark and was able to filter the traffic down to find the traffic related to the image:

![Pasted image 20240409191112](https://github.com/lm3nitro/Projects/assets/55665256/c1eb5238-a47e-440a-b122-33a851a5a63b)

Once the image was found, I tried exported the image:

![Pasted image 20240409191503](https://github.com/lm3nitro/Projects/assets/55665256/5684fe95-a684-4da0-85e7-0532ab403ccc)

![Pasted image 20240409191541](https://github.com/lm3nitro/Projects/assets/55665256/a6d21c07-d033-4375-86da-7de5dbeea955)

Here I was able to see same image of the tree that was exported:

![Pasted image 20240409191342](https://github.com/lm3nitro/Projects/assets/55665256/7bd08c85-8358-41ae-807a-72d30e8beaf9)

As another quick test, I went to Instagram to check the headers:

![Pasted image 20240409192913](https://github.com/lm3nitro/Projects/assets/55665256/f64ea7be-1e67-45bb-9f6e-c297502152b5)

Here I used Wireshark to view the pcap that the PolarProxy captured:

![Pasted image 20240409195637](https://github.com/lm3nitro/Projects/assets/55665256/d3ceb9a4-8ffb-4d4f-98dc-2ce545aa426f)

By reviewing the network traffic that was captured while using PolarProxy I was able to see the decrypted traffic which allowed me to better understand the data exchanged between my system and the external server. 

### Summary:

In summary, using PolarProxy provides essential capabilities for decrypting, capturing, and analyzing network traffic, which is crucial for uncovering evidence, understanding malicious activities, and ensuring compliance with regulations. These capabilities enhance the overall effectiveness of digital forensic investigations and help establish a clearer understanding of security incidents.

This hands-on experience improved my understanding of network traffic analysis and helped me apply these techniques in real-world forensic situations. It ensured I could effectively manage encrypted communications and uncover important information. These tools work together to facilitate the identification of suspicious behavior, the collection of evidence, and the reconstruction of events surrounding security incidents. This comprehensive approach ultimately strengthens the capability of forensic analysts to conduct thorough investigations and provide actionable insights into cybersecurity threats.
