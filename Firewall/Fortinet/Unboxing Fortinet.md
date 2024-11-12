# Unboxing Fortinet and Registration

I successfully integrated Fortinet into my home network, enhancing its security and protection against cyber threats. Starting with thorough research, I selected a Fortinet model that suited my needs, I decided to go with the 60F model. Setting up the Fortinet device was straightforward, and I gained hands-on experience by exploring its interface, configuring security policies, DNS, and more.

## Unpackaging

<img width="466" alt="Screenshot 2024-04-06 at 8 57 28 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1dc3a0cc-d19f-4d1c-95de-b78d91bccdc8">
<img width="569" alt="Screenshot 2024-04-06 at 9 04 16 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/1790616a-63f3-495f-bef1-43028c387072">
<img width="575" alt="Screenshot 2024-04-06 at 9 04 03 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8aaffd68-14c9-4366-8266-3ead4aa4c52c">
<img width="581" alt="Screenshot 2024-04-06 at 9 04 27 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a5217ca0-04ac-4d80-9f15-c480dba680c9">

## Configuration 
Now to set it up. First, I powered it on, went to the login page, in this case the default was 192.168.1.99. The default username is admin and there is no password. Once I was in. it was time to get the firewall registered. To get it registered, I went to the forticloud login page and selected login/register:

<img width="1094" alt="Screenshot 2024-04-06 at 9 09 25 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/a66a2e60-bf2b-4746-95d4-84c959ef1679">

Once I logged in, I then proceeded to register the device using the FortiCloud Key that came with the device itself. Once completed, I was able to confirm it indeed was registered:

<img width="1437" alt="Screenshot 2024-04-06 at 9 16 46 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/c6000f36-c09a-4a18-af1f-9eb177c6b8dc">

Now that the device was registered, I needed to make sure that everything was activated on the actual firewall: 

<img width="953" alt="Screenshot 2024-04-06 at 9 54 15 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/66574006-5062-4840-8a90-682cd4671421">

Upon logging in, I navigated to System > FortiGuard, there I can see that my firewall was not showing as registered:

<img width="272" alt="Screenshot 2024-04-06 at 9 36 33 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/f49550e4-e1d5-42b9-bd16-45937af4180a">

When trying to register the firewall, I was presented with an error stating that it was already registered...which is true, because I had registered it in FortiCloud previously. 

<img width="738" alt="Screenshot 2024-04-05 at 9 25 23 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/759c88f4-95d8-4bbb-8b02-7de45517e438">

After trial and error of trying to get it registered, I found out that by trying to update the firewall, it was triggered, and it allowed me to log into FortiCloud and make the connection. 

<img width="405" alt="Screenshot 2024-04-05 at 9 23 22 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/777dff18-325f-4375-b7db-d531c74a637c">

<img width="681" alt="Screenshot 2024-04-06 at 9 44 39 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/03331b2f-c876-4018-b2a5-c1b611062d28">

I then proceeded to install the latest update. At the time of this writing, the latest available was v7.2.8. 

<img width="1188" alt="Screenshot 2024-04-05 at 11 32 39 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8404b738-17ed-42e8-8a7c-61f4abe98b2d">

<img width="674" alt="Screenshot 2024-04-05 at 11 33 24 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/8c643013-db7e-4292-bdd7-1b48eaccd9e0">

I went ahead and downloaded a copy of the configuration file in case anything happened during the upgrade and proceeded to update. This was done faster than I thought and only took about 10 minutes to complete.

<img width="1434" alt="Screenshot 2024-04-05 at 11 40 06 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/4c7e3077-fc10-4941-9972-bbe9aeba9aaf">

<img width="550" alt="Screenshot 2024-04-05 at 11 40 17 PM" src="https://github.com/lm3nitro/Projects/assets/55665256/01e54dd0-b65f-4178-928f-a77d1bfe6cd0">

Now that the firewall is registered, activated, and up to date, I proceeded to getting everything else set-up including DNS, interfaces, policies, and more. 

### Summary:

Installing and getting it was fairly easy; I like how user friendly the interface is. Having a firewall in your home network is crucial to protect against intrusion, network control, and for privacy and malware defense. Having a firewall is a fundamental layer of security that enhances the safety of your home network.  

