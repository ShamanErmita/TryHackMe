<p align="center">
  <img src="https://github.com/user-attachments/assets/af6e43e2-d3a4-4768-9493-74f92424b21e" alt="image" width="200"/>
</p>
<h3 align="center">Pickle Rick</h3>
<p align="center">A Rick and Morty CTF. Help turn Rick back into a human!</p>

## Port Scanning
#### Command
```
nmap -sC -A 10.10.193.242
```
#### Command Breakdown
| Parameter   | Description                           |
| :---------- | :---------------------------------- |
| `-sC` | Scans with default NSE scripts |
| `-A` | Does an extensive scan on these ports |

#### Result
```
Starting Nmap 7.80 ( https://nmap.org ) at 2025-02-12 14:06 GMT
Nmap scan report for 10.10.193.242
Host is up (0.00030s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.11 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Rick is sup4r cool
MAC Address: 02:BD:A5:E2:17:CD (Unknown)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=2/12%OT=22%CT=1%CU=37423%PV=Y%DS=1%DC=D%G=Y%M=02BDA5%T
OS:M=67ACAAE2%P=x86_64-pc-linux-gnu)SEQ(SP=FF%GCD=1%ISR=10A%TI=Z%CI=Z%II=I%
OS:TS=A)OPS(O1=M2301ST11NW7%O2=M2301ST11NW7%O3=M2301NNT11NW7%O4=M2301ST11NW
OS:7%O5=M2301ST11NW7%O6=M2301ST11)WIN(W1=F4B3%W2=F4B3%W3=F4B3%W4=F4B3%W5=F4
OS:B3%W6=F4B3)ECN(R=Y%DF=Y%T=40%W=F507%O=M2301NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=
OS:40%S=O%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%
OS:O=%RD=0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=4
OS:0%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%
OS:Q=)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=
OS:Y%DFI=N%T=40%CD=S)

Network Distance: 1 hop
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE
HOP RTT     ADDRESS
1   0.30 ms 10.10.193.242

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.86 seconds
```
As we can see there is an http port open (port 80) which means this machine is using a webservice of some sort (website)

## Searching the Website
![image](https://github.com/user-attachments/assets/3bece0ef-5cbe-4c7b-aa09-c550b784dfac)

Looking at the source code of the website we can find a username as a comment
```
<!DOCTYPE html>
<html lang="en">
<head>
  <title>Rick is sup4r cool</title>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="assets/bootstrap.min.css">
  <script src="assets/jquery.min.js"></script>
  <script src="assets/bootstrap.min.js"></script>
  <style>
  .jumbotron {
    background-image: url("assets/rickandmorty.jpeg");
    background-size: cover;
    height: 340px;
  }
  </style>
</head>
<body>

  <div class="container">
    <div class="jumbotron"></div>
    <h1>Help Morty!</h1></br>
    <p>Listen Morty... I need your help, I've turned myself into a pickle again and this time I can't change back!</p></br>
    <p>I need you to <b>*BURRRP*</b>....Morty, logon to my computer and find the last three secret ingredients to finish my pickle-reverse potion. The only problem is,
    I have no idea what the <b>*BURRRRRRRRP*</b>, password was! Help Morty, Help!</p></br>
  </div>

  <!--

    Note to self, remember username!

    Username: R1ckRul3s

  -->

</body>
</html>
```
## Finding Hidden Directories
#### Command
```
gobuster dir -u http://10.10.193.242 -w Tools/wordlists/dirb/common.txt -x php,sh, txt,cgi,html,css,js,py
```
#### Command Breakdown
| Parameter   | Description                           |
| :---------- | :---------------------------------- |
| `dir` | Tells the mode so that the tool looks for directories and files on a website |
| `-u` | Specifies the target URL |
| `-w` | Specifies the wordlist to use for brute-forcing |
| `-x` | Specifies file extensions to append to each word in the wordlist when making requests |

#### Result
```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.193.242
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                Tools/wordlists/dirb/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.6
[+] Extensions:              ,php,sh
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.php                 (Status: 403) [Size: 278]
/.                    (Status: 200) [Size: 1062]
/.hta.php             (Status: 403) [Size: 278]
/.hta.sh              (Status: 403) [Size: 278]
/.hta                 (Status: 403) [Size: 278]
/.htaccess.sh         (Status: 403) [Size: 278]
/.htaccess            (Status: 403) [Size: 278]
/.htaccess.php        (Status: 403) [Size: 278]
/.htpasswd            (Status: 403) [Size: 278]
/.htaccess.           (Status: 403) [Size: 278]
/.htpasswd.sh         (Status: 403) [Size: 278]
/.htpasswd.php        (Status: 403) [Size: 278]
/.htpasswd.           (Status: 403) [Size: 278]
/.hta.                (Status: 403) [Size: 278]
/assets               (Status: 301) [Size: 315] [--> http://10.10.193.242/assets/]
/denied.php           (Status: 302) [Size: 0] [--> /login.php]
/index.html           (Status: 200) [Size: 1062]
/login.php            (Status: 200) [Size: 882]
/portal.php           (Status: 302) [Size: 0] [--> /login.php]
/robots.txt           (Status: 200) [Size: 17]
/server-status        (Status: 403) [Size: 278]
Progress: 18456 / 18460 (99.98%)
===============================================================
Finished
===============================================================
```
## Checking the Hidden Website Pages
Checking http://10.10.193.242/robots.txt we can find the password: Wubbalubbadubdub

Now going to http://10.10.193.242/login.php we can login using the username and password previously found

![image](https://github.com/user-attachments/assets/b27d413d-08b4-4c7c-8058-8da327bda376)

Once we login we get prompted with a Command Panel

![image](https://github.com/user-attachments/assets/2cd691c9-544b-42b4-9104-862e39382754)

Using ```ls``` shows us the contents we previously found and 2 new text files

![image](https://github.com/user-attachments/assets/7689a11d-3e5e-45be-942b-4b2a72fcc454)

Going to http://10.10.193.242/Sup3rS3cretPickl3Ingred.txt we find the 1st Ingredient

Now going to http://10.10.193.242/clue.txt tells us to look around the file system to find the other 2 ingredients

Using ```ls /home``` we find a directory called rick, accessing that directory using ```ls /home/rick``` we find a file called "second ingredients" which if we use the command ```less '/home/rick/second ingredients'``` we will get the 2nd Ingredient as the output (we use the command ```less``` because there are some banned commands we cannot use in this command panel like for example ```cat```)

Now trying to access the /root folder instead of the home one using ```ls /root``` we get nothing as the output, probably meaning we don't have priviliges

Using the command ```sudo -l``` we were able to elevate our user priviliges and now we can use commands we didn't had priviliges to use and we only need to write "sudo " before the command, so doing ``` sudo ls /root``` shows us these files

![image](https://github.com/user-attachments/assets/36d02c06-320a-47e3-b23c-ec124e733e7c)

Now doing ```sudo less /root/3rd.txt``` we find the 3rd and last ingredient as the output

