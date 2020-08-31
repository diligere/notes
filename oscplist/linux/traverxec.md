# traverxec



## nmap



PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.9p1 Debian 10+deb10u1 (protocol 2.0)
| ssh-hostkey: 
|   2048 aa:99:a8:16:68:cd:41:cc:f9:6c:84:01:c7:59:09:5c (RSA)
|   256 93:dd:1a:23:ee:d7:1f:08:6b:58:47:09:73:a3:88:cc (ECDSA)
|_  256 9d:d6:62:1e:7a:fb:8f:56:92:e6:37:f1:10:db:9b:ce (ED25519)
80/tcp open  http    nostromo 1.9.6
|_http-server-header: nostromo 1.9.6
|_http-title: TRAVERXEC
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel



## searchsploit



![image-20200828155830703](traverxec.assets/image-20200828155830703.png)

- we see a remote code execution of nostromo 1.9.6
- we can use it to get initial shell

![image-20200828155909271](traverxec.assets/image-20200828155909271.png)



#### command execution

![image-20200828160240487](traverxec.assets/image-20200828160240487.png)



#### base64

- using base64 encoded the command and then send it as arguement
- then decode it and passed it bash
- did this avoid any badcharacters problem terminating the request

![image-20200828163813597](traverxec.assets/image-20200828163813597.png)

![image-20200828163843292](/home/manish/.config/Typora/typora-user-images/image-20200828163843292.png)



- using linpeas found a interesting file
- it had a md5crypt encrypted password hash

![image-20200828164120359](traverxec.assets/image-20200828164120359.png)



#### hashcat

- it a md5crypt password hash
- cracked it using hashcat

![image-20200828164214961](traverxec.assets/image-20200828164214961.png)



#### credentials

~~username 		david~~

~~password		 Nowonly4me~~

- cannot use those creds anywhere



- found a directory inside david that is world readable 
- we found it configuration file
- there is ssh private key hidden in it
- we will crack it using john and get paraphrase

![image-20200828180154802](traverxec.assets/image-20200828180154802.png)

- got the shell as root



## post

- there bash script in which we can see something running as root

![image-20200828180256395](traverxec.assets/image-20200828180256395.png)

- sudo doestnot allow anything after pipe to run as root
- even if we remove /usr/bin/cat it will still work

![image-20200828180439870](traverxec.assets/image-20200828180439870.png)