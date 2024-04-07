# Unboxing Fortinet and Registration

I've successfully integrated Fortinet into my home network, enhancing its security and protection against cyber threats. Starting with thorough research, I selected a Fortinet model that suited my needs, I decided to go with the 60F model. Setting up the Fortinet device was straightforward, and I gained hands-on experience by exploring its interface, configuring security policies, DNS, and more.

<img width="466" alt="Screenshot 2024-04-06 at 8 57 28 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1dc3a0cc-d19f-4d1c-95de-b78d91bccdc8">
<img width="569" alt="Screenshot 2024-04-06 at 9 04 16 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1790616a-63f3-495f-bef1-43028c387072">
<img width="575" alt="Screenshot 2024-04-06 at 9 04 03 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8aaffd68-14c9-4366-8266-3ead4aa4c52c">
<img width="581" alt="Screenshot 2024-04-06 at 9 04 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a5217ca0-04ac-4d80-9f15-c480dba680c9">

Now to set it up. First I powered it one, when to the login page, in this case the default is 192.168.1.99, the default username is admin and there is no password. Once I was in. it was time to get the firewall. To get it registered, we will go to the forticloud login page and select login/register:

<img width="1094" alt="Screenshot 2024-04-06 at 9 09 25 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a66a2e60-bf2b-4746-95d4-84c959ef1679">

Once I logged in, I then proceeded to register the device using the FortiCloud Key that came with the device itself. Once that is done, I was able to confirm it indeed was registered:

<img width="1437" alt="Screenshot 2024-04-06 at 9 16 46 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c6000f36-c09a-4a18-af1f-9eb177c6b8dc">

Now that the device was registered, I needed to make sure that everything was activated on the actual firewall. 

<img width="520" alt="Screenshot 2024-04-06 at 9 21 03 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/08973429-d885-4243-aae4-c7cf795281d3">

Upon logging in, I navigated to System > FortiGuard, there I can see that my firewall was not showing as registered:

<img width="272" alt="Screenshot 2024-04-06 at 9 36 33 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f49550e4-e1d5-42b9-bd16-45937af4180a">

When trying 
