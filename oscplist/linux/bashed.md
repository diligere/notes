# bashed



## nmap



PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Arrexel's Development Site





## gobuster



/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/css (Status: 301)
/dev (Status: 301)
/fonts (Status: 301)
/images (Status: 301)
/js (Status: 301)
/php (Status: 301)
/server-status (Status: 403)
/uploads (Status: 301)

- dev folder looks internesting
- found a php bashic shell installed already
- we can use it to get a shell



![image-20200824115637605](/home/manish/.config/Typora/typora-user-images/image-20200824115637605.png)





## shell



- uploaded rev.php in /dev/shm
- executed it with php and got the shell



![image-20200824120423880](bashed.assets/image-20200824120423880.png)



shell

![image-20200824120358683](bashed.assets/image-20200824120358683.png)





## post

- sudo -l we c![image-20200824121636496](bashed.assets/image-20200824121636496.png)an run as scriptmanager to escalate to scriptmanager

![image-20200824121556250](bashed.assets/image-20200824121556250.png)





#### script folder

- we can see test.py is accessible by scriptmanager
- and it is execute by test because test.txt has ownership of root
- we can manipulate test.py to setuid of bash

![image-20200824121824401](bashed.assets/image-20200824121824401.png)





- successfully setting up set uid binary now we can exploit bash

![image-20200824123348305](bashed.assets/image-20200824123348305.png)



#### root

![image-20200824123519175](bashed.assets/image-20200824123519175.png)