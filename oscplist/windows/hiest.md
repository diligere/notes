# hiest



## nmap



PORT    STATE SERVICE       VERSION
80/tcp  open  http          Microsoft IIS httpd 10.0
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
| http-title: Support Login Page
|_Requested resource was login.php
135/tcp open  msrpc         Microsoft Windows RPC
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 14s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   datPORT    STATE SERVICE       VERSION
80/tcp  open  http          Microsoft IIS httpd 10.0
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
| http-title: Support Login Page
|_Requested resource was login.php
135/tcp open  msrpc         Microsoft Windows RPC
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 14s
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-08-25T14:20:31
|_  start_date: N1 md5 cisco password/Ae: 2020-08-25T14:20:31
|_  start_date: N/A



## httpenumeration

- from the webpage we logged in as guest 
- and downloaded a config.txt file
- enumerating the file we get **2 password of cisco 7**  and **1 md5 cisco password** 
- using a ciscot7 tool we cracked the password of cisco 

![image-20200825230626464](hiest.assets/image-20200825230626464.png)





#### passwords

- this is what password file looks like

![image-20200825230822221](hiest.assets/image-20200825230822221.png)





## crackmapexec

- we got one valid hit 
- but we have not access to smb

![image-20200825231057276](hiest.assets/image-20200825231057276.png)



- we can use this credentials to get into domain and enumerate other usersimage-20200825231057276



### rpcclient

- using rpcclient we can enumerate other users but we need sid
- we already know one username and password we can get his sid and bruteforce sids

![image-20200825231900803](hiest.assets/image-20200825231900803.png)

- this hazards sid
- now we can brute force other users sid by changing last 4 digit we can verify it by typing 500 which is default to administrator

![image-20200825232121514](hiest.assets/image-20200825232121514.png)



### impacket lookupsids

- we got so many new users we can also use impacket tools for this

![image-20200825232811040](hiest.assets/image-20200825232811040.png)

- we can add this to our usernames and see if anything matches using crackmapexec



### shell

- using crackmapexec we got a shell because we use ps-session to login to the desktop

![image-20200825233454873](hiest.assets/image-20200825233454873.png)



**credentials**

chase		:		Q4)sJu\Y8qz*A3?d

- we got the shell

![image-20200825234112397](hiest.assets/image-20200825234112397.png)





## post

- we see mozilla firefox is installed
- we can try to dump the credentials from its memory

![image-20200825234306955](hiest.assets/image-20200825234306955.png)



#### powershell

- we can dump firefox from main memory
- and check for any passwords 
- we will use sysinternals tool **procdump64**

![image-20200825234928275](hiest.assets/image-20200825234928275.png)



#### sysinternals

- we will use procdump to dump a process from memory

```
procdump -ma pid firefox.dmp
```

- this will dump firefox from main memory to hard drive and we can analyse it
- then we will use strings to get information about anything realted to password

neonlogin_username=admin%40aadmin&login_password=admin&login=

neonneon![image-20200826093809989](hiest.assets/image-20200826093809989.png)

- we can use strings to get grep for password



#### Invoke-Mimikittenz

- this is a useful tool for search for a something in memory with creating dump
- it uses windows api ReadProcessMemory()
- we can use it and give a custom regex to search for our strings

![image-20200826105405602](hiest.assets/image-20200826105405602.png)

- we use regex to match for the login pattern
- and we find a match



![image-20200826105454969](hiest.assets/image-20200826105454969.png)

#### credentials

username = admin

password = 4dD!5}x/re8]FBuZ



![image-20200826105857582](hiest.assets/image-20200826105857582.png)