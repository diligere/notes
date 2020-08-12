# Gatekeeper

## nmap

PORT      STATE    SERVICE            VERSION
135/tcp   open     msrpc              Microsoft Windows RPC
139/tcp   open     netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open     microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open     ssl/ms-wbt-server?
|_ssl-date: 2020-08-12T13:29:34+00:00; +2s from scanner time.
3925/tcp  filtered zmp
5005/tcp  filtered avt-profile-2
31337/tcp open     Elite?

Host script results:
|_clock-skew: mean: 1h00m02s, deviation: 2h00m00s, median: 1s
|_nbstat: NetBIOS name: GATEKEEPER, NetBIOS user: <unknown>, NetBIOS MAC: 02:c3:73:ed:0c:d9 (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: gatekeeper
|   NetBIOS computer name: GATEKEEPER\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2020-08-12T09:29:27-04:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2020-08-12T13:29:27
|_  start_date: 2020-08-12T13:24:07



## smb Enumeration

### smbclient

After enumeration we find some shares lets see with smbmap if we can see can read any.

![image-20200812191212039](Gatekeeper.assets/image-20200812191212039.png)



got file brainpain.exe

![image-20200812191736874](Gatekeeper.assets/image-20200812191736874.png)

