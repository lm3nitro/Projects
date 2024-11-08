# Palo Alto

The Palo Alto Networks PA-220 is a next-generation firewall designed for small to medium-sized businesses, this is the version that I currently have in my home. What I like the most about this firewall is its URL filtering, application visibility, and user-based policies. 
This was the first Firewall I had purchased.

Unboxing:

![image](https://github.com/lm3nitro/Projects/assets/55665256/cf1fd83b-ca7d-4838-92dd-b2556d4c29bc)

Here is the initial login page for my Palo Alto 220:

![Pasted image 20240331161322](https://github.com/lm3nitro/Projects/assets/55665256/d60a2e2a-7304-4c27-bee4-da7bba51e82d)

Here I can see the traffic from my network:

![Pasted image 20240331161734](https://github.com/lm3nitro/Projects/assets/55665256/70b88005-1b92-4e9d-b659-7e9c53a27d98)

The main dashboard:

![Pasted image 20240331161015](https://github.com/lm3nitro/Projects/assets/55665256/4fe525ae-cd0a-4017-8acb-92a0499e98f9)

The general information:

![Pasted image 20240331160743](https://github.com/lm3nitro/Projects/assets/55665256/de9cb3dd-bcb1-4778-b1cc-307d3fb2bb69)

In Splunk I also have set up 2 separate indexes for each of my Firewalls (Palo Alto and pfSense).

![Pasted image 20240331153812](https://github.com/lm3nitro/Projects/assets/55665256/c9c3ffb1-dadd-4f56-831e-0e5b5587d0f0)

I have my Palo Alto logs configured to be sent to Splunk:

![Pasted image 20240331152753](https://github.com/lm3nitro/Projects/assets/55665256/6d141fe7-4724-467a-bb56-93caa4ae97e2)

![Pasted image 20240331152325](https://github.com/lm3nitro/Projects/assets/55665256/8d1dcac5-1bd7-4564-af18-df9313c08093)

Also, I can see that my indexes are getting information:

![Pasted image 20240331150634](https://github.com/lm3nitro/Projects/assets/55665256/508547e7-7cac-4a42-9b3b-155c43669283)

Here I can see that I am getting application detection from the Palo Alto NGFW. 

![Pasted image 20240331151611](https://github.com/lm3nitro/Projects/assets/55665256/cf3ac96f-6633-40fa-89f3-0df88350a48e)

Application detection information from a Next-Generation Firewall (NGFW) is crucial for several reasons:

- Granular Control
- Security Policy Enforcement
- Threat Detection
- Visibility and Analytics
- Compliance and Reporting

Overall, I highly recommend having a firewall in your home network. It not only provides controlled access to the internet, allowing you to manage which applications and services can communicate externally, most modern firewalls often include features like parental controls, further adding to the overall safety and control of your home network.
