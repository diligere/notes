# curling



## nmap



PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8a:d1:69:b4:90:20:3e:a7:b6:54:01:eb:68:30:3a:ca (RSA)
|   256 9f:0b:c2:b2:0b:ad:8f:a1:4e:0b:f6:33:79:ef:fb:43 (ECDSA)
|_  256 c1:2a:35:44:30:0c:5b:56:6a:3f:a5:cc:64:66:d9:a9 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-generator: Joomla! - Open Source Content Management
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Home
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel



## httpenumeration 

- we know its running joomla on http

![image-20200830152942954](curling.assets/image-20200830152942954.png)

- the site doesnot look vulnerable to any kind of rce or login bypass attack
- but we do get username from the homepage





- potential usernames are **Super User and Floris**

  ![image-20200830153136780](curling.assets/image-20200830153136780.png)



- source of the page tells us there is secret.txt file inside 
- it is base 64 encode we decode it and get the password for logging into joomla 
- then we can create a shell by modifying on of the template

#### shell

![image-20200830164207750](curling.assets/image-20200830164207750.png)



## xdd

- we got a hexdump of password_backup file

![image-20200830164525123](curling.assets/image-20200830164525123.png)

- after using xxd and multiple reverse techniques we got a password og floris account 



## post



-  we see a cron running as root using pspy64s

![image-20200830164629961](curling.assets/image-20200830164629961.png)

- -K is for configuration
- from man page we can see it can get input and save anywhere in the curling machine since the cron is running as root

![image-20200830164901534](curling.assets/image-20200830164901534.png)



- changed sudoers file to get access get sudo privilege on all

![image-20200830165933022](curling.assets/image-20200830165933022.png)