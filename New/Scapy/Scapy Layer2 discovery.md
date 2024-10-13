

```

from scapy.all import *  
  
# Define the target subnet  
target_subnet = "10.10.100.0/24"  
  
# Send ARP requests  
answered, unanswered = srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=target_subnet), timeout=2, verbose=False)  
  
# Print out the list of discovered hosts  
for sent, received in answered:  
    print(f"Host Up: {received.psrc} MAC: {received.hwsrc}")
```


![Pasted image 20241009234740](https://github.com/user-attachments/assets/6cfd6085-34c6-4a13-8018-e62a38312e44)



![Pasted image 20241009234458](https://github.com/user-attachments/assets/bc259adf-070d-44e6-914c-36ceaf9770e1)




![Pasted image 20241009234024](https://github.com/user-attachments/assets/ff3c1dec-89bf-4b42-8afd-757f15ed3d94)
