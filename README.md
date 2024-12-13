# tcpdump Challenge CTF

<h2>Description</h2>
In this tcpdump challenge, I used tcpdump command-line analysis to investigate an endpoint alert triggering detections pointing to a potential info-stealer malware. 


<h2>Languages and Utilities Used</h2>

- <b>tcpdump</b>
- <b>linux CLI</b>


<h2>Environments Used </h2>

- <b>Unbuntu</b> 

<br />
<br />
Challenge prompt.

![1) scenario prompt](https://github.com/user-attachments/assets/d293eb98-65ba-4337-9c04-7498e4516af2)

<br />
<br />
Packet count enumeration for total count and icmp packets.

![2) questions 1 2](https://github.com/user-attachments/assets/7d3c666f-5658-4056-b664-cb26be2ef19c)

<br />
<br />  
The ASN for the destination IP was asked for. 

![3](https://github.com/user-attachments/assets/01ddfae5-0f5b-4ad9-9ea6-ceec7289630d)

<br />
<br />
Asked how many http post requests were in the packet. Filtering showed just 1. 

![4) q4 1 http post request](https://github.com/user-attachments/assets/52fac50d-b321-4e59-95e3-534201b904e9)

<br />
<br />
Reading the pcap in ASCII with -A | then grep to remove (-v) User-Agent | then grep -i case insensitive to search for 'user\|pass\|login'. Forgot the \ after pass but results still showed the password inside the http payloads.

![5 q asscii then grep -v useragent then grep unpasslogin](https://github.com/user-attachments/assets/c9f7054e-85a2-4e5c-9f15-901eb541cd29)

<br />
<br />
Filtered for the pcap with -n to not resolve ips to domain names | then cut -d " " to remove the first 4 objects with spaces leaving the 5th one with -f 5 | the  cut -d "." the first 4 objects with periods leaving the 5th one with -f 5 | then sort | then filtered duplicates and counted amount per unique result with uniq -c | then sorted in reverse numerical with sort -nr showing greatest first. Question was asking for 2nd most common port, so ephemerial port 44174 is omitted leaving port 21 next.  

![6) other well known dst port ](https://github.com/user-attachments/assets/46f8a198-5791-4055-bbea-eb1c4de3a268)

<br />
<br />
Read pcap in ASCII with -A | grepped for user and pass with grep -E "USER|PASS" to show all user and pass strings. Tried all 5 and the last user/pass was the answer. I'm guessing its the answer since it was the most recent user/pass login. 

![7) finding ftp user and pass](https://github.com/user-attachments/assets/c23474f7-455a-4431-860b-0204741ece02)

<br />
<br />  
Using the src of 10.0.2.10 and dst 194.168.117.16 shows the file retrieved through ftp. 

![8) file retrieved from ftp server](https://github.com/user-attachments/assets/d79f5c73-ed98-40a9-b094-d6df312dc1b1)

<br />
<br />
Reading the pcap with src 10.0.2.10 in ASCII with -A and | grepping for "USer-Agent" I notied an odd UA named TeslaBrowser. Doing some OSINT, it turns out the TeslaBrowser UA was a Lumma info stealer. 

![9 lumma](https://github.com/user-attachments/assets/05b26a36-ce05-4712-b9b2-4d721293b462)

<br />
<br />
Reading the pcap in ASCII with -A then | grepping for "Tesla" -A 50 to print 50 lines of context after each matching Tesla* line. This showed a URL the endpoint tried to connect to. 

![10) t me](https://github.com/user-attachments/assets/e4d7e974-5185-4c00-b29b-1384e5dcccb3)

<br />
<br />
BONUS: got rick rolled | grepping for "youtube" lol. 

![11)rickroll](https://github.com/user-attachments/assets/8b16b952-e770-484e-9026-2f95fe627b4b)

<br />
<br />
