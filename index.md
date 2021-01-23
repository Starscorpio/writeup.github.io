# HTB-Writeups

## Lame


   ![image of bashed](https://miro.medium.com/max/529/1*_2A-7FgpNOMcLenwQqqX7g.png)


### Enumeration (Information Gathering):
Starting off with an nmap scan to check which ports are open:
1. 21 - ftp
2. 22 - ssh
3. 139, 445 - smb (netbios)

After doing a full port scan using:
`nmap -sC -sV -oA nmap/lamefull 10.10.10.3 --max-retries 0`, we get:


![nmap scan](/assets/Lamenmap.png)



Ports open after full port scan:
1. 21 - ftp
2. 22 - ssh
3. 139, 445 - smb

As we can see that port 21/ftp is open, we can try and connect to it via ftp:
`ftp 10.10.10.3`

Port 3432 distccd has a remote command execution.
There is an nmap script avaiable for this and you can find it here:
[port 3432 rce nmap script](https://nmap.org/nsedoc/scripts/distcc-cve2004-2687.html)



**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Starscorpio/writeup.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
