# Set



## nmap



#### service version scan

PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
443/tcp   open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
445/tcp   open  microsoft-ds?
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
49666/tcp open  msrpc         Microsoft Windows RPC



#### smb enumeration

443/tcp   open  ssl/http      Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
| ssl-cert: Subject: commonName=set.windcorp.thm
| Subject Alternative Name: DNS:set.windcorp.thm, DNS:seth.windcorp.thm
| Not valid before: 2020-06-07T15:00:22
|_Not valid after:  2036-10-07T15:10:21
|_ssl-date: 2020-08-15T13:08:52+00:00; +4s from scanner time.
| tls-alpn: 
|_  http/1.1





