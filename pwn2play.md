# Pwn2Play Open CTF
Sat, 30 May 2026, 09:00 UTC — Sat, 30 May 2026, 18:00 UTC

Links:
- CTFtime: <https://ctftime.org/event/3220>
- Official URL: <https://pwn2play.biterra.co/>


# My facilities
<img width="1569" height="634" alt="2026-05-25-18:33:49" src="https://github.com/user-attachments/assets/9de40e24-b8a7-4a02-91ec-ea55741268d8" />


# Challenge page

<img width="2431" height="1326" alt="2026-05-30-11:02:42" src="https://github.com/user-attachments/assets/c84501b1-2028-4629-9131-2a036af18261" />


# Starting off light
Description:

A suspicious device on the network sent a single "Heartbeat" packet. We captured it in a file named heartbeat.pcap. Find the hidden message inside the packet's payload to get the flag.


We download the pcap and open it up using Wireshark, there's a single UDP packet waiting for us with the secret data:

<img width="1603" height="681" alt="2026-05-30-11:17:12" src="https://github.com/user-attachments/assets/d9149810-c6e1-42d4-9489-391e330e4a55" />

<img width="656" height="256" alt="2026-05-30-11:19:48" src="https://github.com/user-attachments/assets/48d5afb1-6b33-4641-b87a-a50669a0d871" />


# Dive right in the deep end:
The file is right there. The permissions are fine. But every time you reach for it, something stops you. Something you can't see, can't find, can't kill. It watches every move you make — and it's faster than you are. Read the flag.

<img width="965" height="77" alt="2026-05-30-11:25:04" src="https://github.com/user-attachments/assets/80a5b49f-3eab-43ae-9936-e01716098129" />

When we click the link the task description, it takes us to a lab machine in TryHackMe. We start the lab machine, download the ovpn config file and start the connection
```bash
$ sudo openvpn <connectionfile>
```


