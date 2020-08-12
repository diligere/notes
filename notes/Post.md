# Post

## Powerview



#### loading powerview

![image-20200812194632942](Post.assets/image-20200812194632942.png)

#### 

#### admin groups

![image-20200812194905491](Post.assets/image-20200812194905491.png)





#### Share Finder

![image-20200812195011839](Post.assets/image-20200812195011839.png)



#### operating system

![image-20200812195227107](Post.assets/image-20200812195227107.png)





## BloodHound



#### SharpHound

- It is used to collect information about domain 
- this is a powershell script 
- it is need to run locally on machine and it will create a zip file which we will collect

```powershell
Invoke-BloodHound -CollectionMethod All -Domain controller.local -ZipFilename loot.zip
```



![image-20200812200021630](Post.assets/image-20200812200021630.png)



now we will collect zip file and run it on bloodhound tool in kali

![image-20200812201052625](Post.assets/image-20200812201052625.png)



#### neo4j

```
sudo neo4j console
bloodhound
```

![image-20200812201328626](Post.assets/image-20200812201328626.png)



- we need to import the file inside the application for analysis

![image-20200812201434179](Post.assets/image-20200812201434179.png)

now we can run various given queries to get our results



