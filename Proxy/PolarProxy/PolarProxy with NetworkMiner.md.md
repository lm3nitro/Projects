# PolarProxy and NetworkMiner

While using polarProxy, I wanted to continue to view the traffic and better see the capabilities that it offers. In this scenario I went to Google and searched for a random picture of a tree.

![Pasted image 20240409185436](https://github.com/lm3nitro/Projects/assets/55665256/b1b90bba-7f8f-4892-860a-9efd4e0bc0bd)

I do have the PolarProxy running from when I previosly installed it and the proxy has saved the traffic:

![Pasted image 20240409185821](https://github.com/lm3nitro/Projects/assets/55665256/ee85f1a8-5e2f-4d05-859b-35374609ddcc)

Here I am going to look at the pcap saved by PolarProxy using NetworkMiner:

![Pasted image 20240409185850](https://github.com/lm3nitro/Projects/assets/55665256/66506397-cafd-4f5d-970d-8cf1a96920ab)

Analysis of the Pcap in NetworkMiner:

![Pasted image 20240409190734](https://github.com/lm3nitro/Projects/assets/55665256/605ce843-c733-4143-ada0-64c0b2020a7c)

![Pasted image 20240409190418](https://github.com/lm3nitro/Projects/assets/55665256/9d5621d4-b84c-4c0e-999d-7daccc4fadec)

In NetworkMinor I was able to locate the image that was previosly searched for in Google:

![Pasted image 20240409190032](https://github.com/lm3nitro/Projects/assets/55665256/5fcd7dbe-45c6-426d-90ef-a0c95a3f0bf3)

Next I used Wireshark to find the image:

![Pasted image 20240409191112](https://github.com/lm3nitro/Projects/assets/55665256/c1eb5238-a47e-440a-b122-33a851a5a63b)

Once the image was found, I tried exporting the image from the HTTP2 trace:

![Pasted image 20240409191503](https://github.com/lm3nitro/Projects/assets/55665256/5684fe95-a684-4da0-85e7-0532ab403ccc)

![Pasted image 20240409191541](https://github.com/lm3nitro/Projects/assets/55665256/a6d21c07-d033-4375-86da-7de5dbeea955)

![Pasted image 20240409191342](https://github.com/lm3nitro/Projects/assets/55665256/7bd08c85-8358-41ae-807a-72d30e8beaf9)

As another quick test, I went to Instagram to check the headers:

![Pasted image 20240409192913](https://github.com/lm3nitro/Projects/assets/55665256/f64ea7be-1e67-45bb-9f6e-c297502152b5)

Here I used Wireshark to view the pcap that PolarProxy captured from this traffic:

![Pasted image 20240409195637](https://github.com/lm3nitro/Projects/assets/55665256/d3ceb9a4-8ffb-4d4f-98dc-2ce545aa426f)

By reviewing the network traffic that was captured while using PolarProxy was very beneficial and I was able to see the decrypted traffic which allowed me to better understand the data exchanged between my system and external servers. 
