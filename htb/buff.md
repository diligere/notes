# buff



## nmap

PORT     STATE SERVICE    VERSION
7680/tcp open  pando-pub?
8080/tcp open  http       Apache httpd 2.4.43 ((Win64) OpenSSL/1.1.1g PHP/7.4.6)
| http-open-proxy: Potentially OPEN proxy.
|_Methods supported:CONNECTION
|_http-server-header: Apache/2.4.43 (Win64) OpenSSL/1.1.1g PHP/7.4.6
|_http-title: mrb3n's Bro Hut







## httpenumaration



![image-20200817170902796](buff.assets/image-20200817170902796.png)

it is running gym management software version 1.0



#### searchsploit

![image-20200817171001043](buff.assets/image-20200817171001043.png)





## exploit



![image-20200817171136036](buff.assets/image-20200817171136036.png)

got the shell



#### shell

we will try to get a meterpreter shell

uploaded meterpreter 

getting a meterpreter session

