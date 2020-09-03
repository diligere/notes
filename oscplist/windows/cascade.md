# cascade



## nmap



PORT      STATE SERVICE       VERSION
53/tcp    open  domain        Microsoft DNS 6.1.7601 (1DB15D39) (Windows Server 2008 R2 SP1)
| dns-nsid: 
|_  bind.version: Microsoft DNS 6.1.7601 (1DB15D39)
88/tcp    open  kerberos-sec  Microsoft Windows Kerberos (server time: 2020-09-03 09:00:43Z)
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp   open  ldap          Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds?
636/tcp   open  tcpwrapped
3268/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: cascade.local, Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped
49154/tcp open  msrpc         Microsoft Windows RPC
49155/tcp open  msrpc         Microsoft Windows RPC
49157/tcp open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
49158/tcp open  msrpc         Microsoft Windows RPC
49165/tcp open  msrpc         Microsoft Windows RPC



- looks like a domain controller with port 53 88 and 389 open



## domain enumeration

#### rpcclient

![image-20200903150158499](cascade.assets/image-20200903150158499.png)



#### ldapsearch

- ldapsearch to search for something interesting

```bash
ldapsearch -x -b "dc=cascade,dc=local" -h 10.10.10.182 '(objectclass=person)'
```

![image-20200903150319485](cascade.assets/image-20200903150319485.png)



- we filter for unique things

```bash
cat ldapquery| awk '{print $1}' | sort | uniq -c | sort -nr | grep ":"
```

![image-20200903150424354](cascade.assets/image-20200903150424354.png)

- we have cascade legacy password lets see what is it

- we got a base64 encoded password and a sAMAccountName

![image-20200903150616596](cascade.assets/image-20200903150616596.png)



- we cannot  use remote session but we can get into shares 

![image-20200903151328026](cascade.assets/image-20200903151328026.png)



## shares

```bash
sudo mount -t cifs - "user=r.thompson" \\\\10.10.10.182\\Data ./mntdata
```

- mounting share to a directory mntdata

![image-20200903151939997](cascade.assets/image-20200903151939997.png)



#### note

- got a mail saying they created a temporary administrator for network migration
- and account will be deleted in 2018

![image-20200903152555686](cascade.assets/image-20200903152555686.png)

- there AD recycle bin logs show a deleted account TempAdmin

![image-20200903152827362](cascade.assets/image-20200903152827362.png)



#### vnc.reg

- found a encrypted hex password so we need to decrypt it as well
- here is a github link that tells us how to decrypt vnc registry password

![image-20200903153023017](cascade.assets/image-20200903153023017.png)

- got the password

![image-20200903154159252](cascade.assets/image-20200903154159252.png)



#### credentials

s.smith					sT333ve2

#### shell

- got a usershell using these credentials

![image-20200903154416618](cascade.assets/image-20200903154416618.png)





## post

- we can access a new share AUDIT$
- we are member of a group called audit share 

![image-20200903155710381](cascade.assets/image-20200903155710381.png)

- lots of things are in this share , mount it to see more information

![image-20200903160202495](cascade.assets/image-20200903160202495.png)



- this looks like windows program lets use windows to analyze it
- going to create a smbshare and access it from there

![image-20200903160848944](cascade.assets/image-20200903160848944.png)

- dumping the database we know its the user ArkSvc and one encrypted password

![image-20200903163600717](cascade.assets/image-20200903163600717.png)

#### dnSpy

- using dnspy we put a breakpoint where it calls for the decrypt string and debugged it to get password

- we got a password

  ​			"w3lc0meFr31nd"

![image-20200903163134465](cascade.assets/image-20200903163134465.png)



#### credentials

ArkSvc									w3lc0meFr31nd

#### shell

![image-20200903164054677](cascade.assets/image-20200903164054677.png)



#### AD recycle bin

![image-20200903164208611](cascade.assets/image-20200903164208611.png)

- we are part of AD recycle bin group
- this means we can check for deleted accounts and users in the domain



```powershell
Get-ADObject -ldapFilter:"(msDS-LastKnownRDN=*)" -IncludeDeletedObjects -property *
```

- we found tempadmin there 
- it also has cascade legacy password that is base64 encoded

![image-20200903165039496](cascade.assets/image-20200903165039496.png)

- we got password for tempadmin
- we know its the password of normal administrator user so we can try this password with normal user



#### root

- we are root

![image-20200903165427773](cascade.assets/image-20200903165427773.png)



- we can forge a golden ticket with krbtgt hash and access any resources

![image-20200903170020424](cascade.assets/image-20200903170020424.png)