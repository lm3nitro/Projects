
# SPAN PORT 

In my home I current have a Cisco Switch 3750G. I currently want to add it to my network and want to configure a Span Port on it. Below are the steps to do that.

This is my Cisco Switch:

![Pasted image 20240416133118](https://github.com/lm3nitro/Projects/assets/55665256/6b784043-d9fd-48bd-baca-455f8766f7f1)

This is the current version that I am running:

![Pasted image 20240416134839](https://github.com/lm3nitro/Projects/assets/55665256/5eed5836-b5c4-4f80-95d7-1da335b3e2f9)

Here is an example topology of what I am going to be doing. I will have 1 PC connected to a port, and I will have the other port set up as a span port with a monitoring device connected to that port.

![Pasted image 20240416133855](https://github.com/lm3nitro/Projects/assets/55665256/b1d33333-adb1-49bc-b93e-9e25c1dfafdb)

Lets take a look at out interfaces

```
show interfaces
```

![Pasted image 20240416133825](https://github.com/lm3nitro/Projects/assets/55665256/aad9e3f5-6109-41af-9ab5-2e5c82a5d51c)

These are the commands that I will use to configure the SPAN session.
```
monitor session 1 source interface Gi2/0/1
```
This command configures source traffic for the SPAN session. In this case, it selects GigabitEthernet interface 2/0/1 (Gi2/0/1) as the source interface. This means that all traffic passing through interface Gi2/0/1 will be copied (mirrored) to the SPAN session.
```
monitor session 1 destination interface Gi2/0/3
```
This command configures the destination for the SPAN session. It specifies that the mirrored traffic from the source interface (Gi2/0/1) will be sent to GigabitEthernet interface 2/0/3 (Gi2/0/3). This destination port is where I will connect my monitoring device to capture and analyze the mirrored traffic.

![Pasted image 20240416134151](https://github.com/lm3nitro/Projects/assets/55665256/3b007c12-044d-48db-adca-6a234f8181fc)

You can verify the monitor port with the following command:
```
show monitor session 1
```



