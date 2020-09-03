# secnotes



## nmap



PORT    STATE SERVICE      VERSION                                                                                                                                       
80/tcp  open  http         Microsoft IIS httpd 10.0                                                                                                                      
| http-methods:                                                                                                                                                          
|_  Potentially risky methods: TRACE                                                                                                                                     
|_http-server-header: Microsoft-IIS/10.0                                                                                                                                 
| http-title: Secure Notes - Login                                                                                                                                       
|_Requested resource was login.php                                                                                                                                       
445/tcp open  microsoft-ds Windows 10 Enterprise 17134 microsoft-ds (workgroup: HTB)                                                                                     



windows 10 17134 					====>  			 windows 10 1803 version



## httpenumeration



- no default credentials on the website
- we can signup to the site and create notes 
- potential username found

![image-20200902201327237](secnotes.assets/image-20200902201327237.png)

username 

​	tyler				or 				tyler@secnotes.htb



![image-20200902201518348](secnotes.assets/image-20200902201518348.png)

- we have a html code injection.

- we can change the password by a simple get request
- we can send a link to the admin and force him to change the password through this link

![image-20200902201908584](secnotes.assets/image-20200902201908584.png)



- admin is responding to our links
- we can simply give it a link to change password to whatever we want

![image-20200902202537702](secnotes.assets/image-20200902202537702.png)



- send a malicious payload to change his password

![image-20200902203138479](secnotes.assets/image-20200902203138479.png)



## shell

- logged in as tyler

![image-20200902203501309](secnotes.assets/image-20200902203501309.png)



- we see a share we can log into with a username and password

#### credentials

username								tyler

password								92g!mA8BGjOirkL%OG*&



#### nmap

PORT     STATE SERVICE
80/tcp   open  http
445/tcp  open  microsoft-ds
8808/tcp open  ssports-bcast

- also found a new port at 8808



#### crackmapexec

![image-20200902203854197](secnotes.assets/image-20200902203854197.png)

- we have read and write access to one of the shares names as new-site
- we can create a malicious payload and execute it



#### code execution

![image-20200902204920018](secnotes.assets/image-20200902204920018.png)



![image-20200902204941047](secnotes.assets/image-20200902204941047.png)



- we got a shell nishang powershell code

![image-20200902205904724](secnotes.assets/image-20200902205904724.png)



## post

- there is ubuntu installed on windows
- we can access ubuntu file structure and all its subdirectories are installed in a particular folder
- there is a artcile on how to access it all [link on accessing ubuntu folders and file in windows](https://www.howtogeek.com/261383/how-to-access-your-ubuntu-bash-files-in-windows-and-your-windows-system-drive-in-bash/)

![image-20200902211725808](secnotes.assets/image-20200902211725808.png)

- we found username and password in .bash_history file



#### credentials



administrator

u6!4ZwgwOM#^OBf#Nwnh



#### root

![image-20200902213320798](secnotes.assets/image-20200902213320798.png)



- we can do secretsdump to dump all the hashes
- its useful in active directory as we can forge a golden ticket with krbtgt's hash

![image-20200902214023820](secnotes.assets/image-20200902214023820.png)