# Result of nmap scans

## Normal Nmap scan

``` bash
nmap -sCTV -oN /home/kali/HTB/scriptkiddie/nmap.txt 10.10.10.226                      
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-14 01:07 EDT
Nmap scan report for 10.10.10.226
Host is up (0.26s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3c:65:6b:c2:df:b9:9d:62:74:27:a7:b8:a9:d3:25:2c (RSA)
|   256 b9:a1:78:5d:3c:1b:25:e0:3c:ef:67:8d:71:d3:a3:ec (ECDSA)
|_  256 8b:cf:41:82:c6:ac:ef:91:80:37:7c:c9:45:11:e8:43 (ED25519)
5000/tcp open  http    Werkzeug httpd 0.16.1 (Python 3.8.5)
|_http-title: k1d'5 h4ck3r t00l5
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 37.53 seconds
```

## vuln script nmap scan

 ``` bash
 nmap --script vuln 10.10.10.226                                                 
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-14 01:07 EDT
Nmap scan report for 10.10.10.226
Host is up (0.26s latency).
Not shown: 998 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
5000/tcp open  upnp

Nmap done: 1 IP address (1 host up) scanned in 16.63 seconds
```

## complete tcp port scan

``` bash 
nmap -sCV -p- -oN /home/kali/HTB/scriptkiddie/nmap_complete.txt 10.10.10.226    
Starting Nmap 7.91 ( https://nmap.org ) at 2021-06-14 01:08 EDT
Nmap scan report for 10.10.10.226
Host is up (0.26s latency).
Not shown: 65533 closed ports
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3c:65:6b:c2:df:b9:9d:62:74:27:a7:b8:a9:d3:25:2c (RSA)
|   256 b9:a1:78:5d:3c:1b:25:e0:3c:ef:67:8d:71:d3:a3:ec (ECDSA)
|_  256 8b:cf:41:82:c6:ac:ef:91:80:37:7c:c9:45:11:e8:43 (ED25519)
5000/tcp open  http    Werkzeug httpd 0.16.1 (Python 3.8.5)
|_http-title: k1d'5 h4ck3r t00l5
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 1502.51 seconds
```

### No new ports are open so we have to work on the open ones

