# ironcorp



## nmap

PORT      STATE SERVICE       VERSION                                                                                                                                    
53/tcp    open  domain?                                                                                                                                                  
| fingerprint-strings:                                                                                                                                                   
|   DNSVersionBindReqTCP:                                                                                                                                                
|     version                                                                                                                                                            
|_    bind                                                                                                                                                               
135/tcp   open  msrpc         Microsoft Windows RPC                                                                                                                      
3389/tcp  open  ms-wbt-server Microsoft Terminal Services                                                                                                                
| rdp-ntlm-info:                                                                                                                                                         
|   Target_Name: WIN-8VMBKF3G815
|   NetBIOS_Domain_Name: WIN-8VMBKF3G815
|   NetBIOS_Computer_Name: WIN-8VMBKF3G815 
|   DNS_Domain_Name: WIN-8VMBKF3G815
|   DNS_Computer_Name: WIN-8VMBKF3G815
|   Product_Version: 10.0.14393
|_  System_Time: 2020-08-13T14:23:12+00:00 
| ssl-cert: Subject: commonName=WIN-8VMBKF3G815
| Not valid before: 2020-04-12T18:27:11
|_Not valid after:  2020-10-12T18:27:11
|_ssl-date: 2020-08-13T14:23:25+00:00; +2s from scanner time.
8080/tcp  open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Microsoft-IIS/10.0
|_http-title: Dashtreme Admin - Free Dashboard for Bootstrap 4 by Codervent
11025/tcp open  http          Apache httpd 2.4.41 ((Win64) OpenSSL/1.1.1c PHP/7.4.4) 
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.41 (Win64) OpenSSL/1.1.1c PHP/7.4.4
|_http-title: Coming Soon - Start Bootstrap Theme
49666/tcp open  msrpc         Microsoft Windows RPC





## http enumeration

