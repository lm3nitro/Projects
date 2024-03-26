

Filtering for tcp syn flags and port range to identify transport protocol abnormal behaviour

![[Pasted image 20240325120655.png]]

Note: It seems like the threat actor on 192.168.11.62 is scanning the node 192.168.11.46 looking for open ports.  We can see the amount sync request going to 192.168.11.46 in many ports in fraction of a second.

Narrow down the host to identify how many syncronizetion request was send by the threat actor:

![[Pasted image 20240325122547.png]]



