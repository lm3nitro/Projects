# NFDump



Installation:


![Pasted image 20240527140837](https://github.com/lm3nitro/Projects/assets/55665256/74449460-6743-465b-8d2f-e52d7b204aae)



Creating a directory where all the NetFlow data will be store, in my case I created a directory under /var/flows/lm3nitro-r1


Start capturing  the NetFlow data:

Let's make sure that we're recving data from lm3nitro-r1 (Cisco Router)

![Pasted image 20240527142135](https://github.com/lm3nitro/Projects/assets/55665256/0506a4a2-0150-4bd1-ad76-5a54bd125205)


Let's start the nfcapd demon to capture the NetFlow collection:

nfcapd -D -T all -n lm3nitro-r1,172.16.10.1,/var/flows/lm3nitro-r1 -p 9996

Verify the UDP port 996 is opened:

![Pasted image 20240527142337](https://github.com/lm3nitro/Projects/assets/55665256/f765bd7d-7f17-4c65-a882-5c08e0118e38)




Checking the status of the NFDUMP service:


![Pasted image 20240527185750](https://github.com/lm3nitro/Projects/assets/55665256/55342320-3783-4c9d-9116-b1fa4aad9885)



Verify if we have data under the directory :


![Pasted image 20240527141756](https://github.com/lm3nitro/Projects/assets/55665256/f11adfed-6ff2-47e2-89a4-311907bd45d3)


Note:

nfcapd daemon receives NetFlow streams and saves them into local files, switching to a new file every 5 minutes (configurable). The naming starts with nfcapd, then dot, and finally date and time stamp.




# Statistics about all the flows in a records file:

nfdump -I -r nfcapd.202405271826


![Pasted image 20240527143601](https://github.com/lm3nitro/Projects/assets/55665256/d55575f4-4051-4bfe-bfeb-679a541798e1)

# Read records form a single file:

nfdump  -r nfcapd.202405271821

![Pasted image 20240527143708](https://github.com/lm3nitro/Projects/assets/55665256/e8802ad8-4f11-496f-a093-fa9d9220c235)




# Print all records from a range of files, starting at  nfcapd.202405271821 and up to but not including the current file.

nfdump  -R nfcapd.202405271821

![Pasted image 20240527144148](https://github.com/lm3nitro/Projects/assets/55665256/661347ff-5dd6-4c70-a845-c8bd3286f983)




# Read sessions where the source or destination IP is 8.8.8.8

![Pasted image 20240527144458](https://github.com/lm3nitro/Projects/assets/55665256/5a65f130-d7e3-4de5-9cb7-286413923cf4)


# Print sessions where the destination port is 443 and the protocol is TCP

nfdump -R .  'dst port 443 and proto tcp'

![Pasted image 20240527145444](https://github.com/lm3nitro/Projects/assets/55665256/30d58462-6c57-4571-bb35-4e95bda2aa69)



# Show top 10 flows sorted by the bits per second statistics

Note: -o extended sets output to include also bps column. -n 10 limits output to top 10 rows (which is default as well). Finally, -O bps tells nfdump to sort the output by bits per second value in descending (default) order.

nfdump -R . -n 10  -O bps -o extended


![Pasted image 20240527145643](https://github.com/lm3nitro/Projects/assets/55665256/add20c99-7378-44f5-817e-26617bd557a6)


note: This shows the hosts that are not generating any traffic:
-n 0  -O bps -o extended 


# Aggregate all flows to/from host  based on source IP 54.230.253.40

 nfdump -R . -A srcip ' host 54.230.253.40'


![Pasted image 20240527150039](https://github.com/lm3nitro/Projects/assets/55665256/49576394-6438-4768-a98c-f8ae8d1bf7dd)




# Calculate statistics for port 443 traffic and sort by bps to see bandwidth abusing hosts


Note; We can include as many -s as needed, each statistics table will be printed independently. Statistics will be calculated for the flows located in this specific nfcapd. file, to count statistics over longer periods of time see -R & -M

nfdump -R . -s srcip/bps -s dstip/bps  ' port 443'


![Pasted image 20240527150311](https://github.com/lm3nitro/Projects/assets/55665256/4f288d44-b7be-473c-bee7-c911efe9757b)



# Sort presented flows by duration, longest at the bottom
nfdump itself has no provision to sort flows by their duration, but we can easily pipe the output to any Linux sorting tool. Let’s display top 10 flows by duration:

nfdump -R .  | sort -n -k3,3 | tail -10

![Pasted image 20240527150453](https://github.com/lm3nitro/Projects/assets/55665256/12ad3916-14ea-44c4-9dc5-9d3106482845)


# Find records in the time range of 12:19:00 - 12:20:00 matching the filter of protocol =TCP and port = 80



Using -t option we can limit the time range of the records to look into. nfdump puts 0 for any missing time part, e.g. 12:19 means 12:19:00.

nfdump -R .  -t 2024/05/27.17:04:09-2024/05/27.18:21:09 'port 80 and proto tcp'

![Pasted image 20240527151106](https://github.com/lm3nitro/Projects/assets/55665256/4bd10cf9-e301-4764-baa6-f88c6865c6b5)



Summary:


What we learn and what we did?

NetFlow is a great protocol to get an insight in your network traffic. It’s the equivalent of a “phone bill” that specifies all calls that were made, where these calls took place, the duration, etc. Only this time, we are tracking all IP packets on the network.


# Nfdump to csv:


nfdump -r /var/flows/nfcapd.2024052949 -o csv > /var/log/netflow/mycsvfile.csv



