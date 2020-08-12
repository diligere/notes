# Fox



## nmap

PORT    STATE SERVICE     REASON  VERSION
80/tcp  open  http        syn-ack Apache httpd 2.4.29
139/tcp open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: YEAROFTHEFOX)
445/tcp open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: YEAROFTHEFOX)
Service Info: Hosts: year-of-the-fox.lan, YEAR-OF-THE-FOX



## smb



#### smb enumeration

![image-20200812211919533](Fox.assets/image-20200812211919533.png)

We find 2 shares yotf and IPC$.



## enum4linux

![image-20200812214738056](Fox.assets/image-20200812214738056.png)

got 2 usernames

- fox
- rascal



#### credentials

![image-20200812221329557](Fox.assets/image-20200812221329557.png)

rascal 	19811981





## http Enumeration



![image-20200812221526327](Fox.assets/image-20200812221526327.png)



#### command injection

we find insecure deserialisation that is user input is not sanitized properly and it ran our query



![image-20200812223007330](Fox.assets/image-20200812223007330.png)





## Exploit



![image-20200812223633120](Fox.assets/image-20200812223633120.png)

we have to use base64 encoder and then decode it when it reaches remote host



#### base64



![image-20200812223919634](Fox.assets/image-20200812223919634.png)

encoded the payload and we see it will work if  decode it and pipe it to bash



![image-20200812224049390](Fox.assets/image-20200812224049390.png)



#### shell

![image-20200812224111613](Fox.assets/image-20200812224111613.png)



## port forwarding



we see ssh bind locally lets use socat to make a tunnel and forward all the traffic to someother port

![image-20200813000235221](Fox.assets/image-20200813000235221.png)



#### socat

we will use socat for port forwarding

![image-20200813000823532](Fox.assets/image-20200813000823532.png)

```bash
./socat TCP-LISTEN:7777,fork tcp:127.0.0.1:22 &
```



verifying the service is running on port 7777

![image-20200813000941639](Fox.assets/image-20200813000941639.png)



## ssh



#### bruteforce

we will try bruteforcing ssh

![image-20200813001819895](Fox.assets/image-20200813001819895.png)

#### credentials

fox		leanne

![image-20200813002025391](Fox.assets/image-20200813002025391.png)



## Post



#### enumeration

sudo -l

we can see it has shutdown command with no passwd as root

![image-20200813002408818](Fox.assets/image-20200813002408818.png)





in the strings of shutdown command we see poweroff

![image-20200813002339352](Fox.assets/image-20200813002339352.png)





#### path manipulation



![image-20200813002919656](Fox.assets/image-20200813002919656.png)



![image-20200813003026916](Fox.assets/image-20200813003026916.png)



#### shell

![image-20200813003159266](Fox.assets/image-20200813003159266.png)

```
find -name "*root*"
```



![image-20200813003639554](Fox.assets/image-20200813003639554.png)

found the root flag