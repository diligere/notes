# base64 encoding for windows



![image-20200918202431988](encoding4windows.assets/image-20200918202431988.png)



- windows uses utf-16
- linux uses utf-8

so we need to convert any base64 payload from utf8 to utf16 to make it work in windows