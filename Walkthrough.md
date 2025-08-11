# Walkthrough on Mrrobot CTF
## 🛠️Tools used
---
- Nmap
- Gobuster / Dirb
- Hydra
- John the Ripper
- Netcat
- Python (Reverse Shells)
- LinPEAS / GTFOBins
---

## Step 1 Enumeration using Nmap
---
 Nmap is tool used to scan network and identify hosts and open ports. It may also be used to identify operating systems the hosts run.
In our case we will use Nmap as follows;
```
(kali@ kali)-[~]
-$ nmap -sS -T4 10.38.1.110-120
Starting Nmap 7.95 ( https://nmap.org ) at 2025-08-09 10:57 EAT
Nmap scan report for 10.38.1.110
Host is up (0.0000090s latency).
All 1000 scanned ports on 10.38.1.110 are in ignored states.
Not shown: 1000 closed tcp ports (reset)

Nmap scan report for 10.38.1.111
Host is up (0.00071s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE   SERVICE
22/tcp   closed   ssh
80/tcp   open     http
443/tcp  open     https
MAC Address: 08:00:27:6C:45:D8 (PCS Systemtechnik/Oracle VirtualBox virtual NIC)

Nmap done: 11 IP addresses (2 hosts up) scanned in 32.47 seconds
```
Using this scan we can see that there is only one host who is up on the network whose IP is 10.38.1.111.
The host also has open ports; 22, 80 and 443 which ultimately mean that the host is a web server.
We can then head to http://10.38.1.111
You can then look around and get to know web app and there is also a lot of fun and cool stuff there but still there is nothing that can lead us to getting the flags.

---
## Step 2 Scanning for hidden directories AKA Web fuzzing

---
For this we will use dirb.
Dirb is a tool that looks for hidden directories within the web application using a pre-written word list.
You can use it as follows;
```
(kali@ kali)-[~]
$ dirb http://10.38.1.111
```
It'll take sometime and yes, I know the outpt is a lot. However, we can see some useful directories like;
```
+ https://10.38.1.111/robots.txt (CODE:200 |SIZE:41)
```
and
```
+ https://10.38.1.111/wp-links-opml (CODE:200|SIZE:227)
+ https://10.38.1.111/wp-load (CODE:200|SIZE:0)
+ https://10.38.1.111/wp-login (CODE:200|SIZE:2605)
+ https://10.38.1.111/wp-mail (CODE:500|SIZE:3064)
+ https://10.38.1.111/wp-settings (CODE:500|SIZE:0)
+ https://10.38.1.111/wp-signup (CODE:302 |SIZE:0)
```
The robot.txt file usually contains confidential files that the developer wants to hide like passwords.
We can also observe that there is a wordpress login page (https://10.38.1.111/wp-login). This means that the website is a word press web application.
Let's head to the robot.txt directory and see what it is all about.

<img width="949" height="876" alt="VirtualBox_kali-linux-2025 1c-virtualbox-amd64_11_08_2025_12_50_24" src="https://github.com/user-attachments/assets/c5761937-fa1e-4f29-8358-984563547f30" />

Here we can see 2 files; fsocity.dic and key-1-of-3.txt.

We can then open the key-1-of-3.txt by going to http://10.38.1.111/key-1-of-3.txt.
Aha!!! Got our first flag.

<img width="951" height="875" alt="VirtualBox_kali-linux-2025 1c-virtualbox-amd64_11_08_2025_12_54_49" src="https://github.com/user-attachments/assets/3e2f1105-1484-4dfa-8479-2b8a994476d1" />

---

## Step 3 (Bruteforcing)

Remember the fsocity.dic file download it and save it in your desired directory. I personally created a new mrrobot directory and store it there as follows;

```
(kali@ kali)-[~/mrrobot]
$ wget http://10.38.1.111/fsocity.dic
-- 2025-08-11 13:12:56 -- http://10.38.1.111/fsocity.dic
Connecting to 10.38.1.111:80 ... connected.
HTTP request sent, awaiting response ... 200 OK
Length: 7245381 (6.9M) [text/x-c]
Saving to: 'fsocity.dic'
fsocity.dic  100%     6.91M 18.3MB/s    in 0.4s

2025-08-11 13:12:56 (18.3 MB/s) - 'fsocity.dic' saved [7245381/7245381]
```
We can then open the fsocity.dic file using the cat command as follows;
```
(kali@ kali)-[~/mrrobot]
$ cat fsocity.dic
```
This produces a lot of output of random words that we can use to bruteforce the website.
We can then use word count to know the exact number of words in the file.
```
(kali@ kali)-[~/mrrobot]
$ wc -l fsocity.dic
858160 fsocity.dic
```











