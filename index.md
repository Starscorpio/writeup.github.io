# HTB-Writeups

## Lame


   ![image of bashed](https://miro.medium.com/max/529/1*_2A-7FgpNOMcLenwQqqX7g.png)


### Enumeration (Information Gathering):
Starting off with an nmap scan to check which ports are open:
Ports open:
1. 21/ftp
2. 22/ssh
3. 139, 445/smb (netbios)

After doing a full port scan using `nmap -sC -sV -oA nmap/lamefull 10.10.10.3 --max-retries 0`, we get:


![nmap scan](/assets/Lamenmap.png)



### Markdown
##halsdkhflakdhslfkahd;lskfha;sldkhf;lakshdf;lkahsdf

Now, check for suids which have there suid bit enabled, by running the LinEnum.sh script:-


 


-As we can see that the suid bit is enabled for nmap binary/tool.

Using nmap in interactive mode to get root:
-nmap --interactive
---Then enter:
!sh

--And we are ROOT!!!!
`uname -a`


**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Starscorpio/writeup.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
