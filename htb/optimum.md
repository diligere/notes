# optimum



## nmap

Nmap scan report for 10.10.10.8
Host is up (0.17s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE VERSION
80/tcp open  http    HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows



## vulnerability



![image-20200821230715348](optimum.assets/image-20200821230715348.png)

rejetto version 2.3 is running

#### searchsploit



![image-20200822091020123](optimum.assets/image-20200822091020123.png)



## exploit

- we need to host a netcat listener for reverse connection 
- change local ip address and port number in file

![image-20200822091209615](optimum.assets/image-20200822091209615.png)



#### shell

![image-20200822091710681](optimum.assets/image-20200822091710681.png)





## post

- We see SeChangeNotifyPrivilege on.
- after some research found a powershell script sherlock



![image-20200822093522495](optimum.assets/image-20200822093522495.png)



- loaded sherlock in powershell
- and run Find-AllVulns

![image-20200822094357963](optimum.assets/image-20200822094357963.png)



- one privilege escalation script looked promising

![image-20200822094536173](optimum.assets/image-20200822094536173.png)



[SeChangeNotifyPrivilege Escalation script](https://github.com/FuzzySecurity/PSKernel-Primitives/tree/master/Sample-Exploits/MS16-135)

- run the script and get NT Authority

![image-20200822094840915](optimum.assets/image-20200822094840915.png)



