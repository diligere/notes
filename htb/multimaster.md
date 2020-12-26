# multimaster



### nmap

```
PORT     STATE SERVICE       VERSION                                                                                                                  
53/tcp   open  domain?                                                                                      | fingerprint-strings:                                                                                     |   DNSVersionBindReqTCP:                                                                                   |     version                                                                                               |_    bind                                                                                                 80/tcp   open  http          Microsoft IIS httpd 10.0                                                       | http-methods:                                                                                             |_  Potentially risky methods: TRACE                                                                       |_http-server-header: Microsoft-IIS/10.0                                                                   |_http-title: 403 - Forbidden: Access is denied.                                                           88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-09-21 11:58:55Z)                 135/tcp  open  msrpc         Microsoft Windows RPC                                                         139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn                                                 389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGACORP.LOCAL, Site: Default-First-Site-Name)                          
445/tcp  open  microsoft-ds  Windows Server 2016 Standard 14393 microsoft-ds (workgroup: MEGACORP)         464/tcp  open  kpasswd5?                                                                                   593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0                                           636/tcp  open  tcpwrapped                                                                                   3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: MEGACORP.LOCAL, Site: Default-First-Site-Name)                          
3269/tcp open  tcpwrapped                                                                                   3389/tcp open  ms-wbt-server Microsoft Terminal Services       
```



### http

1.  found few employees from website search feature

![image-20200921221642152](/home/manish/.config/Typora/typora-user-images/image-20200921221642152.png)



2. used jq for beautifying json file

![image-20200921221723498](/home/manish/.config/Typora/typora-user-images/image-20200921221723498.png)



3. grepped all the emails and usernames

```
cat employees | jq | grep email | awk -F "\"" '{print $4}' | tee emails  
cat employees | jq | grep email | awk -F "\"" '{print $4}' | awk -F "@" '{print $1}' | tee user
```

![image-20200921221941613](/home/manish/.config/Typora/typora-user-images/image-20200921221941613.png)



### sql injection

1. there is a sql injection on the site but it need character unicode
2. we can use sqlmap to perform it but we will try manual method

![image-20200922092232555](multimaster.assets/image-20200922092232555.png)





3. now we can perform sqli injection with the help requests module from commandline

![image-20200922092914569](multimaster.assets/image-20200922092914569.png)





4. we were able to extract username and password.

   - it is a nested select statement

   ```
   (SELECT STRING_AGG(name,',') FROM Hub_DB..sysobjects WHERE xtype = 'U')
   (SELECT STRING_AGG(username,',') FROM logins)
   (SELECT STRING_AGG(password,',') FROM logins)
   ```

   

![image-20200922123829954](multimaster.assets/image-20200922123829954.png)



#### hash

using hashcat to crack these password hashes its a 96 char hash

![image-20200922124417996](multimaster.assets/image-20200922124417996.png)

![image-20200922124516490](multimaster.assets/image-20200922124516490.png)

1. Keccak-384 hashtype got cracked and we got 3 passwords
2. we can use this for password spray

![image-20200922124631411](multimaster.assets/image-20200922124631411.png)



#### SID BRUTE

3. not a single success

![image-20200922124941426](multimaster.assets/image-20200922124941426.png)



4. we need to brute force rid to get other usernames using mssql injection

```
master.dbo.fn_varbintohexstr(SUSER_SID('MEGACORP\Administrator'))
```

![image-20200922125629456](multimaster.assets/image-20200922125629456.png)



5. can use this information to get all the username by querying there sid

![image-20200922130225272](multimaster.assets/image-20200922130225272.png)

-   using struct to convert rid to little endian
- we already got sid by querying the mssql server we just need add last 4 hex bytes that is rid
- we will increment it by 1 each time



![image-20200922130517321](multimaster.assets/image-20200922130517321.png)

6. we got some extra users now  lets try to crackmapexec with these users

![image-20200922130602678](multimaster.assets/image-20200922130602678.png)



7. got a match and we can also get a shell with evil-winrm

![image-20200922131036025](multimaster.assets/image-20200922131036025.png)



### shell



1. got shell as tushikikatomo
2. enumerating with winPEAS.exe
3. did not get anything useful

![image-20200922153845291](multimaster.assets/image-20200922153845291.png)



4. there are random ports opening and closing in localhost

![image-20200922154055759](multimaster.assets/image-20200922154055759.png)

5. further enumeration shows it is visual code debug ports

![image-20200922154145068](multimaster.assets/image-20200922154145068.png)



6. we will use cefdebug to run arbitrary code as that process
7. we have code execution

![image-20200922155350772](multimaster.assets/image-20200922155350772.png)



8. now we can get a reverseshell

![image-20200922162330394](multimaster.assets/image-20200922162330394.png)

now we are megacorp\cyork

9. running as cyork we find that we can read inetpub directory 
10. we find an interesting dll **MultimasterAPI.dll** we copy to local machine to analyze further

![image-20200922162753255](multimaster.assets/image-20200922162753255.png)

11. using strings in utf16 little endian format we get credentials

![image-20200922163012023](multimaster.assets/image-20200922163012023.png)



#### credentials

```
uid=finder password=D3veL0pM3nT!
```



#### crackmapexec

![image-20200922163350585](multimaster.assets/image-20200922163350585.png)

- got sbauer user lets login as that user





### bloodhound

1. logging in as sbauer user lets run bloodhound

![image-20200922173251920](multimaster.assets/image-20200922173251920.png)

2. we have generic write over jorden that means we can make it kerberostable by setting a spn
3. we will use evil-winrm inbuilt feature to bypass AMSI

![image-20200922173355826](multimaster.assets/image-20200922173355826.png)

4. then we can click on abuse for details

![image-20200922173417383](multimaster.assets/image-20200922173417383.png)



5. once we set a spn we can request for its ticket

![image-20200922173440038](multimaster.assets/image-20200922173440038.png)



6. we will crack it using hashcat

![image-20200922173743142](multimaster.assets/image-20200922173743142.png)

```
jorden		rainforest786
```



## weak service permissions

1. now we will login as jordon to see what privileges we have
2. we have generic write permission over all the services

![image-20200922180036755](multimaster.assets/image-20200922180036755.png)



3. means we can change service paths running as localsystem.

![image-20200922200002937](multimaster.assets/image-20200922200002937.png)