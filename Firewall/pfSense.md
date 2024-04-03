## pfSense:

As previously mentioned, I also have a pfSense Firewall in my home network.

![Pasted image 20240331162214](https://github.com/lm3nitro/Projects/assets/55665256/8a3abddc-3d87-4557-aa06-f43a30f10a11)

Here we can see the system information. I installed this firewall back in December and it is currently running version 23.09.1:

![Pasted image 20240331161939](https://github.com/lm3nitro/Projects/assets/55665256/649aa069-e0a3-4928-a6d2-353c5a7da1d6)

Here is the initial dashboard upon logging into the firewall:

![Pasted image 20240331155455](https://github.com/lm3nitro/Projects/assets/55665256/0b322e24-055c-46f1-8913-2bd2b0501425)

Here are the Rules that I currently have in place:

![Pasted image 20240331155547](https://github.com/lm3nitro/Projects/assets/55665256/6dd0ad15-9d96-4eb0-bf3a-7c06fc1c92d8)
![Pasted image 20240331155659](https://github.com/lm3nitro/Projects/assets/55665256/eb3dcda9-6d21-481d-b57a-4f0172e526c0)

I also have NAT configured:

![Pasted image 20240331155824](https://github.com/lm3nitro/Projects/assets/55665256/401e6649-223e-464b-a678-b680ff1e98b3)

Here's a view of the pfSense logs that I am sending to Splunk. This is how they look in the firewall:

![Pasted image 20240331155954](https://github.com/lm3nitro/Projects/assets/55665256/65d7fd56-6185-4f66-9799-38f1c10866c1)

And this is the view form Splunk:

![Pasted image 20240331151819](https://github.com/lm3nitro/Projects/assets/55665256/61aa89c4-c610-440c-b96b-f3a5e98483b6)

PfSense fields in Splunk:

![Pasted image 20240331151902](https://github.com/lm3nitro/Projects/assets/55665256/a5ef5870-0fa3-4bbd-8bf5-4d215f358144)

- Source IP and Destination IP
- Port Numbers
- Protocol
- Action
- Time and Date
- Interface
- TTL and more...
