## Palo Alto

Now that I have Splunk installed and it is receiving traffic from Zeek and Suricata, I now want to also send my Firewall logs to Splunk as well. Here is the initial login page for my Palo Alto 220:

![Pasted image 20240331161322](https://github.com/lm3nitro/Projects/assets/55665256/d60a2e2a-7304-4c27-bee4-da7bba51e82d)

Here I can see the traffic from my network:

![Pasted image 20240331161734](https://github.com/lm3nitro/Projects/assets/55665256/70b88005-1b92-4e9d-b659-7e9c53a27d98)

The dashboard:

![Pasted image 20240331161015](https://github.com/lm3nitro/Projects/assets/55665256/4fe525ae-cd0a-4017-8acb-92a0499e98f9)

General Info:

![Pasted image 20240331160743](https://github.com/lm3nitro/Projects/assets/55665256/de9cb3dd-bcb1-4778-b1cc-307d3fb2bb69)

In Splunk I set up 2 seperate indexes for each of my Firewalls (Palo Alto and pfSense).

![Pasted image 20240331153812](https://github.com/lm3nitro/Projects/assets/55665256/c9c3ffb1-dadd-4f56-831e-0e5b5587d0f0)

I also verified that I was indeed getting logs to Splunk:

![Pasted image 20240331152753](https://github.com/lm3nitro/Projects/assets/55665256/6d141fe7-4724-467a-bb56-93caa4ae97e2)

![Pasted image 20240331152325](https://github.com/lm3nitro/Projects/assets/55665256/8d1dcac5-1bd7-4564-af18-df9313c08093)

Also, I can see that my indexes are getting information:

![Pasted image 20240331150634](https://github.com/lm3nitro/Projects/assets/55665256/508547e7-7cac-4a42-9b3b-155c43669283)

Here I can see that I am getting application detection from the NGFW. 

![Pasted image 20240331151611](https://github.com/lm3nitro/Projects/assets/55665256/cf3ac96f-6633-40fa-89f3-0df88350a48e)

Application detection information from a Next-Generation Firewall (NGFW) is crucial for several reasons:

- Granular Control
- Security Policy Enforcement
- Threat Detection
- Visibility and Analytics
- Compliance and Reporting
  
I also checked to see the amount of events that had been generated in Splunk:

![Pasted image 20240331153221](https://github.com/lm3nitro/Projects/assets/55665256/6f5a0928-7471-4ca2-840b-f3ed33af272f)

I checked and verified the CPU as well to ensure efficient system performance:

![Pasted image 20240331153652](https://github.com/lm3nitro/Projects/assets/55665256/b77b171b-9ad3-47f0-8d9f-abdaf75ce3dc)

![Pasted image 20240331153506](https://github.com/lm3nitro/Projects/assets/55665256/93382723-6afa-4c29-96bb-6881d1b768b1)

![Pasted image 20240331153549](https://github.com/lm3nitro/Projects/assets/55665256/f516253e-725a-4fab-839f-0fd8fccfb12c)


